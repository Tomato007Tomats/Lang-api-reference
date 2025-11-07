<!-- URL: page_45 -->

Skip to content

Edit this page

# Agents

## ``langchain.agents ¶

Entrypoint to building Agents with LangChain.

Reference docs

This page contains **reference documentation** for Agents. See
the docs for conceptual
guides, tutorials, and examples on using Agents.

### ``create\_agent ¶

```
create_agent(
    model: str | BaseChatModel,
    tools: Sequence[BaseTool | Callable | dict[str, Any]] | None = None,
    *,
    system_prompt: str | None = None,
    middleware: Sequence[AgentMiddleware[StateT_co, ContextT]] = (),
    response_format: ResponseFormat[ResponseT] | type[ResponseT] | None = None,
    state_schema: type[AgentState[ResponseT]] | None = None,
    context_schema: type[ContextT] | None = None,
    checkpointer: Checkpointer | None = None,
    store: BaseStore | None = None,
    interrupt_before: list[str] | None = None,
    interrupt_after: list[str] | None = None,
    debug: bool = False,
    name: str | None = None,
    cache: BaseCache | None = None,
) -> CompiledStateGraph[\
    AgentState[ResponseT], ContextT, _InputAgentState, _OutputAgentState[ResponseT]\
]
```

Creates an agent graph that calls tools in a loop until a stopping condition is met.

For more details on using `create_agent`,
visit the Agents docs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| #### `model` ¶ "Copy anchor link to this section for reference") | The language model for the agent. Can be a string identifier<br>(e.g., `"openai:gpt-4"`) or a direct chat model instance (e.g.,<br>`ChatOpenAI` or other another<br>chat model).<br>For a full list of supported model strings, see<br>`init_chat_model` "<code>model_provider</code>").<br>**TYPE:**`str | BaseChatModel` |
| #### `tools` ¶ "Copy anchor link to this section for reference") | A list of tools, `dicts`, or `Callable`.<br>If `None` or an empty list, the agent will consist of a model node without a<br>tool calling loop.<br>**TYPE:**`Sequence[BaseTool | Callable | dict[str, Any]] | None`**DEFAULT:**`None` |
| #### `system_prompt` ¶ "Copy anchor link to this section for reference") | An optional system prompt for the LLM.<br>Prompts are converted to a<br>`SystemMessage` and added to the<br>beginning of the message list.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| #### `middleware` ¶ "Copy anchor link to this section for reference") | A sequence of middleware instances to apply to the agent.<br>Middleware can intercept and modify agent behavior at various stages. See<br>the full guide.<br>**TYPE:**`Sequence[AgentMiddleware[StateT_co, ContextT]]`**DEFAULT:**`()` |
| #### `response_format` ¶ "Copy anchor link to this section for reference") | An optional configuration for structured responses.<br>Can be a `ToolStrategy`, `ProviderStrategy`, or a Pydantic model class.<br>If provided, the agent will handle structured output during the<br>conversation flow. Raw schemas will be wrapped in an appropriate strategy<br>based on model capabilities.<br>**TYPE:**`ResponseFormat[ResponseT] | type[ResponseT] | None`**DEFAULT:**`None` |
| #### `state_schema` ¶ "Copy anchor link to this section for reference") | An optional `TypedDict` schema that extends `AgentState`.<br>When provided, this schema is used instead of `AgentState` as the base<br>schema for merging with middleware state schemas. This allows users to<br>add custom state fields without needing to create custom middleware.<br>Generally, it's recommended to use `state_schema` extensions via middleware<br>to keep relevant extensions scoped to corresponding hooks / tools.<br>The schema must be a subclass of `AgentState[ResponseT]`.<br>**TYPE:**`type[AgentState[ResponseT]] | None`**DEFAULT:**`None` |
| #### `context_schema` ¶ "Copy anchor link to this section for reference") | An optional schema for runtime context.<br>**TYPE:**`type[ContextT] | None`**DEFAULT:**`None` |
| #### `checkpointer` ¶ "Copy anchor link to this section for reference") | An optional checkpoint saver object.<br>Used for persisting the state of the graph (e.g., as chat memory) for a<br>single thread (e.g., a single conversation).<br>**TYPE:**`Checkpointer | None`**DEFAULT:**`None` |
| #### `store` ¶ "Copy anchor link to this section for reference") | An optional store object.<br>Used for persisting data across multiple threads (e.g., multiple<br>conversations / users).<br>**TYPE:**`BaseStore | None`**DEFAULT:**`None` |
| #### `interrupt_before` ¶ "Copy anchor link to this section for reference") | An optional list of node names to interrupt before.<br>Useful if you want to add a user confirmation or other interrupt<br>before taking an action.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| #### `interrupt_after` ¶ "Copy anchor link to this section for reference") | An optional list of node names to interrupt after.<br>Useful if you want to return directly or run additional processing<br>on an output.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| #### `debug` ¶ "Copy anchor link to this section for reference") | Whether to enable verbose logging for graph execution.<br>When enabled, prints detailed information about each node execution, state<br>updates, and transitions during agent runtime. Useful for debugging<br>middleware behavior and understanding agent execution flow.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| #### `name` ¶ "Copy anchor link to this section for reference") | An optional name for the `CompiledStateGraph`.<br>This name will be automatically used when adding the agent graph to<br>another graph as a subgraph node - particularly useful for building<br>multi-agent systems.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| #### `cache` ¶ "Copy anchor link to this section for reference") | An optional `BaseCache` instance to enable caching of graph execution.<br>**TYPE:**`BaseCache | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CompiledStateGraph[AgentState[ResponseT], ContextT, _InputAgentState, _OutputAgentState[ResponseT]]` | A compiled `StateGraph` that can be used for chat interactions. |

The agent node calls the language model with the messages list (after applying
the system prompt). If the resulting `AIMessage`
contains `tool_calls`, the graph will then call the tools. The tools node executes
the tools and adds the responses to the messages list as
`ToolMessage` objects. The agent node then calls
the language model again. The process repeats until no more `tool_calls` are present
in the response. The agent then returns the full list of messages.

Example

```
from langchain.agents import create_agent

def check_weather(location: str) -> str:
    '''Return the weather forecast for the specified location.'''
    return f"It's always sunny in {location}"

graph = create_agent(
    model="anthropic:claude-sonnet-4-5-20250929",
    tools=[check_weather],
    system_prompt="You are a helpful assistant",
)
inputs = {"messages": [{"role": "user", "content": "what is the weather in sf"}]}
for chunk in graph.stream(inputs, stream_mode="updates"):
    print(chunk)
```

### ``AgentState ¶

Bases: `TypedDict`, `Generic[ResponseT]`

State schema for the agent.

Back to top