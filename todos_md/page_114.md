<!-- URL: page_114 -->

Skip to content

Edit this page

# Middleware

## ``langchain.agents.middleware ¶

Entrypoint to using Middleware plugins with Agents.

Reference docs

This page contains **reference documentation** for Middleware. See
the docs for conceptual
guides, tutorials, and examples on using Middleware.

| CLASS | DESCRIPTION |
| --- | --- |
| `ContextEditingMiddleware` | Automatically prunes tool results to manage context size. |
| `HumanInTheLoopMiddleware` | Human in the loop middleware. |
| `LLMToolSelectorMiddleware` | Uses an LLM to select relevant tools before calling the main model. |
| `LLMToolEmulator` | Emulates specified tools using an LLM instead of executing them. |
| `ModelCallLimitMiddleware` | Tracks model call counts and enforces limits. |
| `ModelFallbackMiddleware` | Automatic fallback to alternative models on errors. |
| `PIIMiddleware` | Detect and handle Personally Identifiable Information (PII) in agent conversations. |
| `PIIDetectionError` | Raised when configured to block on detected sensitive values. |
| `SummarizationMiddleware` | Summarizes conversation history when token limits are approached. |
| `ToolCallLimitMiddleware` | Track tool call counts and enforces limits during agent execution. |
| `AgentMiddleware` | Base middleware class for an agent. |
| `AgentState` | State schema for the agent. |
| `ClearToolUsesEdit` | Configuration for clearing tool outputs when token limits are exceeded. |
| `InterruptOnConfig` | Configuration for an action requiring human in the loop. |
| `ModelRequest` | Model request information for the agent. |
| `ModelResponse` | Response from model execution including messages and optional structured output. |
| `ModelRequest` | Model request information for the agent. |

### ``ContextEditingMiddleware ¶

Bases: `AgentMiddleware`

Automatically prunes tool results to manage context size.

The middleware applies a sequence of edits when the total input token count
exceeds configured thresholds. Currently the `ClearToolUsesEdit` strategy is
supported, aligning with Anthropic's `clear_tool_uses_20250919` behaviour.

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema: type[StateT] = cast('type[StateT]', AgentState)
```

The schema for state passed to the middleware nodes.

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``before\_model ¶

```
before_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the model is called.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``after\_model ¶

```
after_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Intercept tool execution for retries, monitoring, or modification.

Multiple middleware compose automatically (first defined = outermost).
Exceptions propagate unless `handle_tool_errors` is configured on `ToolNode`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callable to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Modify request before execution:

```
def wrap_tool_call(self, request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Retry on error (call handler multiple times):

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

Conditional retry based on response:

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        result = handler(request)
        if isinstance(result, ToolMessage) and result.status != "error":
            return result
        if attempt < 2:
            continue
        return result
```

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Intercept and control async tool execution via handler callback.

The handler callback executes the tool call and returns a `ToolMessage` or
`Command`. Middleware can call the handler multiple times for retry logic, skip
calling it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callable to execute the tool and returns `ToolMessage` or<br>`Command`. Call this to execute the tool. Can be called multiple times<br>for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Async retry on error:

```
async def awrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = await handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

```
async def awrap_tool_call(self, request, handler):
    if cached := await get_cache_async(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = await handler(request)
    await save_cache_async(request, result)
    return result
```

#### ``\_\_init\_\_ ¶

```
__init__(
    *,
    edits: Iterable[ContextEdit] | None = None,
    token_count_method: Literal["approximate", "model"] = "approximate",
) -> None
```

Initializes a context editing middleware instance.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `edits` | Sequence of edit strategies to apply. Defaults to a single<br>`ClearToolUsesEdit` mirroring Anthropic defaults.<br>**TYPE:**`Iterable[ContextEdit] | None`**DEFAULT:**`None` |
| `token_count_method` | Whether to use approximate token counting<br>(faster, less accurate) or exact counting implemented by the<br>chat model (potentially slower, more accurate).<br>**TYPE:**`Literal['approximate', 'model']`**DEFAULT:**`'approximate'` |

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Apply context edits before invoking the model via handler.

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Apply context edits before invoking the model via handler (async version).

### ``HumanInTheLoopMiddleware ¶

Bases: `AgentMiddleware`

Human in the loop middleware.

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema: type[StateT] = cast('type[StateT]', AgentState)
```

The schema for state passed to the middleware nodes.

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``before\_model ¶

```
before_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the model is called.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Intercept and control model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], ModelResponse]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | `ModelCallResult` |

Examples:

Retry on error:

```
def wrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return handler(request)
        except Exception:
            if attempt == 2:
                raise
```

Rewrite response:

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    ai_msg = response.result[0]
    return ModelResponse(
        result=[AIMessage(content=f"[{ai_msg.content}]")],
        structured_response=response.structured_response,
    )
```

Error to fallback:

```
def wrap_model_call(self, request, handler):
    try:
        return handler(request)
    except Exception:
        return ModelResponse(result=[AIMessage(content="Service unavailable")])
```

Cache/short-circuit:

```
def wrap_model_call(self, request, handler):
    if cached := get_cache(request):
        return cached  # Short-circuit with cached result
    response = handler(request)
    save_cache(request, response)
    return response
```

Simple AIMessage return (converted automatically):

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    # Can return AIMessage directly for simple cases
    return AIMessage(content="Simplified response")
```

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Intercept and control async model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Async callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], Awaitable[ModelResponse]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | ModelCallResult |

Examples:

Retry on error:

```
async def awrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return await handler(request)
        except Exception:
            if attempt == 2:
                raise
```

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Intercept tool execution for retries, monitoring, or modification.

Multiple middleware compose automatically (first defined = outermost).
Exceptions propagate unless `handle_tool_errors` is configured on `ToolNode`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callable to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Modify request before execution:

```
def wrap_tool_call(self, request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Retry on error (call handler multiple times):

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

Conditional retry based on response:

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        result = handler(request)
        if isinstance(result, ToolMessage) and result.status != "error":
            return result
        if attempt < 2:
            continue
        return result
```

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Intercept and control async tool execution via handler callback.

The handler callback executes the tool call and returns a `ToolMessage` or
`Command`. Middleware can call the handler multiple times for retry logic, skip
calling it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callable to execute the tool and returns `ToolMessage` or<br>`Command`. Call this to execute the tool. Can be called multiple times<br>for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Async retry on error:

