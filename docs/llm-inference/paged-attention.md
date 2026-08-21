# Paged Attention

## Summary

PagedAttention is a memory optimization technique for LLM inference that partitions the KV (key-value) cache into blocks accessed through a lookup table. Instead of storing attention keys and values in contiguous GPU memory, blocks are allocated on-demand, improving memory efficiency and enabling higher GPU utilization for memory-bound workloads.

## Best For

- Long-context generation where KV cache grows large
- Memory-constrained GPU environments
- Parallel sampling where multiple outputs are generated for the same prompt
- Batch inference workloads that need to maximize throughput
- Production LLM serving with high concurrent request loads
- Large models where KV cache is a significant memory bottleneck

## Avoid When

- You have abundant GPU memory and memory is not a bottleneck
- Working with very short sequences where KV cache overhead is minimal
- The inference framework doesn't support PagedAttention (check compatibility)
- Latency is more critical than throughput (lookup overhead may add small latency)
- Simple single-generation use cases without batching

## How It Works

### The Problem

During LLM decoding, all attention keys and values generated for previous tokens are stored in GPU memory for reuse. This is called the **KV cache**:

- Each token in the sequence requires storing key and value vectors
- For large models (e.g., 70B parameters) and long contexts (e.g., 32k tokens), this can consume GBs of memory
- Traditional approach requires contiguous memory allocation, leading to fragmentation and inefficiency

### The Solution

PagedAttention solves this by:

1. **Partitioning**: Split KV cache into fixed-size blocks
2. **Lookup Table**: Use a table to map logical KV positions to physical memory blocks
3. **Non-contiguous Storage**: Blocks can be scattered across GPU memory
4. **On-demand Allocation**: Allocate blocks only as needed during generation
5. **Block Sharing**: Multiple generations can share KV blocks for the same prompt

### Memory Efficiency Example

```
Traditional KV Cache:
[Token1][Token2][Token3]...[TokenN]  (contiguous, pre-allocated)

PagedAttention:
Block 0: [Token1][Token2][Token3][Token4]
Block 1: [Token5][Token6][Token7][Token8]
...
Block N: [TokenX][TokenY][TokenZ][____]  (partial block, allocated on-demand)

Lookup Table: [Block0 @ 0x1000, Block1 @ 0x3000, BlockN @ 0x7000]
```

## KV Sharing for Parallel Sampling

When generating multiple outputs for the same prompt:

```
Prompt: "Explain quantum computing"

Output 1: "Quantum computing uses qubits..."
Output 2: "Quantum computing leverages..."
Output 3: "Quantum computing is based on..."

Traditional: 3x full KV cache for the prompt (wasteful)
PagedAttention: 1x shared KV cache for the prompt + separate blocks for each output
```

## Setup

### Text Generation Inference (TGI)

PagedAttention is built into TGI and uses custom CUDA kernels from the vLLM project:

```bash
# TGI with PagedAttention (enabled by default)
docker run --gpus all --shm-size 1g -p 8080:80 \
  -v $PWD/data:/data \
  ghcr.io/huggingface/text-generation-inference:latest \
  --model-id meta-llama/Llama-2-70b-chat-hf \
  --max-batch-prefill-tokens 4096 \
  --max-total-tokens 32768
```

### vLLM

vLLM is the original implementation of PagedAttention:

```bash
# Install vLLM
pip install vllm

# Run vLLM server with PagedAttention
python -m vllm.entrypoints.openai.api_server \
  --model meta-llama/Llama-2-70b-chat-hf \
  --tensor-parallel-size 4 \
  --max-model-len 32768
```

### Python API (vLLM)

```python
from vllm import LLM, SamplingParams

# Initialize LLM with PagedAttention (default)
llm = LLM(
    model="meta-llama/Llama-2-70b-chat-hf",
    tensor_parallel_size=4,
    max_model_len=32768,
)

# Parallel sampling benefits from KV sharing
sampling_params = SamplingParams(
    temperature=0.8,
    top_p=0.95,
    n=5,  # Generate 5 outputs - PagedAttention shares prompt KV cache
)

prompts = ["Explain quantum computing in simple terms."]
outputs = llm.generate(prompts, sampling_params)

for output in outputs[0].outputs:
    print(f"Output: {output.text}")
```

## Performance Characteristics

### Memory Savings

- **Traditional**: Linear memory growth with sequence length and batch size
- **PagedAttention**: Near-optimal memory utilization with block-level granularity
- **Typical improvement**: 2-4x reduction in memory footprint for long contexts

### Throughput Gains

- More requests can fit in GPU memory simultaneously
- Higher batch sizes enable better GPU utilization
- Typical improvement: 2-3x higher throughput for memory-bound workloads

### Latency Considerations

- Lookup table adds minimal overhead (<1% for most workloads)
- Benefits increase with longer contexts and larger batch sizes
- Single short generations may see negligible improvement

## Configuration Tips

### Block Size Selection

```python
# Typical block sizes (vLLM)
# Larger blocks = less lookup overhead, more internal fragmentation
# Smaller blocks = more flexible sharing, higher lookup cost

# Default: 16 tokens per block (good balance)
llm = LLM(model="...", block_size=16)

# For very long contexts, consider larger blocks
llm = LLM(model="...", block_size=32)
```

### Memory Management

```bash
# TGI: Control max batch size and token limits
--max-batch-prefill-tokens 8192  # Tokens processed in parallel during prefill
--max-total-tokens 65536          # Total tokens across all requests
--max-batch-total-tokens 1048576  # Total batch capacity
```

### Parallel Sampling Optimization

```python
# Generate multiple outputs efficiently
sampling_params = SamplingParams(
    n=10,  # PagedAttention shares prompt KV blocks across all 10 outputs
    temperature=0.9,
)
```

## Monitoring

Check memory utilization to verify PagedAttention benefits:

```python
# vLLM provides metrics
from vllm import LLM

llm = LLM(model="...")

# After generation
metrics = llm.get_model_config()
print(f"Block size: {metrics.block_size}")
print(f"Max model len: {metrics.max_model_len}")
```

## Frameworks Supporting PagedAttention

- **vLLM** — Original implementation with custom CUDA kernels
- **Text Generation Inference (TGI)** — Hugging Face's inference server using vLLM kernels
- **Ray Serve** — Can deploy vLLM models with PagedAttention
- **SGLang** — Structured generation framework built on vLLM
- **LMDeploy** — OpenMMLab inference engine with PagedAttention support

## Reference

- [Hugging Face TGI Paged Attention Documentation](https://huggingface.co/docs/text-generation-inference/en/conceptual/paged_attention)
- [vLLM Project](https://github.com/vllm-project/vllm)
- [vLLM Technical Blog](https://vllm.ai/)
- [PagedAttention Paper](https://arxiv.org/abs/2309.06180) (Efficient Memory Management for Large Language Model Serving with PagedAttention)

## Lessons Learned

- PagedAttention is most effective for long-context and high-throughput scenarios
- Parallel sampling sees dramatic memory savings through KV block sharing
- Block size tuning is rarely needed; defaults work well for most cases
- Memory-bound workloads benefit more than compute-bound workloads
- Lookup overhead is negligible compared to memory efficiency gains
- TGI and vLLM enable PagedAttention by default; no special configuration needed

## Status

Status: production-ready
Implementations: vLLM (original), TGI, LMDeploy, SGLang
