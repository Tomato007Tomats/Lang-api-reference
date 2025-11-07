<!-- URL: page_42 -->

Skip to content

Edit this page

# LangGraph Supervisor ¶

## ``langgraph\_supervisor.supervisor ¶

| FUNCTION | DESCRIPTION |
| --- | --- |
| `create_supervisor` | Create a multi-agent supervisor. |

### ``create\_supervisor ¶

```
create_supervisor(
    agents: list[Pregel],
    *,
    model: LanguageModelLike,
    tools: list[BaseTool | Callable] | ToolNode | None = None,
    prompt: Prompt | None = None,
    response_format: StructuredResponseSchema
    | tuple[str, StructuredResponseSchema]
    | None = None,
    pre_model_hook: RunnableLike | None = None,
    post_model_hook: RunnableLike | None = None,
    parallel_tool_calls: bool = False,
    state_schema: StateSchemaType | None = None,
    context_schema: Type[Any] | None = None,
    output_mode: OutputMode = "last_message",
    add_handoff_messages: bool = True,
    handoff_tool_prefix: str | None = None,
    add_handoff_back_messages: bool | None = None,
    supervisor_name: str = "supervisor",
    include_agent_name: AgentNameMode | None = None,
    **deprecated_kwargs: Unpack[DeprecatedKwargs],
) -> StateGraph
```

Create a multi-agent supervisor.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `agents` | List of agents to manage.<br>An agent can be a LangGraph `CompiledStateGraph`,<br>a functional API workflow, or any other Pregel<br>object.<br>**TYPE:**`list[Pregel]` |
| `model` | Language model to use for the supervisor<br>**TYPE:**`LanguageModelLike` |
| `tools` | Tools to use for the supervisor<br>**TYPE:**`list[BaseTool | Callable] | ToolNode | None`**DEFAULT:**`None` |
| `prompt` | Optional prompt to use for the supervisor. Can be one of:<br>- `str`: This is converted to a `SystemMessage` and added to the beginning of the list of messages in `state["messages"]`.<br>- `SystemMessage`: this is added to the beginning of the list of messages in `state["messages"]`.<br>- `Callable`: This function should take in full graph state and the output is then passed to the language model.<br>- `Runnable`: This runnable should take in full graph state and the output is then passed to the language model.<br>**TYPE:**`Prompt | None`**DEFAULT:**`None` |
| `response_format` | An optional schema for the final supervisor output.<br>If provided, output will be formatted to match the given schema and returned in the `'structured_response'` state key.<br>If not provided, `structured_response` will not be present in the output state.<br>Can be passed in as:<br>```<br>- An OpenAI function/tool schema,<br>- A JSON Schema,<br>- A TypedDict class,<br>- A Pydantic class.<br>- A tuple `(prompt, schema)`, where schema is one of the above.<br>    The prompt will be used together with the model that is being used to generate the structured response.<br>```<br>Important<br>`response_format` requires the model to support `.with_structured_output`<br>Note<br>`response_format` requires `structured_response` key in your state schema.<br>You can use the prebuilt `langgraph.prebuilt.chat_agent_executor.AgentStateWithStructuredResponse`.<br>**TYPE:**`StructuredResponseSchema | tuple[str, StructuredResponseSchema] | None`**DEFAULT:**`None` |
| `pre_model_hook` | An optional node to add before the LLM node in the supervisor agent (i.e., the node that calls the LLM).<br>Useful for managing long message histories (e.g., message trimming, summarization, etc.).<br>Pre-model hook must be a callable or a runnable that takes in current graph state and returns a state update in the form of<br> <br>```<br># At least one of `messages` or `llm_input_messages` MUST be provided<br>{<br>    # If provided, will UPDATE the `messages` in the state<br>    "messages": [RemoveMessage(id=REMOVE_ALL_MESSAGES), ...],<br>    # If provided, will be used as the input to the LLM,<br>    # and will NOT UPDATE `messages` in the state<br>    "llm_input_messages": [...],<br>    # Any other state keys that need to be propagated<br>    ...<br>}<br>```<br>Important<br>At least one of `messages` or `llm_input_messages` MUST be provided and will be used as an input to the `agent` node.<br>The rest of the keys will be added to the graph state.<br>Warning<br>If you are returning `messages` in the pre-model hook, you should OVERWRITE the `messages` key by doing the following:<br>```<br>{<br>    "messages": [RemoveMessage(id=REMOVE_ALL_MESSAGES), *new_messages]<br>    ...<br>}<br>```<br>**TYPE:**`RunnableLike | None`**DEFAULT:**`None` |
| `post_model_hook` | An optional node to add after the LLM node in the supervisor agent (i.e., the node that calls the LLM).<br>Useful for implementing human-in-the-loop, guardrails, validation, or other post-processing.<br>Post-model hook must be a callable or a runnable that takes in current graph state and returns a state update.<br>**TYPE:**`RunnableLike | None`**DEFAULT:**`None` |
| `parallel_tool_calls` | Whether to allow the supervisor LLM to call tools in parallel (only OpenAI and Anthropic).<br>Use this to control whether the supervisor can hand off to multiple agents at once.<br>If `True`, will enable parallel tool calls.<br>If `False`, will disable parallel tool calls.<br>Important<br>This is currently supported only by OpenAI and Anthropic models.<br>To control parallel tool calling for other providers, add explicit instructions for tool use to the system prompt.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `state_schema` | State schema to use for the supervisor graph.<br>**TYPE:**`StateSchemaType | None`**DEFAULT:**`None` |
| `context_schema` | Specifies the schema for the context object that will be passed to the workflow.<br>**TYPE:**`Type[Any] | None`**DEFAULT:**`None` |
| `output_mode` | Mode for adding managed agents' outputs to the message history in the multi-agent workflow.<br>Can be one of:<br>- `full_history`: Add the entire agent message history<br>- `last_message`: Add only the last message<br>**TYPE:**`OutputMode`**DEFAULT:**`'last_message'` |
| `add_handoff_messages` | Whether to add a pair of `(AIMessage, ToolMessage)` to the message history<br>when a handoff occurs.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `handoff_tool_prefix` | Optional prefix for the handoff tools (e.g., `'delegate_to_'` or `'transfer_to_'`)<br>If provided, the handoff tools will be named `handoff_tool_prefix_agent_name`.<br>If not provided, the handoff tools will be named `transfer_to_agent_name`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `add_handoff_back_messages` | Whether to add a pair of `(AIMessage, ToolMessage)` to the message history<br>when returning control to the supervisor to indicate that a handoff has occurred.<br>**TYPE:**`bool | None`**DEFAULT:**`None` |
| `supervisor_name` | Name of the supervisor node.<br>**TYPE:**`str`**DEFAULT:**`'supervisor'` |
| `include_agent_name` | Use to specify how to expose the agent name to the underlying supervisor LLM.<br>- `None`: Relies on the LLM provider using the name attribute on the AI message. Currently, only OpenAI supports this.<br>- `'inline'`: Add the agent name directly into the content field of the AI message using XML-style tags.<br>Example: `"How can I help you"` -\> `"<name>agent_name</name><content>How can I help you?</content>"`<br>**TYPE:**`AgentNameMode | None`**DEFAULT:**`None` |

