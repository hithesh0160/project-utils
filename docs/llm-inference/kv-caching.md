# KV Caching

## Summary

Key-Value (KV) caching is a fundamental optimization technique for transformer-based text generation that stores intermediate attention layer states (keys and values) during inference. Instead of recomputing attention for all previous tokens at each generation step, the model reuses cached K/V pairs, dramatically reducing computation and improving generation speed—typically 5x faster or more.

## Best For

- Autoregressive text generation (GPT-style models)
- Long-form content generation where sequences grow over time
- Interactive chat applications with multi-turn conversations
- Any scenario where tokens are generated one at a time
- Production inference servers handling real-time requests
- Memory-constrained environments where repeated computation is expensive

## Avoid When

- Batch processing of independent prompts (no sequential dependency)
- Single forward passes without generation (e.g., classification, embedding extraction)
- Memory is severely limited and cache size would exceed available RAM/VRAM
- Non-autoregressive models (e.g., BERT for encoding tasks)
- Very short generations where cache overhead exceeds computation savings

## How It Works

### The Problem: Repeated Computation

In standard autoregressive generation, each new token requires computing attention over **all previous tokens**:

```
Step 1: Generate token 1
  - Compute attention for: [input]
  
Step 2: Generate token 2
  - Compute attention for: [input, token 1]  ← Recomputes token 1
  
Step 3: Generate token 3
  - Compute attention for: [input, token 1, token 2]  ← Recomputes tokens 1 & 2
  
Step 4: Generate token 4
  - Compute attention for: [input, token 1, token 2, token 3]  ← Recomputes all
```

This results in **quadratic computation** as sequence length grows.

### The Solution: Cache Keys and Values

In transformer attention, we compute:

```
Attention(Q, K, V) = softmax(Q * K^T / √d_k) * V
```

Where:
- **Q** (Query): Depends on the current token being processed
- **K** (Key): Depends on all tokens in the sequence
- **V** (Value): Depends on all tokens in the sequence

**Key insight**: For previous tokens, K and V never change—only Q changes for the new token.

KV caching stores the K and V matrices from previous steps:

```
Step 1: Generate token 1
  - Compute Q1, K1, V1
  - Cache: [K1], [V1]
  
Step 2: Generate token 2
  - Compute Q2, K2, V2
  - Retrieve cached: [K1], [V1]
  - Concatenate: [K1, K2], [V1, V2]
  - Compute attention: Q2 * [K1, K2]^T
  - Update cache: [K1, K2], [V1, V2]
  
Step 3: Generate token 3
  - Compute Q3, K3, V3
  - Retrieve cached: [K1, K2], [V1, V2]
  - Concatenate: [K1, K2, K3], [V1, V2, V3]
  - Compute attention: Q3 * [K1, K2, K3]^T
  - Update cache: [K1, K2, K3], [V1, V2, V3]
```

### Computation Comparison

| Generation Step | Without KV Cache | With KV Cache |
|----------------|------------------|---------------|
| Token 1 | Compute Q1, K1, V1 | Compute Q1, K1, V1 + cache K1, V1 |
| Token 2 | Compute Q1, K1, V1, Q2, K2, V2 | Compute Q2, K2, V2 + retrieve cache |
| Token 3 | Compute Q1, K1, V1, Q2, K2, V2, Q3, K3, V3 | Compute Q3, K3, V3 + retrieve cache |
| Token n | Compute all n tokens | Compute only token n + retrieve cache |

**Complexity:**
- Without cache: O(n²) for n tokens
- With cache: O(n) for n tokens

## Setup

### Using Transformers Library (Default Behavior)

KV caching is **enabled by default** in Hugging Face Transformers:

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained('HuggingFaceTB/SmolLM2-1.7B')
model = AutoModelForCausalLM.from_pretrained('HuggingFaceTB/SmolLM2-1.7B').cuda()

tokens = tokenizer.encode("The red cat was", return_tensors="pt").cuda()

# KV caching enabled by default
output = model.generate(
    tokens, 
    max_new_tokens=300, 
    use_cache=True  # Default is True
)

