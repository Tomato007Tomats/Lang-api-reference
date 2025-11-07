<!-- URL: page_3 -->

Skip to content

Edit this page

# Deep Agents reference

![PyPI - Version](https://pypi.org/project/deepagents/#history)![PyPI - License](https://opensource.org/licenses/MIT)![PyPI - Downloads](https://pypistats.org/packages/deepagents)

Welcome to the Deep Agents reference documentation!

Work in progress

This page is a work in progress, and we appreciate your patience as we continue to expand and improve the content.

## ``deepagents ¶

DeepAgents package.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `create_deep_agent` | Create a deep agent. |

### ``FilesystemMiddleware ¶

Bases: `AgentMiddleware`

Middleware for providing filesystem tools to an agent.

This middleware adds six filesystem tools to the agent: ls, read\_file, write\_file,
edit\_file, glob, and grep. Files can be stored using any backend that implements
the BackendProtocol.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `backend` | Backend for file storage. If not provided, defaults to StateBackend<br>(ephemeral storage in agent state). For persistent storage or hybrid setups,<br>use CompositeBackend with custom routes.<br>**TYPE:**`BACKEND_TYPES | None`**DEFAULT:**`None` |
| `system_prompt` | Optional custom system prompt override.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `custom_tool_descriptions` | Optional custom tool descriptions override.<br>**TYPE:**`dict[str, str] | None`**DEFAULT:**`None` |
| `tool_token_limit_before_evict` | Optional token limit before evicting a tool result to the filesystem.<br>**TYPE:**`int | None`**DEFAULT:**`20000` |

Example

```
from deepagents.middleware.filesystem import FilesystemMiddleware
from deepagents.memory.backends import StateBackend, StoreBackend, CompositeBackend
from langchain.agents import create_agent

# Ephemeral storage only (default)
agent = create_agent(middleware=[FilesystemMiddleware()])

# With hybrid storage (ephemeral + persistent /memories/)
backend = CompositeBackend(default=StateBackend(), routes={"/memories/": StoreBackend()})
agent = create_agent(middleware=[FilesystemMiddleware(memory_backend=backend)])
```

| METHOD | DESCRIPTION |
| --- | --- |
| `before_agent` | Logic to run before the agent execution starts. |
| `abefore_agent` | Async logic to run before the agent execution starts. |
| `before_model` | Logic to run before the model is called. |
| `abefore_model` | Async logic to run before the model is called. |
| `after_model` | Logic to run after the model is called. |
| `aafter_model` | Async logic to run after the model is called. |
| `after_agent` | Logic to run after the agent execution completes. |
| `aafter_agent` | Async logic to run after the agent execution completes. |
| `__init__` | Initialize the filesystem middleware. |
| `wrap_model_call` | Update the system prompt to include instructions on using the filesystem. |
| `awrap_model_call` | (async) Update the system prompt to include instructions on using the filesystem. |
| `wrap_tool_call` | Check the size of the tool call result and evict to filesystem if too large. |
| `awrap_tool_call` | (async)Check the size of the tool call result and evict to filesystem if too large. |

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema = FilesystemState
```

The schema for state passed to the middleware nodes.

#### ``tools`instance-attribute`¶

```
tools = _get_filesystem_tools(backend, custom_tool_descriptions)
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

#### ``\_\_init\_\_ ¶

```
__init__(
    *,
    backend: BACKEND_TYPES | None = None,
    system_prompt: str | None = None,
    custom_tool_descriptions: dict[str, str] | None = None,
    tool_token_limit_before_evict: int | None = 20000,
) -> None
```

Initialize the filesystem middleware.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `backend` | Backend for file storage, or a factory callable. Defaults to StateBackend if not provided.<br>**TYPE:**`BACKEND_TYPES | None`**DEFAULT:**`None` |
| `system_prompt` | Optional custom system prompt override.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `custom_tool_descriptions` | Optional custom tool descriptions override.<br>**TYPE:**`dict[str, str] | None`**DEFAULT:**`None` |
| `tool_token_limit_before_evict` | Optional token limit before evicting a tool result to the filesystem.<br>**TYPE:**`int | None`**DEFAULT:**`20000` |

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelResponse
```

Update the system prompt to include instructions on using the filesystem.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | The model request being processed.<br>**TYPE:**`ModelRequest` |
| `handler` | The handler function to call with the modified request.<br>**TYPE:**`Callable[[ModelRequest], ModelResponse]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelResponse` | The model response from the handler. |

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelResponse
```

(async) Update the system prompt to include instructions on using the filesystem.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | The model request being processed.<br>**TYPE:**`ModelRequest` |
| `handler` | The handler function to call with the modified request.<br>**TYPE:**`Callable[[ModelRequest], Awaitable[ModelResponse]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ModelResponse` | The model response from the handler. |

#### ``wrap\_tool\_call ¶

```
wrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], ToolMessage | Command],
) -> ToolMessage | Command
```

Check the size of the tool call result and evict to filesystem if too large.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | The tool call request being processed.<br>**TYPE:**`ToolCallRequest` |
| `handler` | The handler function to call with the modified request.<br>**TYPE:**`Callable[[ToolCallRequest], ToolMessage | Command]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | The raw ToolMessage, or a pseudo tool message with the ToolResult in state. |

#### ``awrap\_tool\_call`async`¶

```
awrap_tool_call(
    request: ToolCallRequest,
    handler: Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]],
) -> ToolMessage | Command
```

(async)Check the size of the tool call result and evict to filesystem if too large.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `request` | The tool call request being processed.<br>**TYPE:**`ToolCallRequest` |
| `handler` | The handler function to call with the modified request.<br>**TYPE:**`Callable[[ToolCallRequest], Awaitable[ToolMessage | Command]]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ToolMessage | Command` | The raw ToolMessage, or a pseudo tool message with the ToolResult in state. |

### ``CompiledSubAgent ¶

Bases: `TypedDict`

A pre-compiled agent spec.

#### ``name`instance-attribute`¶

```
name: str
```

The name of the agent.

#### ``description`instance-attribute`¶

```
description: str
```

The description of the agent.

#### ``runnable`instance-attribute`¶

```
runnable: Runnable
```

The Runnable to use for the agent.

### ``SubAgent ¶

Bases: `TypedDict`

Specification for an agent.

When specifying custom agents, the `default_middleware` from `SubAgentMiddleware`
will be applied first, followed by any `middleware` specified in this spec.
To use only custom middleware without the defaults, pass `default_middleware=[]`
to `SubAgentMiddleware`.

#### ``name`instance-attribute`¶

```
name: str
```

The name of the agent.

#### ``description`instance-attribute`¶

```
description: str
```

The description of the agent.

#### ``system\_prompt`instance-attribute`¶

```
system_prompt: str
```

The system prompt to use for the agent.

#### ``tools`instance-attribute`¶

```
tools: Sequence[BaseTool | Callable | dict[str, Any]]
```

The tools to use for the agent.

#### ``model`instance-attribute`¶

```
model: NotRequired[str | BaseChatModel]
```

The model for the agent. Defaults to `default_model`.

#### ``middleware`instance-attribute`¶

```
middleware: NotRequired[list[AgentMiddleware]]
```

Additional middleware to append after `default_middleware`.

#### ``interrupt\_on`instance-attribute`¶

```
interrupt_on: NotRequired[dict[str, bool | InterruptOnConfig]]
```

The tool configs to use for the agent.

### ``SubAgentMiddleware ¶

Bases: `AgentMiddleware`

Middleware for providing subagents to an agent via a `task` tool.

This middleware adds a `task` tool to the agent that can be used to invoke subagents.
Subagents are useful for handling complex tasks that require multiple steps, or tasks
that require a lot of context to resolve.

A chief benefit of subagents is that they can handle multi-step tasks, and then return
a clean, concise response to the main agent.

Subagents are also great for different domains of expertise that require a narrower
subset of tools and focus.

This middleware comes with a default general-purpose subagent that can be used to
handle the same tasks as the main agent, but with isolated context.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `default_model` | The model to use for subagents.<br>Can be a LanguageModelLike or a dict for init\_chat\_model.<br>**TYPE:**`str | BaseChatModel` |
| `default_tools` | The tools to use for the default general-purpose subagent.<br>**TYPE:**`Sequence[BaseTool | Callable | dict[str, Any]] | None`**DEFAULT:**`None` |
| `default_middleware` | Default middleware to apply to all subagents. If `None` (default),<br>no default middleware is applied. Pass a list to specify custom middleware.<br>**TYPE:**`list[AgentMiddleware] | None`**DEFAULT:**`None` |
| `default_interrupt_on` | The tool configs to use for the default general-purpose subagent. These<br>are also the fallback for any subagents that don't specify their own tool configs.<br>**TYPE:**`dict[str, bool | InterruptOnConfig] | None`**DEFAULT:**`None` |
| `subagents` | A list of additional subagents to provide to the agent.<br>**TYPE:**`list[SubAgent | CompiledSubAgent] | None`**DEFAULT:**`None` |
| `system_prompt` | Full system prompt override. When provided, completely replaces<br>the agent's system prompt.<br>**TYPE:**`str | None`**DEFAULT:**`TASK_SYSTEM_PROMPT` |
| `general_purpose_agent` | Whether to include the general-purpose agent. Defaults to `True`.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `task_description` | Custom description for the task tool. If `None`, uses the<br>default description template.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

Example

```
from langchain.agents.middleware.subagents import SubAgentMiddleware
from langchain.agents import create_agent