```
async def awrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = await handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

```
async def awrap_tool_call(self, request, handler):
    if cached := await get_cache_async(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = await handler(request)
    await save_cache_async(request, result)
    return result
```

#### ``\_\_init\_\_ ¶

```
__init__(
    interrupt_on: dict[str, bool | InterruptOnConfig],
    *,
    description_prefix: str = "Tool execution requires approval",
) -> None
```

Initialize the human in the loop middleware.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `interrupt_on` | Mapping of tool name to allowed actions.<br>If a tool doesn't have an entry, it's auto-approved by default.<br>- `True` indicates all decisions are allowed: approve, edit, and reject.<br>- `False` indicates that the tool is auto-approved.<br>- `InterruptOnConfig` indicates the specific decisions allowed for this<br>tool.<br>The InterruptOnConfig can include a `description` field (`str` or<br>`Callable`) for custom formatting of the interrupt description.<br>**TYPE:**`dict[str, bool | InterruptOnConfig]` |
| `description_prefix` | The prefix to use when constructing action requests.<br>This is used to provide context about the tool call and the action being<br>requested. Not used if a tool has a `description` in its<br>`InterruptOnConfig`.<br>**TYPE:**`str`**DEFAULT:**`'Tool execution requires approval'` |

#### ``after\_model ¶

```
after_model(state: AgentState, runtime: Runtime) -> dict[str, Any] | None
```

Trigger interrupt flows for relevant tool calls after an `AIMessage`.

### ``LLMToolSelectorMiddleware ¶

Bases: `AgentMiddleware`

Uses an LLM to select relevant tools before calling the main model.

When an agent has many tools available, this middleware filters them down
to only the most relevant ones for the user's query. This reduces token usage
and helps the main model focus on the right tools.

Examples:

Limit to 3 tools:

```
from langchain.agents.middleware import LLMToolSelectorMiddleware

middleware = LLMToolSelectorMiddleware(max_tools=3)

agent = create_agent(
    model="openai:gpt-4o",
    tools=[tool1, tool2, tool3, tool4, tool5],
    middleware=[middleware],
)
```

Use a smaller model for selection:

```
middleware = LLMToolSelectorMiddleware(model="openai:gpt-4o-mini", max_tools=2)
```

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema: type[StateT] = cast('type[StateT]', AgentState)
```

The schema for state passed to the middleware nodes.

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``before\_model ¶

```
before_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the model is called.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``after\_model ¶

```
after_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Intercept tool execution for retries, monitoring, or modification.

Multiple middleware compose automatically (first defined = outermost).
Exceptions propagate unless `handle_tool_errors` is configured on `ToolNode`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callable to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Modify request before execution:

```
def wrap_tool_call(self, request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Retry on error (call handler multiple times):

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

Conditional retry based on response:

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        result = handler(request)
        if isinstance(result, ToolMessage) and result.status != "error":
            return result
        if attempt < 2:
            continue
        return result
```

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Intercept and control async tool execution via handler callback.

The handler callback executes the tool call and returns a `ToolMessage` or
`Command`. Middleware can call the handler multiple times for retry logic, skip
calling it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callable to execute the tool and returns `ToolMessage` or<br>`Command`. Call this to execute the tool. Can be called multiple times<br>for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Async retry on error:

```
async def awrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = await handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

```
async def awrap_tool_call(self, request, handler):
    if cached := await get_cache_async(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = await handler(request)
    await save_cache_async(request, result)
    return result
```

#### ``\_\_init\_\_ ¶

```
__init__(
    *,
    model: str | BaseChatModel | None = None,
    system_prompt: str = DEFAULT_SYSTEM_PROMPT,
    max_tools: int | None = None,
    always_include: list[str] | None = None,
) -> None
```

Initialize the tool selector.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `model` | Model to use for selection. If not provided, uses the agent's main model.<br>Can be a model identifier string or BaseChatModel instance.<br>**TYPE:**`str | BaseChatModel | None`**DEFAULT:**`None` |
| `system_prompt` | Instructions for the selection model.<br>**TYPE:**`str`**DEFAULT:**`DEFAULT_SYSTEM_PROMPT` |
| `max_tools` | Maximum number of tools to select. If the model selects more,<br>only the first max\_tools will be used. No limit if not specified.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `always_include` | Tool names to always include regardless of selection.<br>These do not count against the max\_tools limit.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Filter tools based on LLM selection before invoking the model via handler.

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Filter tools based on LLM selection before invoking the model via handler.

### ``LLMToolEmulator ¶

Bases: `AgentMiddleware`

Emulates specified tools using an LLM instead of executing them.

This middleware allows selective emulation of tools for testing purposes.
By default (when tools=None), all tools are emulated. You can specify which
tools to emulate by passing a list of tool names or BaseTool instances.

Examples:

Emulate all tools (default behavior):

```
from langchain.agents.middleware import LLMToolEmulator

middleware = LLMToolEmulator()

agent = create_agent(
    model="openai:gpt-4o",
    tools=[get_weather, get_user_location, calculator],
    middleware=[middleware],
)
```

Emulate specific tools by name:

```
middleware = LLMToolEmulator(tools=["get_weather", "get_user_location"])
```

Use a custom model for emulation:

```
middleware = LLMToolEmulator(
    tools=["get_weather"], model="anthropic:claude-sonnet-4-5-20250929"
)
```

Emulate specific tools by passing tool instances:

```
middleware = LLMToolEmulator(tools=[get_weather, get_user_location])
```

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema: type[StateT] = cast('type[StateT]', AgentState)
```

The schema for state passed to the middleware nodes.

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``before\_model ¶

```
before_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the model is called.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``after\_model ¶

```
after_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Intercept and control model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], ModelResponse]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | `ModelCallResult` |

Examples:

Retry on error:

```
def wrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return handler(request)
        except Exception:
            if attempt == 2:
                raise
```

Rewrite response:

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    ai_msg = response.result[0]
    return ModelResponse(
        result=[AIMessage(content=f"[{ai_msg.content}]")],
        structured_response=response.structured_response,
    )
```

Error to fallback:

```
def wrap_model_call(self, request, handler):
    try:
        return handler(request)
    except Exception:
        return ModelResponse(result=[AIMessage(content="Service unavailable")])
```

Cache/short-circuit:

```
def wrap_model_call(self, request, handler):
    if cached := get_cache(request):
        return cached  # Short-circuit with cached result
    response = handler(request)
    save_cache(request, response)
    return response
```

Simple AIMessage return (converted automatically):

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    # Can return AIMessage directly for simple cases
    return AIMessage(content="Simplified response")
```

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Intercept and control async model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Async callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], Awaitable[ModelResponse]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | ModelCallResult |

Examples:

Retry on error:

```
async def awrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return await handler(request)
        except Exception:
            if attempt == 2:
                raise
```

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``\_\_init\_\_ ¶

```
__init__(
    *,
    tools: list[str | BaseTool] | None = None,
    model: str | BaseChatModel | None = None,
) -> None
```

Initialize the tool emulator.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `tools` | List of tool names (str) or BaseTool instances to emulate.<br>If None (default), ALL tools will be emulated.<br>If empty list, no tools will be emulated.<br>**TYPE:**`list[str | BaseTool] | None`**DEFAULT:**`None` |
| `model` | Model to use for emulation.<br>Defaults to "anthropic:claude-sonnet-4-5-20250929".<br>Can be a model identifier string or BaseChatModel instance.<br>**TYPE:**`str | BaseChatModel | None`**DEFAULT:**`None` |

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Emulate tool execution using LLM if tool should be emulated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request to potentially emulate.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callback to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | ToolMessage with emulated response if tool should be emulated, |
| `ToolMessage | Command` | otherwise calls handler for normal execution. |

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Async version of wrap\_tool\_call.

Emulate tool execution using LLM if tool should be emulated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request to potentially emulate.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callback to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | ToolMessage with emulated response if tool should be emulated, |
| `ToolMessage | Command` | otherwise calls handler for normal execution. |

### ``ModelCallLimitMiddleware ¶

Bases: `AgentMiddleware[ModelCallLimitState, Any]`

Tracks model call counts and enforces limits.

This middleware monitors the number of model calls made during agent execution
and can terminate the agent when specified limits are reached. It supports
both thread-level and run-level call counting with configurable exit behaviors.

Thread-level: The middleware tracks the number of model calls and persists
call count across multiple runs (invocations) of the agent.

Run-level: The middleware tracks the number of model calls made during a single
run (invocation) of the agent.

Example

