# CrewAI LLM Hooks

## Summary

CrewAI LLM Hooks provide fine-grained control over language model interactions during agent execution. They allow you to intercept LLM calls at two points (before and after), modify prompts, transform responses, implement approval gates, and add custom logging or monitoring.

## Best For

- Implementing iteration limits to prevent runaway agent loops
- Creating human-in-the-loop approval workflows
- Adding system-level guardrails and context to all LLM calls
- Sanitizing or redacting sensitive data (PII, API keys) from responses
- Debug logging and telemetry for LLM interactions
- Modifying prompts dynamically based on execution state
- Blocking unsafe or policy-violating LLM calls

## Avoid When

- You need to intercept tool calls (use Tool Call Hooks instead)
- You need crew/task lifecycle hooks (use Execution Boundary Hooks)
- Performance is critical and hooks would add overhead to every LLM call
- Simple logging is sufficient (use built-in CrewAI telemetry)

## Setup

```python
from crewai.hooks import on, HookAborted, InterceptionPoint, LLMCallHookContext

# Global hook - applies to all crews
@on(InterceptionPoint.PRE_MODEL_CALL)
def before_hook(ctx: LLMCallHookContext) -> None:
    # Mutate ctx.messages in place, or
    # raise HookAborted(reason, source) to block the call
    pass

@on(InterceptionPoint.POST_MODEL_CALL)
def after_hook(ctx: LLMCallHookContext) -> str | None:
    # Return a string to replace ctx.response
    # Return None to keep the original response
    return None

# Crew-scoped hook - only applies to this crew
@CrewBase
class MyProjCrew:
    @on(InterceptionPoint.PRE_MODEL_CALL)
    def validate_inputs(self, ctx):
        # Only applies to this crew
        if ctx.iterations == 0:
            print(f"Starting task: {ctx.task.description}")

    @crew
    def crew(self) -> Crew:
        return Crew(agents=self.agents, tasks=self.tasks, process=Process.sequential)
```

## Common Patterns

### Iteration Limiting

```python
@on(InterceptionPoint.PRE_MODEL_CALL)
def limit_iterations(ctx: LLMCallHookContext) -> None:
    if ctx.iterations > 15:
        raise HookAborted(reason="exceeded 15 iterations", source="loop-guard")
```

### Human Approval Gate

```python
@on(InterceptionPoint.PRE_MODEL_CALL)
def require_approval(ctx: LLMCallHookContext) -> None:
    if ctx.iterations > 5:
        response = ctx.request_human_input(
            prompt=f"Iteration {ctx.iterations}: Approve LLM call?",
            default_message="Press Enter to approve, or type 'no' to block:",
        )
        if response.lower() == "no":
            raise HookAborted(reason="blocked by user", source="approval-gate")
```

### Adding System Context

```python
@on(InterceptionPoint.PRE_MODEL_CALL)
def add_guardrails(ctx: LLMCallHookContext) -> None:
    ctx.messages.append({
        "role": "system",
        "content": "Ensure responses are factual and cite sources when possible."
    })
```

### Response Sanitization

```python
import re

@on(InterceptionPoint.POST_MODEL_CALL)
def sanitize_sensitive_data(ctx: LLMCallHookContext) -> str | None:
    if not ctx.response:
        return None
    sanitized = re.sub(r'\b\d{3}-\d{2}-\d{4}\b', '[SSN-REDACTED]', ctx.response)
    return re.sub(r'\b\d{4}[- ]?\d{4}[- ]?\d{4}[- ]?\d{4}\b', '[CARD-REDACTED]', sanitized)
```

### Debug Logging

```python
@on(InterceptionPoint.PRE_MODEL_CALL)
def debug_request(ctx: LLMCallHookContext) -> None:
    print(f"Agent: {ctx.agent.role}, iteration {ctx.iterations}, "
          f"{len(ctx.messages)} messages")

@on(InterceptionPoint.POST_MODEL_CALL)
def debug_response(ctx: LLMCallHookContext) -> None:
    if ctx.response:
        print(f"Response preview: {ctx.response[:100]}...")
```

### Agent-Scoped Hook

```python
# Only runs for specific agent roles
@on(InterceptionPoint.POST_MODEL_CALL, agents=["Researcher"])
def log_researcher_responses(ctx):
    print(f"Researcher response length: {len(ctx.response)}")
```

## Hook Management