output_text = tokenizer.batch_decode(output, skip_special_tokens=True)[0]
```

### Disabling KV Cache (for comparison)

```python
# Disable to measure impact
output_no_cache = model.generate(
    tokens, 
    max_new_tokens=300, 
    use_cache=False  # Forces recomputation at each step
)
```

### Custom Cache Implementations

Transformers supports multiple cache strategies via `cache_implementation`:

```python
from transformers import GenerationConfig

# Static cache (pre-allocated, faster but fixed size)
gen_config = GenerationConfig(
    max_new_tokens=300,
    cache_implementation="static",
    max_cache_len=2048
)
output = model.generate(tokens, generation_config=gen_config)

# Dynamic cache (default, grows as needed)
gen_config = GenerationConfig(
    max_new_tokens=300,
    cache_implementation="dynamic"
)

# Quantized cache (reduces memory, slight speed tradeoff)
gen_config = GenerationConfig(
    max_new_tokens=300,
    cache_implementation="quantized"
)
```

## Manual KV Cache Implementation

For understanding or custom use cases:

```python
import torch

class KVCache:
    def __init__(self):
        self.cache = {"key": None, "value": None}

    def update(self, key, value):
        """Add new keys and values to the cache."""
        if self.cache["key"] is None:
            # First token: initialize cache
            self.cache["key"] = key
            self.cache["value"] = value
        else:
            # Subsequent tokens: concatenate along sequence dimension
            self.cache["key"] = torch.cat([self.cache["key"], key], dim=1)
            self.cache["value"] = torch.cat([self.cache["value"], value], dim=1)

    def get_cache(self):
        """Retrieve cached keys and values."""
        return self.cache["key"], self.cache["value"]
    
    def clear(self):
        """Clear cache for new generation."""
        self.cache = {"key": None, "value": None}

# Usage in attention layer
kv_cache = KVCache()

for step in range(num_generation_steps):
    # Compute Q, K, V for current token
    query = compute_query(current_token)
    key = compute_key(current_token)
    value = compute_value(current_token)
    
    # Update cache with new K, V
    kv_cache.update(key, value)
    
    # Retrieve full cached K, V
    cached_keys, cached_values = kv_cache.get_cache()
    
    # Compute attention using current Q and all cached K, V
    attention_scores = torch.matmul(query, cached_keys.transpose(-2, -1))
    attention_probs = torch.softmax(attention_scores / math.sqrt(d_k), dim=-1)
    output = torch.matmul(attention_probs, cached_values)
```

## Performance Benchmarks

From the Hugging Face blog post (T4 GPU, 300 tokens):

| Configuration | Time | Speedup |
|---------------|------|---------|
| With KV Cache | 11.7s | Baseline |
| Without KV Cache | 1min 1s (61s) | **5.21x slower** |

### Expected Improvements

- **Short sequences (< 50 tokens)**: 2-3x speedup
- **Medium sequences (50-500 tokens)**: 5-10x speedup
- **Long sequences (500+ tokens)**: 10-20x speedup

Speedup increases with sequence length because cache benefits compound.

## Memory Considerations

### Memory Usage

KV cache stores two matrices per attention head per layer:

```
Memory per layer = 2 * num_heads * head_dim * sequence_length * batch_size * dtype_bytes
```

Example for LLaMA-2-7B (32 layers, 32 heads, 128 head_dim, fp16):
```
Per token: 2 * 32 * 32 * 128 * 2 bytes = 524 KB per layer
Full model: 524 KB * 32 layers = 16.4 MB per token
1000 tokens: ~16.4 GB
```

### Memory vs. Speed Tradeoff

| Aspect | Without KV Cache | With KV Cache |
|--------|------------------|---------------|
| Computation | High (O(n²)) | Low (O(n)) |
| Memory | Low (constant per step) | High (grows with sequence) |
| Speed | Slow, degrades with length | Fast, consistent per token |
| Use Case | Memory-critical, short sequences | Production inference, long sequences |

## Cache Management Strategies

### Static Cache (Pre-allocated)

```python
# Pre-allocate cache for maximum efficiency
gen_config = GenerationConfig(
    cache_implementation="static",
    max_cache_len=2048  # Maximum sequence length
)
```

**Pros**: Fastest, no dynamic allocation overhead  
**Cons**: Fixed size, wastes memory if sequence is shorter

### Dynamic Cache (Grows as Needed)

```python
# Default behavior
gen_config = GenerationConfig(
    cache_implementation="dynamic"
)
```

**Pros**: Memory-efficient, adapts to sequence length  
**Cons**: Slight overhead from concatenation operations

### Quantized Cache (Reduced Precision)

```python
# Reduce memory footprint
gen_config = GenerationConfig(
    cache_implementation="quantized",
    cache_dtype="int8"  # or "int4"
)
```

**Pros**: ~4-8x memory reduction  
**Cons**: Slight accuracy loss, quantization/dequantization overhead

## Multi-Layer Caching

KV caching is applied **independently to each transformer layer**:

```
Layer 1: Cache K1, V1 for all tokens
Layer 2: Cache K2, V2 for all tokens
...
Layer N: Cache KN, VN for all tokens
```

Each layer maintains its own cache because attention representations differ across layers.

## Advanced: Cache Reuse Patterns

### Prefix Caching

For repeated prompts (e.g., system messages in chat):

```python
# Cache the system prompt once, reuse for all user messages
system_prompt = "You are a helpful assistant."
system_tokens = tokenizer.encode(system_prompt, return_tensors="pt")