```
from langchain.agents.middleware.call_tracking import ModelCallLimitMiddleware
from langchain.agents import create_agent

# Create middleware with limits
call_tracker = ModelCallLimitMiddleware(thread_limit=10, run_limit=5, exit_behavior="end")

agent = create_agent("openai:gpt-4o", middleware=[call_tracker])

# Agent will automatically jump to end when limits are exceeded
result = await agent.invoke({"messages": [HumanMessage("Help me with a task")]})
```

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Intercept and control model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], ModelResponse]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | `ModelCallResult` |

Examples:

Retry on error:

```
def wrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return handler(request)
        except Exception:
            if attempt == 2:
                raise
```

Rewrite response:

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    ai_msg = response.result[0]
    return ModelResponse(
        result=[AIMessage(content=f"[{ai_msg.content}]")],
        structured_response=response.structured_response,
    )
```

Error to fallback:

```
def wrap_model_call(self, request, handler):
    try:
        return handler(request)
    except Exception:
        return ModelResponse(result=[AIMessage(content="Service unavailable")])
```

Cache/short-circuit:

```
def wrap_model_call(self, request, handler):
    if cached := get_cache(request):
        return cached  # Short-circuit with cached result
    response = handler(request)
    save_cache(request, response)
    return response
```

Simple AIMessage return (converted automatically):

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    # Can return AIMessage directly for simple cases
    return AIMessage(content="Simplified response")
```

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Intercept and control async model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Async callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], Awaitable[ModelResponse]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | ModelCallResult |

Examples:

Retry on error:

```
async def awrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return await handler(request)
        except Exception:
            if attempt == 2:
                raise
```

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Intercept tool execution for retries, monitoring, or modification.

Multiple middleware compose automatically (first defined = outermost).
Exceptions propagate unless `handle_tool_errors` is configured on `ToolNode`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callable to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Modify request before execution:

```
def wrap_tool_call(self, request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Retry on error (call handler multiple times):

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

Conditional retry based on response:

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        result = handler(request)
        if isinstance(result, ToolMessage) and result.status != "error":
            return result
        if attempt < 2:
            continue
        return result
```

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Intercept and control async tool execution via handler callback.

The handler callback executes the tool call and returns a `ToolMessage` or
`Command`. Middleware can call the handler multiple times for retry logic, skip
calling it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callable to execute the tool and returns `ToolMessage` or<br>`Command`. Call this to execute the tool. Can be called multiple times<br>for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Async retry on error:

```
async def awrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = await handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

```
async def awrap_tool_call(self, request, handler):
    if cached := await get_cache_async(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = await handler(request)
    await save_cache_async(request, result)
    return result
```

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema = ModelCallLimitState
```

The schema for state passed to the middleware nodes.

#### ``\_\_init\_\_ ¶

```
__init__(
    *,
    thread_limit: int | None = None,
    run_limit: int | None = None,
    exit_behavior: Literal["end", "error"] = "end",
) -> None
```

Initialize the call tracking middleware.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `thread_limit` | Maximum number of model calls allowed per thread.<br>None means no limit.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `run_limit` | Maximum number of model calls allowed per run.<br>None means no limit.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `exit_behavior` | What to do when limits are exceeded.<br>\- "end": Jump to the end of the agent execution and<br>inject an artificial AI message indicating that the limit was exceeded.<br>\- "error": Raise a `ModelCallLimitExceededError`<br>**TYPE:**`Literal['end', 'error']`**DEFAULT:**`'end'` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If both limits are `None` or if `exit_behavior` is invalid. |

#### ``before\_model ¶

```
before_model(state: ModelCallLimitState, runtime: Runtime) -> dict[str, Any] | None
```

Check model call limits before making a model call.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `state` | The current agent state containing call counts.<br>**TYPE:**`ModelCallLimitState` |
| `runtime` | The langgraph runtime.<br>**TYPE:**`Runtime` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any] | None` | If limits are exceeded and exit\_behavior is "end", returns |
| `dict[str, Any] | None` | a Command to jump to the end with a limit exceeded message. Otherwise returns None. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ModelCallLimitExceededError` | If limits are exceeded and exit\_behavior<br>is "error". |

#### ``after\_model ¶

```
after_model(state: ModelCallLimitState, runtime: Runtime) -> dict[str, Any] | None
```

Increment model call counts after a model call.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `state` | The current agent state.<br>**TYPE:**`ModelCallLimitState` |
| `runtime` | The langgraph runtime.<br>**TYPE:**`Runtime` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any] | None` | State updates with incremented call counts. |

### ``ModelFallbackMiddleware ¶

Bases: `AgentMiddleware`

Automatic fallback to alternative models on errors.

Retries failed model calls with alternative models in sequence until
success or all models exhausted. Primary model specified in create\_agent().

Example

```
from langchain.agents.middleware.model_fallback import ModelFallbackMiddleware
from langchain.agents import create_agent

fallback = ModelFallbackMiddleware(
    "openai:gpt-4o-mini",  # Try first on error
    "anthropic:claude-sonnet-4-5-20250929",  # Then this
)

agent = create_agent(
    model="openai:gpt-4o",  # Primary model
    middleware=[fallback],
)

# If primary fails: tries gpt-4o-mini, then claude-sonnet-4-5-20250929
result = await agent.invoke({"messages": [HumanMessage("Hello")]})
```

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema: type[StateT] = cast('type[StateT]', AgentState)
```

The schema for state passed to the middleware nodes.

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``before\_model ¶

```
before_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the model is called.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``after\_model ¶

```
after_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Intercept tool execution for retries, monitoring, or modification.

Multiple middleware compose automatically (first defined = outermost).
Exceptions propagate unless `handle_tool_errors` is configured on `ToolNode`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callable to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Modify request before execution:

```
def wrap_tool_call(self, request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Retry on error (call handler multiple times):

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

Conditional retry based on response:

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        result = handler(request)
        if isinstance(result, ToolMessage) and result.status != "error":
            return result
        if attempt < 2:
            continue
        return result
```

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Intercept and control async tool execution via handler callback.

The handler callback executes the tool call and returns a `ToolMessage` or
`Command`. Middleware can call the handler multiple times for retry logic, skip
calling it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callable to execute the tool and returns `ToolMessage` or<br>`Command`. Call this to execute the tool. Can be called multiple times<br>for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Async retry on error:

```
async def awrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = await handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

```
async def awrap_tool_call(self, request, handler):
    if cached := await get_cache_async(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = await handler(request)
    await save_cache_async(request, result)
    return result
```

#### ``\_\_init\_\_ ¶

```
__init__(
    first_model: str | BaseChatModel, *additional_models: str | BaseChatModel
) -> None
```

Initialize model fallback middleware.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `first_model` | First fallback model (string name or instance).<br>**TYPE:**`str | BaseChatModel` |
| `*additional_models` | Additional fallbacks in order.<br>**TYPE:**`str | BaseChatModel`**DEFAULT:**`()` |

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Try fallback models in sequence on errors.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Initial model request.<br>**TYPE:**`ModelRequest` |
| `handler` | Callback to execute the model.<br>**TYPE:**`Callable[[ModelRequest], ModelResponse]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | AIMessage from successful model call. |

| RAISES | DESCRIPTION |
| --- | --- |
| `Exception` | If all models fail, re-raises last exception. |

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Try fallback models in sequence on errors (async version).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Initial model request.<br>**TYPE:**`ModelRequest` |
| `handler` | Async callback to execute the model.<br>**TYPE:**`Callable[[ModelRequest], Awaitable[ModelResponse]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | AIMessage from successful model call. |

| RAISES | DESCRIPTION |
| --- | --- |
| `Exception` | If all models fail, re-raises last exception. |

### ``PIIMiddleware ¶

Bases: `AgentMiddleware`

Detect and handle Personally Identifiable Information (PII) in agent conversations.

This middleware detects common PII types and applies configurable strategies
to handle them. It can detect emails, credit cards, IP addresses,
MAC addresses, and URLs in both user input and agent output.

Built-in PII types

- `email`: Email addresses
- `credit_card`: Credit card numbers (validated with Luhn algorithm)
- `ip`: IP addresses (validated with stdlib)
- `mac_address`: MAC addresses
- `url`: URLs (both `http`/`https` and bare URLs)

Strategies

- `block`: Raise an exception when PII is detected
- `redact`: Replace PII with `[REDACTED_TYPE]` placeholders
- `mask`: Partially mask PII (e.g., `****-****-****-1234` for credit card)
- `hash`: Replace PII with deterministic hash (e.g., `<email_hash:a1b2c3d4>`)

Strategy Selection Guide:

| Strategy | Preserves Identity? | Best For |
| --- | --- | --- |
| `block` | N/A | Avoid PII completely |
| `redact` | No | General compliance, log sanitization |
| `mask` | No | Human readability, customer service UIs |
| `hash` | Yes (pseudonymous) | Analytics, debugging |

Example

```
from langchain.agents.middleware import PIIMiddleware
from langchain.agents import create_agent

# Redact all emails in user input
agent = create_agent(
    "openai:gpt-5",
    middleware=[\
        PIIMiddleware("email", strategy="redact"),\
    ],
)

# Use different strategies for different PII types
agent = create_agent(
    "openai:gpt-4o",
    middleware=[\
        PIIMiddleware("credit_card", strategy="mask"),\
        PIIMiddleware("url", strategy="redact"),\
        PIIMiddleware("ip", strategy="hash"),\
    ],
)

# Custom PII type with regex
agent = create_agent(
    "openai:gpt-5",
    middleware=[\
        PIIMiddleware("api_key", detector=r"sk-[a-zA-Z0-9]{32}", strategy="block"),\
    ],
)
```

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema: type[StateT] = cast('type[StateT]', AgentState)
```

The schema for state passed to the middleware nodes.

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Intercept and control model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], ModelResponse]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | `ModelCallResult` |

Examples:

Retry on error:

```
def wrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return handler(request)
        except Exception:
            if attempt == 2:
                raise
```

Rewrite response:

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    ai_msg = response.result[0]
    return ModelResponse(
        result=[AIMessage(content=f"[{ai_msg.content}]")],
        structured_response=response.structured_response,
    )
```

Error to fallback:

```
def wrap_model_call(self, request, handler):
    try:
        return handler(request)
    except Exception:
        return ModelResponse(result=[AIMessage(content="Service unavailable")])
```

Cache/short-circuit:

```
def wrap_model_call(self, request, handler):
    if cached := get_cache(request):
        return cached  # Short-circuit with cached result
    response = handler(request)
    save_cache(request, response)
    return response
```

Simple AIMessage return (converted automatically):

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    # Can return AIMessage directly for simple cases
    return AIMessage(content="Simplified response")
```

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Intercept and control async model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Async callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], Awaitable[ModelResponse]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | ModelCallResult |

Examples:

Retry on error:

```
async def awrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return await handler(request)
        except Exception:
            if attempt == 2:
                raise
```

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Intercept tool execution for retries, monitoring, or modification.

Multiple middleware compose automatically (first defined = outermost).
Exceptions propagate unless `handle_tool_errors` is configured on `ToolNode`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callable to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Modify request before execution:

```
def wrap_tool_call(self, request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Retry on error (call handler multiple times):

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

Conditional retry based on response:

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        result = handler(request)
        if isinstance(result, ToolMessage) and result.status != "error":
            return result
        if attempt < 2:
            continue
        return result
```

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Intercept and control async tool execution via handler callback.

The handler callback executes the tool call and returns a `ToolMessage` or
`Command`. Middleware can call the handler multiple times for retry logic, skip
calling it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callable to execute the tool and returns `ToolMessage` or<br>`Command`. Call this to execute the tool. Can be called multiple times<br>for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Async retry on error:

```
async def awrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = await handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

```
async def awrap_tool_call(self, request, handler):
    if cached := await get_cache_async(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = await handler(request)
    await save_cache_async(request, result)
    return result
```

#### ``\_\_init\_\_ ¶

```
__init__(
    pii_type: Literal["email", "credit_card", "ip", "mac_address", "url"] | str,
    *,
    strategy: Literal["block", "redact", "mask", "hash"] = "redact",
    detector: Callable[[str], list[PIIMatch]] | str | None = None,
    apply_to_input: bool = True,
    apply_to_output: bool = False,
    apply_to_tool_results: bool = False,
) -> None
```

Initialize the PII detection middleware.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `pii_type` | Type of PII to detect. Can be a built-in type<br>(`email`, `credit_card`, `ip`, `mac_address`, `url`)<br>or a custom type name.<br>**TYPE:**`Literal['email', 'credit_card', 'ip', 'mac_address', 'url'] | str` |
| `strategy` | How to handle detected PII:<br>- `block`: Raise PIIDetectionError when PII is detected<br>- `redact`: Replace with `[REDACTED_TYPE]` placeholders<br>- `mask`: Partially mask PII (show last few characters)<br>- `hash`: Replace with deterministic hash (format: `<type_hash:digest>`)<br>**TYPE:**`Literal['block', 'redact', 'mask', 'hash']`**DEFAULT:**`'redact'` |
| `detector` | Custom detector function or regex pattern.<br>- If `Callable`: Function that takes content string and returns<br>list of PIIMatch objects<br>- If `str`: Regex pattern to match PII<br>- If `None`: Uses built-in detector for the pii\_type<br>**TYPE:**`Callable[[str], list[PIIMatch]] | str | None`**DEFAULT:**`None` |
| `apply_to_input` | Whether to check user messages before model call.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `apply_to_output` | Whether to check AI messages after model call.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `apply_to_tool_results` | Whether to check tool result messages after tool execution.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If pii\_type is not built-in and no detector is provided. |

#### ``name`property`¶

```
name: str
```

Name of the middleware.

#### ``before\_model ¶

```
before_model(state: AgentState, runtime: Runtime) -> dict[str, Any] | None
```

Check user messages and tool results for PII before model invocation.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `state` | The current agent state.<br>**TYPE:**`AgentState` |
| `runtime` | The langgraph runtime.<br>**TYPE:**`Runtime` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any] | None` | Updated state with PII handled according to strategy, or None if no PII detected. |

| RAISES | DESCRIPTION |
| --- | --- |
| `PIIDetectionError` | If PII is detected and strategy is "block". |

#### ``after\_model ¶

```
after_model(state: AgentState, runtime: Runtime) -> dict[str, Any] | None
```

Check AI messages for PII after model invocation.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `state` | The current agent state.<br>**TYPE:**`AgentState` |
| `runtime` | The langgraph runtime.<br>**TYPE:**`Runtime` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any] | None` | Updated state with PII handled according to strategy, or None if no PII detected. |

| RAISES | DESCRIPTION |
| --- | --- |
| `PIIDetectionError` | If PII is detected and strategy is "block". |

### ``PIIDetectionError ¶

Bases: `Exception`

Raised when configured to block on detected sensitive values.

#### ``\_\_init\_\_ ¶

```
__init__(pii_type: str, matches: Sequence[PIIMatch]) -> None
```

Initialize the exception with match context.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `pii_type` | Name of the detected sensitive type.<br>**TYPE:**`str` |
| `matches` | All matches that were detected for that type.<br>**TYPE:**`Sequence[PIIMatch]` |

### ``SummarizationMiddleware ¶

Bases: `AgentMiddleware`

Summarizes conversation history when token limits are approached.

This middleware monitors message token counts and automatically summarizes older
messages when a threshold is reached, preserving recent messages and maintaining
context continuity by ensuring AI/Tool message pairs remain together.

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema: type[StateT] = cast('type[StateT]', AgentState)
```