```python
from crewai.hooks import (
    InterceptionPoint,
    clear_all_hooks,
    clear_hooks,
    get_hooks,
    unregister_hook,
)

# Unregister a specific hook
unregister_hook(InterceptionPoint.PRE_MODEL_CALL, my_hook)

# Clear one point, or everything (e.g. between tests)
clear_hooks(InterceptionPoint.POST_MODEL_CALL)
clear_all_hooks()

# Inspect what's registered
print(len(get_hooks(InterceptionPoint.PRE_MODEL_CALL)))
```

## LLMCallHookContext Object

```python
class LLMCallHookContext:
    executor: CrewAgentExecutor | LiteAgent | None  # Executor (None for direct LLM calls)
    messages: list               # Mutable message list
    agent: Agent | None          # Current agent (None for direct LLM calls)
    task: Task | None            # Current task (None for direct calls or LiteAgent)
    crew: Crew | None            # Crew instance (None for direct calls or LiteAgent)
    llm: BaseLLM | None          # LLM instance
    iterations: int              # Current iteration count (0 for direct calls)
    response: str | None         # LLM response (POST_MODEL_CALL only)
```

## Interception Points

| Point | When | Context Available |
|-------|------|-------------------|
| PRE_MODEL_CALL | Before every LLM call | LLMCallHookContext (no response) |
| POST_MODEL_CALL | After every LLM call | LLMCallHookContext (with response) |

## Best Practices

1. **Keep hooks focused and fast** — they run on every LLM call
2. **Modify in-place** — always mutate `ctx.messages`, never replace the list
   ```python
   # ✅ Correct - modify in-place
   ctx.messages.append({"role": "system", "content": "Be concise"})
   
   # ❌ Wrong - replaces list reference and breaks the executor
   ctx.messages = [{"role": "system", "content": "Be concise"}]
   ```
3. **Use type hints** — annotate with `LLMCallHookContext` for IDE support
4. **Abort loudly** — raise `HookAborted` with a meaningful reason and source; any other exception is swallowed (fail-open)
5. **Clear hooks in tests** — call `clear_all_hooks()` between test runs
6. **Scope appropriately** — use crew-scoped hooks for crew-specific logic, global hooks for cross-cutting concerns

## Troubleshooting

### Hook Not Executing

- Verify the hook is registered before crew execution
- Check whether an earlier hook aborted (subsequent hooks don't run)
- Ensure you're using the correct interception point

### Message Modifications Not Persisting

- Use in-place modifications: `ctx.messages.append(...)`
- Don't replace the list: `ctx.messages = []`

### Response Modifications Not Working

- Return the modified string from a `POST_MODEL_CALL` hook
- Returning `None` keeps the original response
- Check that your hook is registered for `POST_MODEL_CALL`, not `PRE_MODEL_CALL`

## Legacy Decorators

The original per-point decorators still work and run in the same chain as `@on` hooks:

```python
from crewai.hooks import before_llm_call, after_llm_call

@before_llm_call
def validate_iteration_count(context):
    if context.iterations > 10:
        return False  # Block execution
    return None

@after_llm_call(agents=["Researcher"])
def sanitize_response(context):
    if context.response and "API_KEY" in context.response:
        return context.response.replace("API_KEY", "[REDACTED]")
    return None
```

**Differences from `@on`:**
- Blocking is `return False` from a before hook (equivalent to raising `HookAborted`)
- No custom reason or source for telemetry when blocking
- Signatures are point-specific: before hooks return `bool | None`, after hooks return `str | None`

**Prefer `@on` for new code**; keep the legacy style where it's already in use.

## Related Hooks

- **Tool Call Hooks** — intercept tool executions
- **Execution Boundary Hooks** — crew/task start/end lifecycle
- **Step Hooks** — agent reasoning step boundaries

## Reference

- [Official CrewAI LLM Hooks Documentation](https://docs.crewai.com/v1.15.16/en/learn/llm-hooks)
- [Execution Hooks Overview](https://docs.crewai.com/edge/en/learn/execution-hooks)

## Lessons Learned

- Always modify `ctx.messages` in-place to avoid breaking the executor
- `HookAborted` provides better telemetry than returning `False` in legacy hooks
- Human approval gates work well for high-iteration scenarios
- Sanitization hooks are essential when dealing with sensitive data
- Crew-scoped hooks prevent polluting global hook registry

## Status

Status: stable
Version: v1.15.16