Example

```
from langchain_openai import ChatOpenAI

from langgraph_supervisor import create_supervisor
from langgraph.prebuilt import create_react_agent

# Create specialized agents

def add(a: float, b: float) -> float:
    '''Add two numbers.'''
    return a + b

def web_search(query: str) -> str:
    '''Search the web for information.'''
    return 'Here are the headcounts for each of the FAANG companies in 2024...'

math_agent = create_react_agent(
    model="openai:gpt-4o",
    tools=[add],
    name="math_expert",
)

research_agent = create_react_agent(
    model="openai:gpt-4o",
    tools=[web_search],
    name="research_expert",
)

# Create supervisor workflow
workflow = create_supervisor(
    [research_agent, math_agent],
    model=ChatOpenAI(model="gpt-4o"),
)

# Compile and run
app = workflow.compile()
result = app.invoke({
    "messages": [\
        {\
            "role": "user",\
            "content": "what's the combined headcount of the FAANG companies in 2024?"\
        }\
    ]
})
```

## ``langgraph\_supervisor.handoff ¶

| FUNCTION | DESCRIPTION |
| --- | --- |
| `create_handoff_tool` | Create a tool that can handoff control to the requested agent. |
| `create_forward_message_tool` | Create a tool the supervisor can use to forward a worker message by name. |

### ``create\_handoff\_tool ¶

```
create_handoff_tool(
    *,
    agent_name: str,
    name: str | None = None,
    description: str | None = None,
    add_handoff_messages: bool = True,
) -> BaseTool
```

Create a tool that can handoff control to the requested agent.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `agent_name` | The name of the agent to handoff control to, i.e.<br>the name of the agent node in the multi-agent graph.<br>Agent names should be simple, clear and unique, preferably in snake\_case,<br>although you are only limited to the names accepted by LangGraph<br>nodes as well as the tool names accepted by LLM providers<br>(the tool name will look like this: `transfer_to_<agent_name>`).<br>**TYPE:**`str` |
| `name` | Optional name of the tool to use for the handoff.<br>If not provided, the tool name will be `transfer_to_<agent_name>`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `description` | Optional description for the handoff tool.<br>If not provided, the description will be `Ask agent <agent_name> for help`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `add_handoff_messages` | Whether to add handoff messages to the message history.<br>If `False`, the handoff messages will be omitted from the message history.<br>**TYPE:**`bool`**DEFAULT:**`True` |

### ``create\_forward\_message\_tool ¶

```
create_forward_message_tool(supervisor_name: str = 'supervisor') -> BaseTool
```

Create a tool the supervisor can use to forward a worker message by name.

This helps avoid information loss any time the supervisor rewrites a worker query
to the user and also can save some tokens.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `supervisor_name` | The name of the supervisor node (used for namespacing the tool).<br>**TYPE:**`str`**DEFAULT:**`'supervisor'` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `BaseTool` | The 'forward\_message' tool.<br>**TYPE:**`BaseTool` |

Back to top