The schema for state passed to the middleware nodes.

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``after\_model ¶

```
after_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Intercept and control model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], ModelResponse]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | `ModelCallResult` |

Examples:

Retry on error:

```
def wrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return handler(request)
        except Exception:
            if attempt == 2:
                raise
```

Rewrite response:

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    ai_msg = response.result[0]
    return ModelResponse(
        result=[AIMessage(content=f"[{ai_msg.content}]")],
        structured_response=response.structured_response,
    )
```

Error to fallback:

```
def wrap_model_call(self, request, handler):
    try:
        return handler(request)
    except Exception:
        return ModelResponse(result=[AIMessage(content="Service unavailable")])
```

Cache/short-circuit:

```
def wrap_model_call(self, request, handler):
    if cached := get_cache(request):
        return cached  # Short-circuit with cached result
    response = handler(request)
    save_cache(request, response)
    return response
```

Simple AIMessage return (converted automatically):

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    # Can return AIMessage directly for simple cases
    return AIMessage(content="Simplified response")
```

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Intercept and control async model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Async callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], Awaitable[ModelResponse]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | ModelCallResult |

Examples:

Retry on error:

```
async def awrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return await handler(request)
        except Exception:
            if attempt == 2:
                raise
```

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Intercept tool execution for retries, monitoring, or modification.

Multiple middleware compose automatically (first defined = outermost).
Exceptions propagate unless `handle_tool_errors` is configured on `ToolNode`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callable to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Modify request before execution:

```
def wrap_tool_call(self, request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Retry on error (call handler multiple times):

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

Conditional retry based on response:

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        result = handler(request)
        if isinstance(result, ToolMessage) and result.status != "error":
            return result
        if attempt < 2:
            continue
        return result
```

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Intercept and control async tool execution via handler callback.

The handler callback executes the tool call and returns a `ToolMessage` or
`Command`. Middleware can call the handler multiple times for retry logic, skip
calling it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callable to execute the tool and returns `ToolMessage` or<br>`Command`. Call this to execute the tool. Can be called multiple times<br>for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Async retry on error:

```
async def awrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = await handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

```
async def awrap_tool_call(self, request, handler):
    if cached := await get_cache_async(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = await handler(request)
    await save_cache_async(request, result)
    return result
```

#### ``\_\_init\_\_ ¶

```
__init__(
    model: str | BaseChatModel,
    max_tokens_before_summary: int | None = None,
    messages_to_keep: int = _DEFAULT_MESSAGES_TO_KEEP,
    token_counter: TokenCounter = count_tokens_approximately,
    summary_prompt: str = DEFAULT_SUMMARY_PROMPT,
    summary_prefix: str = SUMMARY_PREFIX,
) -> None
```

Initialize the summarization middleware.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `model` | The language model to use for generating summaries.<br>**TYPE:**`str | BaseChatModel` |
| `max_tokens_before_summary` | Token threshold to trigger summarization.<br>If `None`, summarization is disabled.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `messages_to_keep` | Number of recent messages to preserve after summarization.<br>**TYPE:**`int`**DEFAULT:**`_DEFAULT_MESSAGES_TO_KEEP` |
| `token_counter` | Function to count tokens in messages.<br>**TYPE:**`TokenCounter`**DEFAULT:**`count_tokens_approximately` |
| `summary_prompt` | Prompt template for generating summaries.<br>**TYPE:**`str`**DEFAULT:**`DEFAULT_SUMMARY_PROMPT` |
| `summary_prefix` | Prefix added to system message when including summary.<br>**TYPE:**`str`**DEFAULT:**`SUMMARY_PREFIX` |

#### ``before\_model ¶

```
before_model(state: AgentState, runtime: Runtime) -> dict[str, Any] | None
```

Process messages before model invocation, potentially triggering summarization.

### ``ToolCallLimitMiddleware ¶

Bases: `AgentMiddleware[ToolCallLimitState[ResponseT], ContextT]`, `Generic[ResponseT, ContextT]`

Track tool call counts and enforces limits during agent execution.

This middleware monitors the number of tool calls made and can terminate or
restrict execution when limits are exceeded. It supports both thread-level
(persistent across runs) and run-level (per invocation) call counting.

Configuration

- `exit_behavior`: How to handle when limits are exceeded
- `"continue"`: Block exceeded tools, let execution continue (default)
- `"error"`: Raise an exception
- `"end"`: Stop immediately with a ToolMessage + AI message for the single
tool call that exceeded the limit (raises `NotImplementedError` if there
are other pending tool calls (due to parallel tool calling).

Examples:

Continue execution with blocked tools (default):

```
from langchain.agents.middleware.tool_call_limit import ToolCallLimitMiddleware
from langchain.agents import create_agent

# Block exceeded tools but let other tools and model continue
limiter = ToolCallLimitMiddleware(
    thread_limit=20,
    run_limit=10,
    exit_behavior="continue",  # default
)

agent = create_agent("openai:gpt-4o", middleware=[limiter])
```

Stop immediately when limit exceeded:

```
# End execution immediately with an AI message
limiter = ToolCallLimitMiddleware(run_limit=5, exit_behavior="end")

agent = create_agent("openai:gpt-4o", middleware=[limiter])
```

Raise exception on limit:

```
# Strict limit with exception handling
limiter = ToolCallLimitMiddleware(tool_name="search", thread_limit=5, exit_behavior="error")

agent = create_agent("openai:gpt-4o", middleware=[limiter])

try:
    result = await agent.invoke({"messages": [HumanMessage("Task")]})
except ToolCallLimitExceededError as e:
    print(f"Search limit exceeded: {e}")
```

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``before\_model ¶

```
before_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the model is called.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Intercept and control model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], ModelResponse]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | `ModelCallResult` |

Examples:

Retry on error:

```
def wrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return handler(request)
        except Exception:
            if attempt == 2:
                raise
```

Rewrite response:

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    ai_msg = response.result[0]
    return ModelResponse(
        result=[AIMessage(content=f"[{ai_msg.content}]")],
        structured_response=response.structured_response,
    )
```

Error to fallback:

```
def wrap_model_call(self, request, handler):
    try:
        return handler(request)
    except Exception:
        return ModelResponse(result=[AIMessage(content="Service unavailable")])
```

Cache/short-circuit:

```
def wrap_model_call(self, request, handler):
    if cached := get_cache(request):
        return cached  # Short-circuit with cached result
    response = handler(request)
    save_cache(request, response)
    return response
```

Simple AIMessage return (converted automatically):

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    # Can return AIMessage directly for simple cases
    return AIMessage(content="Simplified response")
```

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Intercept and control async model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Async callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], Awaitable[ModelResponse]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | ModelCallResult |

Examples:

Retry on error:

```
async def awrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return await handler(request)
        except Exception:
            if attempt == 2:
                raise
```

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Intercept tool execution for retries, monitoring, or modification.

Multiple middleware compose automatically (first defined = outermost).
Exceptions propagate unless `handle_tool_errors` is configured on `ToolNode`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callable to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Modify request before execution:

```
def wrap_tool_call(self, request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Retry on error (call handler multiple times):

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

Conditional retry based on response:

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        result = handler(request)
        if isinstance(result, ToolMessage) and result.status != "error":
            return result
        if attempt < 2:
            continue
        return result
```

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Intercept and control async tool execution via handler callback.

The handler callback executes the tool call and returns a `ToolMessage` or
`Command`. Middleware can call the handler multiple times for retry logic, skip
calling it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callable to execute the tool and returns `ToolMessage` or<br>`Command`. Call this to execute the tool. Can be called multiple times<br>for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Async retry on error:

```
async def awrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = await handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

```
async def awrap_tool_call(self, request, handler):
    if cached := await get_cache_async(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = await handler(request)
    await save_cache_async(request, result)
    return result
```

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema = ToolCallLimitState
```

The schema for state passed to the middleware nodes.

#### ``\_\_init\_\_ ¶

```
__init__(
    *,
    tool_name: str | None = None,
    thread_limit: int | None = None,
    run_limit: int | None = None,
    exit_behavior: ExitBehavior = "continue",
) -> None
```

Initialize the tool call limit middleware.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `tool_name` | Name of the specific tool to limit. If `None`, limits apply<br>to all tools. Defaults to `None`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `thread_limit` | Maximum number of tool calls allowed per thread.<br>`None` means no limit. Defaults to `None`.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `run_limit` | Maximum number of tool calls allowed per run.<br>`None` means no limit. Defaults to `None`.<br>**TYPE:**`int | None`**DEFAULT:**`None` |
| `exit_behavior` | How to handle when limits are exceeded.<br>\- `"continue"`: Block exceeded tools with error messages, let other<br>tools continue. Model decides when to end. (default)<br>\- `"error"`: Raise a `ToolCallLimitExceededError` exception<br>\- `"end"`: Stop execution immediately with a ToolMessage + AI message<br>for the single tool call that exceeded the limit. Raises<br>`NotImplementedError` if there are multiple parallel tool<br>calls to other tools or multiple pending tool calls.<br>**TYPE:**`ExitBehavior`**DEFAULT:**`'continue'` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If both limits are `None`, if exit\_behavior is invalid,<br>or if run\_limit exceeds thread\_limit. |

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Includes the tool name if specified to allow multiple instances
of this middleware with different tool names.

#### ``after\_model ¶

```
after_model(
    state: ToolCallLimitState[ResponseT], runtime: Runtime[ContextT]
) -> dict[str, Any] | None
```

Increment tool call counts after a model call and check limits.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `state` | The current agent state.<br>**TYPE:**`ToolCallLimitState[ResponseT]` |
| `runtime` | The langgraph runtime.<br>**TYPE:**`Runtime[ContextT]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any] | None` | State updates with incremented tool call counts. If limits are exceeded |
| `dict[str, Any] | None` | and exit\_behavior is "end", also includes a jump to end with a ToolMessage |
| `dict[str, Any] | None` | and AI message for the single exceeded tool call. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ToolCallLimitExceededError` | If limits are exceeded and exit\_behavior<br>is "error". |
| `NotImplementedError` | If limits are exceeded, exit\_behavior is "end",<br>and there are multiple tool calls. |

### ``AgentMiddleware ¶

Bases: `Generic[StateT, ContextT]`

Base middleware class for an agent.

Subclass this and implement any of the defined methods to customize agent behavior
between steps in the main agent loop.

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema: type[StateT] = cast('type[StateT]', AgentState)
```

The schema for state passed to the middleware nodes.

#### ``tools`instance-attribute`¶

```
tools: list[BaseTool]
```

Additional tools registered by the middleware.

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``before\_agent ¶

```
before_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the agent execution starts.

#### ``abefore\_agent`async`¶

```
abefore_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the agent execution starts.

#### ``before\_model ¶

```
before_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run before the model is called.

#### ``abefore\_model`async`¶

```
abefore_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run before the model is called.

#### ``after\_model ¶

```
after_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the model is called.

#### ``aafter\_model`async`¶

```
aafter_model(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the model is called.

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelCallResult
```

Intercept and control model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], ModelResponse]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | `ModelCallResult` |