# Generate system prompt cache
_ = model.generate(system_tokens, max_new_tokens=1, use_cache=True)
# Cache now contains system prompt K/V

# Reuse for multiple user queries (implementation-dependent)
```

### Sliding Window Cache

For very long sequences, keep only recent tokens:

```python
# Pseudo-code for sliding window
max_cache_len = 1024
if current_cache_len > max_cache_len:
    # Drop oldest tokens from cache
    cache = cache[:, -max_cache_len:, :]
```

## Comparison with Related Techniques

| Technique | Purpose | Scope |
|-----------|---------|-------|
| **KV Caching** | Speed up generation by avoiding recomputation | Per-sequence optimization |
| **PagedAttention** | Optimize KV cache memory layout | Memory management layer on top of KV cache |
| **Flash Attention** | Optimize attention computation itself | Attention kernel optimization |
| **Quantization** | Reduce model size and memory | Model weights and activations |

**KV caching and PagedAttention are complementary**: KV caching is the concept, PagedAttention optimizes how the cache is stored in memory.

## Troubleshooting

### Out of Memory Errors

```python
# Solution 1: Reduce max length
output = model.generate(tokens, max_new_tokens=100)  # Instead of 1000

# Solution 2: Use quantized cache
gen_config = GenerationConfig(cache_implementation="quantized")

# Solution 3: Disable cache (slower but uses less memory)
output = model.generate(tokens, use_cache=False)
```

### Unexpected Slow Generation

```python
# Verify cache is enabled
output = model.generate(tokens, use_cache=True)

# Check if model supports caching
print(model.config.use_cache)  # Should be True
```

### Cache Not Updating

- Ensure you're using `model.generate()`, not manual forward passes
- For manual implementation, verify concatenation dimension is correct

## Reference

- [Hugging Face Blog: KV Caching Explained](https://huggingface.co/blog/not-lain/kv-caching)
- [Transformers Documentation: Generation Strategies](https://huggingface.co/docs/transformers/main/en/generation_strategies#kv-caching)
- [Medium: KV Caching Explained](https://medium.com/@joaolages/kv-caching-explained-276520203249)
- [Neptune.ai: Transformers Key-Value Caching](https://neptune.ai/blog/transformers-key-value-caching)
- [NVIDIA: Mastering LLM Techniques - Inference Optimization](https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/)

## Related Techniques

- **PagedAttention** (`docs/paged-attention.md`) — Memory-efficient KV cache management
- **Flash Attention** — Optimized attention kernel implementation
- **Speculative Decoding** — Generate multiple tokens in parallel using draft model

## Lessons Learned

- KV caching is enabled by default in Transformers—verify `use_cache=True` is set
- Speedup is proportional to sequence length; most dramatic for long generations
- Memory grows linearly with sequence length; monitor for OOM on long contexts
- Each transformer layer maintains its own independent KV cache
- Static cache offers best performance when max length is known in advance
- Quantized cache provides good memory/speed tradeoff for resource-constrained environments
- KV caching is essential for production inference; disabling it severely degrades performance

## Status

Status: production-ready
Default: Enabled in Transformers library
Performance: 5-20x speedup depending on sequence length