# Basic usage with defaults (no default middleware)
agent = create_agent(
    "openai:gpt-4o",
    middleware=[\
        SubAgentMiddleware(\
            default_model="openai:gpt-4o",\
            subagents=[],\
        )\
    ],
)

# Add custom middleware to subagents
agent = create_agent(
    "openai:gpt-4o",
    middleware=[\
        SubAgentMiddleware(\
            default_model="openai:gpt-4o",\
            default_middleware=[TodoListMiddleware()],\
            subagents=[],\
        )\
    ],
)
```

| METHOD | DESCRIPTION |
| --- | --- |
| `before_agent` | Logic to run before the agent execution starts. |
| `abefore_agent` | Async logic to run before the agent execution starts. |
| `before_model` | Logic to run before the model is called. |
| `abefore_model` | Async logic to run before the model is called. |
| `after_model` | Logic to run after the model is called. |
| `aafter_model` | Async logic to run after the model is called. |
| `after_agent` | Logic to run after the agent execution completes. |
| `aafter_agent` | Async logic to run after the agent execution completes. |
| `wrap_tool_call` | Intercept tool execution for retries, monitoring, or modification. |
| `awrap_tool_call` | Intercept and control async tool execution via handler callback. |
| `__init__` | Initialize the SubAgentMiddleware. |
| `wrap_model_call` | Update the system prompt to include instructions on using subagents. |
| `awrap_model_call` | (async) Update the system prompt to include instructions on using subagents. |

#### ``state\_schema`class-attribute``instance-attribute`¶

```
state_schema: type[StateT] = cast('type[StateT]', AgentState)
```

The schema for state passed to the middleware nodes.

#### ``name`property`¶

```
name: str
```

The name of the middleware instance.

Defaults to the class name, but can be overridden for custom naming.

#### ``tools`instance-attribute`¶

```
tools = [task_tool]
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
    default_model: str | BaseChatModel,
    default_tools: Sequence[BaseTool | Callable | dict[str, Any]] | None = None,
    default_middleware: list[AgentMiddleware] | None = None,
    default_interrupt_on: dict[str, bool | InterruptOnConfig] | None = None,
    subagents: list[SubAgent | CompiledSubAgent] | None = None,
    system_prompt: str | None = TASK_SYSTEM_PROMPT,
    general_purpose_agent: bool = True,
    task_description: str | None = None,
) -> None
```

Initialize the SubAgentMiddleware.

#### ``wrap\_model\_call ¶

```
wrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], ModelResponse]
) -> ModelResponse
```

Update the system prompt to include instructions on using subagents.

#### ``awrap\_model\_call`async`¶

```
awrap_model_call(
    request: ModelRequest, handler: Callable[[ModelRequest], Awaitable[ModelResponse]]
) -> ModelResponse
```

(async) Update the system prompt to include instructions on using subagents.

### ``create\_deep\_agent ¶

```
create_deep_agent(
    model: str | BaseChatModel | None = None,
    tools: Sequence[BaseTool | Callable | dict[str, Any]] | None = None,
    *,
    system_prompt: str | None = None,
    middleware: Sequence[AgentMiddleware] = (),
    subagents: list[SubAgent | CompiledSubAgent] | None = None,
    response_format: ResponseFormat | None = None,
    context_schema: type[Any] | None = None,
    checkpointer: Checkpointer | None = None,
    store: BaseStore | None = None,
    backend: BackendProtocol | BackendFactory | None = None,
    interrupt_on: dict[str, bool | InterruptOnConfig] | None = None,
    debug: bool = False,
    name: str | None = None,
    cache: BaseCache | None = None,
) -> CompiledStateGraph
```

Create a deep agent.

This agent will by default have access to a tool to write todos (write\_todos),
six file editing tools: write\_file, ls, read\_file, edit\_file, glob\_search, grep\_search,
and a tool to call subagents.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `model` | The model to use. Defaults to Claude Sonnet 4.<br>**TYPE:**`str | BaseChatModel | None`**DEFAULT:**`None` |
| `tools` | The tools the agent should have access to.<br>**TYPE:**`Sequence[BaseTool | Callable | dict[str, Any]] | None`**DEFAULT:**`None` |
| `system_prompt` | The additional instructions the agent should have. Will go in<br>the system prompt.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `middleware` | Additional middleware to apply after standard middleware.<br>**TYPE:**`Sequence[AgentMiddleware]`**DEFAULT:**`()` |
| `subagents` | The subagents to use. Each subagent should be a dictionary with the<br>following keys:<br>\- `name`<br>\- `description` (used by the main agent to decide whether to call the<br>sub agent)<br>\- `prompt` (used as the system prompt in the subagent)<br>\- (optional) `tools`<br>\- (optional) `model` (either a LanguageModelLike instance or dict<br>settings)<br>\- (optional) `middleware` (list of AgentMiddleware)<br>**TYPE:**`list[SubAgent | CompiledSubAgent] | None`**DEFAULT:**`None` |
| `response_format` | A structured output response format to use for the agent.<br>**TYPE:**`ResponseFormat | None`**DEFAULT:**`None` |
| `context_schema` | The schema of the deep agent.<br>**TYPE:**`type[Any] | None`**DEFAULT:**`None` |
| `checkpointer` | Optional checkpointer for persisting agent state between runs.<br>**TYPE:**`Checkpointer | None`**DEFAULT:**`None` |
| `store` | Optional store for persistent storage (required if backend uses StoreBackend).<br>**TYPE:**`BaseStore | None`**DEFAULT:**`None` |
| `backend` | Optional backend for file storage. Pass either a Backend instance or a<br>callable factory like `lambda rt: StateBackend(rt)`.<br>**TYPE:**`BackendProtocol | BackendFactory | None`**DEFAULT:**`None` |
| `interrupt_on` | Optional Dict\[str, bool \| InterruptOnConfig\] mapping tool names to<br>interrupt configs.<br>**TYPE:**`dict[str, bool | InterruptOnConfig] | None`**DEFAULT:**`None` |
| `debug` | Whether to enable debug mode. Passed through to create\_agent.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `name` | The name of the agent. Passed through to create\_agent.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `cache` | The cache to use for the agent. Passed through to create\_agent.<br>**TYPE:**`BaseCache | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CompiledStateGraph` | A configured deep agent. |

Back to top