Examples:

Retry on error:

```
def wrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return handler(request)
        except Exception:
            if attempt == 2:
                raise
```

Rewrite response:

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    ai_msg = response.result[0]
    return ModelResponse(
        result=[AIMessage(content=f"[{ai_msg.content}]")],
        structured_response=response.structured_response,
    )
```

Error to fallback:

```
def wrap_model_call(self, request, handler):
    try:
        return handler(request)
    except Exception:
        return ModelResponse(result=[AIMessage(content="Service unavailable")])
```

Cache/short-circuit:

```
def wrap_model_call(self, request, handler):
    if cached := get_cache(request):
        return cached  # Short-circuit with cached result
    response = handler(request)
    save_cache(request, response)
    return response
```

Simple AIMessage return (converted automatically):

```
def wrap_model_call(self, request, handler):
    response = handler(request)
    # Can return AIMessage directly for simple cases
    return AIMessage(content="Simplified response")
```

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelCallResult
```

Intercept and control async model execution via handler callback.

The handler callback executes the model request and returns a `ModelResponse`.
Middleware can call the handler multiple times for retry logic, skip calling
it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Model request to execute (includes state and runtime).<br>**TYPE:**`ModelRequest` |
| `handler` | Async callback that executes the model request and returns<br>`ModelResponse`. Call this to execute the model. Can be called multiple<br>times for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ModelRequest], Awaitable[ModelResponse]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelCallResult` | ModelCallResult |

Examples:

Retry on error:

```
async def awrap_model_call(self, request, handler):
    for attempt in range(3):
        try:
            return await handler(request)
        except Exception:
            if attempt == 2:
                raise
```

#### ``after\_agent ¶

```
after_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Logic to run after the agent execution completes.

#### ``aafter\_agent`async`¶

```
aafter_agent(state: StateT, runtime: Runtime[ContextT]) -> dict[str, Any] | None
```

Async logic to run after the agent execution completes.

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Intercept tool execution for retries, monitoring, or modification.

Multiple middleware compose automatically (first defined = outermost).
Exceptions propagate unless `handle_tool_errors` is configured on `ToolNode`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Callable to execute the tool (can be called multiple times).<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Modify request before execution:

```
def wrap_tool_call(self, request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Retry on error (call handler multiple times):

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

Conditional retry based on response:

```
def wrap_tool_call(self, request, handler):
    for attempt in range(3):
        result = handler(request)
        if isinstance(result, ToolMessage) and result.status != "error":
            return result
        if attempt < 2:
            continue
        return result
```

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

Intercept and control async tool execution via handler callback.

The handler callback executes the tool call and returns a `ToolMessage` or
`Command`. Middleware can call the handler multiple times for retry logic, skip
calling it to short-circuit, or modify the request/response. Multiple middleware
compose with first in list as outermost layer.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | Tool call request with call `dict`, `BaseTool`, state, and runtime.<br>Access state via `request.state` and runtime via `request.runtime`.<br>**TYPE:**`ToolCallRequest` |
| `handler` | Async callable to execute the tool and returns `ToolMessage` or<br>`Command`. Call this to execute the tool. Can be called multiple times<br>for retry logic. Can skip calling it to short-circuit.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | `ToolMessage` or `Command` (the final result). |

The handler callable can be invoked multiple times for retry logic.
Each call to handler is independent and stateless.

Examples:

Async retry on error:

```
async def awrap_tool_call(self, request, handler):
    for attempt in range(3):
        try:
            result = await handler(request)
            if is_valid(result):
                return result
        except Exception:
            if attempt == 2:
                raise
    return result
```

```
async def awrap_tool_call(self, request, handler):
    if cached := await get_cache_async(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = await handler(request)
    await save_cache_async(request, result)
    return result
```

### ``AgentState ¶

Bases: `TypedDict`, `Generic[ResponseT]`

State schema for the agent.

### ``ClearToolUsesEdit`dataclass`¶

Bases: `ContextEdit`

Configuration for clearing tool outputs when token limits are exceeded.

#### ``trigger`class-attribute``instance-attribute`¶

```
trigger: int = 100000
```

Token count that triggers the edit.

#### ``clear\_at\_least`class-attribute``instance-attribute`¶

```
clear_at_least: int = 0
```

Minimum number of tokens to reclaim when the edit runs.

#### ``keep`class-attribute``instance-attribute`¶

```
keep: int = 3
```

Number of most recent tool results that must be preserved.

#### ``clear\_tool\_inputs`class-attribute``instance-attribute`¶

```
clear_tool_inputs: bool = False
```

Whether to clear the originating tool call parameters on the AI message.

#### ``exclude\_tools`class-attribute``instance-attribute`¶

```
exclude_tools: Sequence[str] = ()
```

List of tool names to exclude from clearing.

#### ``placeholder`class-attribute``instance-attribute`¶

```
placeholder: str = DEFAULT_TOOL_PLACEHOLDER
```

Placeholder text inserted for cleared tool outputs.

#### ``apply ¶

```
apply(messages: list[AnyMessage], *, count_tokens: TokenCounter) -> None
```

Apply the clear-tool-uses strategy.

### ``InterruptOnConfig ¶

Bases: `TypedDict`

Configuration for an action requiring human in the loop.

This is the configuration format used in the `HumanInTheLoopMiddleware.__init__`
method.

#### ``allowed\_decisions`instance-attribute`¶

```
allowed_decisions: list[DecisionType]
```

The decisions that are allowed for this action.

#### ``description`instance-attribute`¶

```
description: NotRequired[str | _DescriptionFactory]
```

The description attached to the request for human input.

Can be either:

- A static string describing the approval request
- A callable that dynamically generates the description based on agent state,
runtime, and tool call information

Example

```
# Static string description
config = ToolConfig(
    allowed_decisions=["approve", "reject"],
    description="Please review this tool execution"
)

# Dynamic callable description
def format_tool_description(
    tool_call: ToolCall,
    state: AgentState,
    runtime: Runtime
) -> str:
    import json
    return (
        f"Tool: {tool_call['name']}\n"
        f"Arguments:\n{json.dumps(tool_call['args'], indent=2)}"
    )

config = InterruptOnConfig(
    allowed_decisions=["approve", "edit", "reject"],
    description=format_tool_description
)
```

#### ``args\_schema`instance-attribute`¶

```
args_schema: NotRequired[dict[str, Any]]
```

JSON schema for the args associated with the action, if edits are allowed.

### ``ModelRequest`dataclass`¶

Model request information for the agent.

#### ``override ¶

```
override(**overrides: Unpack[_ModelRequestOverrides]) -> ModelRequest
```

Replace the request with a new request with the given overrides.

Returns a new `ModelRequest` instance with the specified attributes replaced.
This follows an immutable pattern, leaving the original request unchanged.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**overrides` | Keyword arguments for attributes to override. Supported keys:<br>\- model: BaseChatModel instance<br>\- system\_prompt: Optional system prompt string<br>\- messages: List of messages<br>\- tool\_choice: Tool choice configuration<br>\- tools: List of available tools<br>\- response\_format: Response format specification<br>\- model\_settings: Additional model settings<br>**TYPE:**`Unpack[_ModelRequestOverrides]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelRequest` | New ModelRequest instance with specified overrides applied. |

Examples:

```
# Create a new request with different model
new_request = request.override(model=different_model)

# Override multiple attributes
new_request = request.override(system_prompt="New instructions", tool_choice="auto")
```

### ``ModelResponse`dataclass`¶

Response from model execution including messages and optional structured output.

The result will usually contain a single AIMessage, but may include
an additional ToolMessage if the model used a tool for structured output.

#### ``result`instance-attribute`¶

```
result: list[BaseMessage]
```

List of messages from model execution.

#### ``structured\_response`class-attribute``instance-attribute`¶

```
structured_response: Any = None
```

Parsed structured output if response\_format was specified, None otherwise.

### ``before\_model ¶

```
before_model(
    func: _CallableWithStateAndRuntime[StateT, ContextT] | None = None,
    *,
    state_schema: type[StateT] | None = None,
    tools: list[BaseTool] | None = None,
    can_jump_to: list[JumpTo] | None = None,
    name: str | None = None,
) -> (
    Callable[\
        [_CallableWithStateAndRuntime[StateT, ContextT]],\
        AgentMiddleware[StateT, ContextT],\
    ]
    | AgentMiddleware[StateT, ContextT]
)
```

Decorator used to dynamically create a middleware with the `before_model` hook.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `func` | The function to be decorated. Must accept:<br>`state: StateT, runtime: Runtime[ContextT]` \- State and runtime context<br>**TYPE:**`_CallableWithStateAndRuntime[StateT, ContextT] | None`**DEFAULT:**`None` |
| `state_schema` | Optional custom state schema type. If not provided, uses the default<br>`AgentState` schema.<br>**TYPE:**`type[StateT] | None`**DEFAULT:**`None` |
| `tools` | Optional list of additional tools to register with this middleware.<br>**TYPE:**`list[BaseTool] | None`**DEFAULT:**`None` |
| `can_jump_to` | Optional list of valid jump destinations for conditional edges.<br>Valid values are: `"tools"`, `"model"`, `"end"`<br>**TYPE:**`list[JumpTo] | None`**DEFAULT:**`None` |
| `name` | Optional name for the generated middleware class. If not provided,<br>uses the decorated function's name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Callable[[_CallableWithStateAndRuntime[StateT, ContextT]], AgentMiddleware[StateT, ContextT]] | AgentMiddleware[StateT, ContextT]` | Either an `AgentMiddleware` instance (if func is provided directly) or a |
| `Callable[[_CallableWithStateAndRuntime[StateT, ContextT]], AgentMiddleware[StateT, ContextT]] | AgentMiddleware[StateT, ContextT]` | decorator function that can be applied to a function it is wrapping. |

The decorated function should return

- `dict[str, Any]` \- State updates to merge into the agent state
- `Command` \- A command to control flow (e.g., jump to different node)
- `None` \- No state updates or flow control

Examples:

Basic usage:

```
@before_model
def log_before_model(state: AgentState, runtime: Runtime) -> None:
    print(f"About to call model with {len(state['messages'])} messages")
```

With conditional jumping:

```
@before_model(can_jump_to=["end"])
def conditional_before_model(state: AgentState, runtime: Runtime) -> dict[str, Any] | None:
    if some_condition(state):
        return {"jump_to": "end"}
    return None
```

With custom state schema:

```
@before_model(state_schema=MyCustomState)
def custom_before_model(state: MyCustomState, runtime: Runtime) -> dict[str, Any]:
    return {"custom_field": "updated_value"}
```

### ``after\_model ¶

```
after_model(
    func: _CallableWithStateAndRuntime[StateT, ContextT] | None = None,
    *,
    state_schema: type[StateT] | None = None,
    tools: list[BaseTool] | None = None,
    can_jump_to: list[JumpTo] | None = None,
    name: str | None = None,
) -> (
    Callable[\
        [_CallableWithStateAndRuntime[StateT, ContextT]],\
        AgentMiddleware[StateT, ContextT],\
    ]
    | AgentMiddleware[StateT, ContextT]
)
```

Decorator used to dynamically create a middleware with the `after_model` hook.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `func` | The function to be decorated. Must accept:<br>`state: StateT, runtime: Runtime[ContextT]` \- State and runtime context<br>**TYPE:**`_CallableWithStateAndRuntime[StateT, ContextT] | None`**DEFAULT:**`None` |
| `state_schema` | Optional custom state schema type. If not provided, uses the<br>default `AgentState` schema.<br>**TYPE:**`type[StateT] | None`**DEFAULT:**`None` |
| `tools` | Optional list of additional tools to register with this middleware.<br>**TYPE:**`list[BaseTool] | None`**DEFAULT:**`None` |
| `can_jump_to` | Optional list of valid jump destinations for conditional edges.<br>Valid values are: `"tools"`, `"model"`, `"end"`<br>**TYPE:**`list[JumpTo] | None`**DEFAULT:**`None` |
| `name` | Optional name for the generated middleware class. If not provided,<br>uses the decorated function's name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Callable[[_CallableWithStateAndRuntime[StateT, ContextT]], AgentMiddleware[StateT, ContextT]] | AgentMiddleware[StateT, ContextT]` | Either an `AgentMiddleware` instance (if func is provided) or a decorator |
| `Callable[[_CallableWithStateAndRuntime[StateT, ContextT]], AgentMiddleware[StateT, ContextT]] | AgentMiddleware[StateT, ContextT]` | function that can be applied to a function. |

The decorated function should return

- `dict[str, Any]` \- State updates to merge into the agent state
- `Command` \- A command to control flow (e.g., jump to different node)
- `None` \- No state updates or flow control

Examples:

Basic usage for logging model responses:

```
@after_model
def log_latest_message(state: AgentState, runtime: Runtime) -> None:
    print(state"messages".content)
```

With custom state schema:

```
@after_model(state_schema=MyCustomState, name="MyAfterModelMiddleware")
def custom_after_model(state: MyCustomState, runtime: Runtime) -> dict[str, Any]:
    return {"custom_field": "updated_after_model"}
```

### ``wrap\_model\_call ¶

```
wrap_model_call(
    func: _CallableReturningModelResponse[StateT, ContextT] | None = None,
    *,
    state_schema: type[StateT] | None = None,
    tools: list[BaseTool] | None = None,
    name: str | None = None,
) -> (
    Callable[\
        [_CallableReturningModelResponse[StateT, ContextT]],\
        AgentMiddleware[StateT, ContextT],\
    ]
    | AgentMiddleware[StateT, ContextT]
)
```

Create middleware with `wrap_model_call` hook from a function.

Converts a function with handler callback into middleware that can intercept
model calls, implement retry logic, handle errors, and rewrite responses.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `func` | Function accepting (request, handler) that calls handler(request)<br>to execute the model and returns `ModelResponse` or `AIMessage`.<br>Request contains state and runtime.<br>**TYPE:**`_CallableReturningModelResponse[StateT, ContextT] | None`**DEFAULT:**`None` |
| `state_schema` | Custom state schema. Defaults to `AgentState`.<br>**TYPE:**`type[StateT] | None`**DEFAULT:**`None` |
| `tools` | Additional tools to register with this middleware.<br>**TYPE:**`list[BaseTool] | None`**DEFAULT:**`None` |
| `name` | Middleware class name. Defaults to function name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Callable[[_CallableReturningModelResponse[StateT, ContextT]], AgentMiddleware[StateT, ContextT]] | AgentMiddleware[StateT, ContextT]` | `AgentMiddleware` instance if func provided, otherwise a decorator. |

Examples:

Basic retry logic:

```
@wrap_model_call
def retry_on_error(request, handler):
    max_retries = 3
    for attempt in range(max_retries):
        try:
            return handler(request)
        except Exception:
            if attempt == max_retries - 1:
                raise
```

Model fallback:

```
@wrap_model_call
def fallback_model(request, handler):
    # Try primary model
    try:
        return handler(request)
    except Exception:
        pass

    # Try fallback model
    request.model = fallback_model_instance
    return handler(request)
```

Rewrite response content (full ModelResponse):

```
@wrap_model_call
def uppercase_responses(request, handler):
    response = handler(request)
    ai_msg = response.result[0]
    return ModelResponse(
        result=[AIMessage(content=ai_msg.content.upper())],
        structured_response=response.structured_response,
    )
```

Simple AIMessage return (converted automatically):

```
@wrap_model_call
def simple_response(request, handler):
    # AIMessage is automatically converted to ModelResponse
    return AIMessage(content="Simple response")
```

### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    func: _CallableReturningToolResponse | None = None,
    *,
    tools: list[BaseTool] | None = None,
    name: str | None = None,
) -> Callable[[_CallableReturningToolResponse], AgentMiddleware] | AgentMiddleware
```

Create middleware with `wrap_tool_call` hook from a function.

Converts a function with handler callback into middleware that can intercept
tool calls, implement retry logic, monitor execution, and modify responses.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `func` | Function accepting (request, handler) that calls<br>handler(request) to execute the tool and returns final `ToolMessage` or<br>`Command`. Can be sync or async.<br>**TYPE:**`_CallableReturningToolResponse | None`**DEFAULT:**`None` |
| `tools` | Additional tools to register with this middleware.<br>**TYPE:**`list[BaseTool] | None`**DEFAULT:**`None` |
| `name` | Middleware class name. Defaults to function name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Callable[[_CallableReturningToolResponse], AgentMiddleware] | AgentMiddleware` | `AgentMiddleware` instance if func provided, otherwise a decorator. |

Examples:

Retry logic:

```
@wrap_tool_call
def retry_on_error(request, handler):
    max_retries = 3
    for attempt in range(max_retries):
        try:
            return handler(request)
        except Exception:
            if attempt == max_retries - 1:
                raise
```

Async retry logic:

```
@wrap_tool_call
async def async_retry(request, handler):
    for attempt in range(3):
        try:
            return await handler(request)
        except Exception:
            if attempt == 2:
                raise
```

Modify request:

```
@wrap_tool_call
def modify_args(request, handler):
    request.tool_call"args" *= 2
    return handler(request)
```

Short-circuit with cached result:

```
@wrap_tool_call
def with_cache(request, handler):
    if cached := get_cache(request):
        return ToolMessage(content=cached, tool_call_id=request.tool_call["id"])
    result = handler(request)
    save_cache(request, result)
    return result
```

### ``ModelRequest`dataclass`¶

Model request information for the agent.

#### ``override ¶

```
override(**overrides: Unpack[_ModelRequestOverrides]) -> ModelRequest
```

Replace the request with a new request with the given overrides.

Returns a new `ModelRequest` instance with the specified attributes replaced.
This follows an immutable pattern, leaving the original request unchanged.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**overrides` | Keyword arguments for attributes to override. Supported keys:<br>\- model: BaseChatModel instance<br>\- system\_prompt: Optional system prompt string<br>\- messages: List of messages<br>\- tool\_choice: Tool choice configuration<br>\- tools: List of available tools<br>\- response\_format: Response format specification<br>\- model\_settings: Additional model settings<br>**TYPE:**`Unpack[_ModelRequestOverrides]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelRequest` | New ModelRequest instance with specified overrides applied. |

Examples:

```
# Create a new request with different model
new_request = request.override(model=different_model)

# Override multiple attributes
new_request = request.override(system_prompt="New instructions", tool_choice="auto")
```

Back to top