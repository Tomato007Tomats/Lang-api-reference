<!-- URL: page_5 -->

Skip to content

Edit this page

# Graphs

## ``langgraph.graph.state.StateGraph ¶

Bases: `Generic[StateT, ContextT, InputT, OutputT]`

A graph whose nodes communicate by reading and writing to a shared state.
The signature of each node is State -> Partial.

Each state key can optionally be annotated with a reducer function that
will be used to aggregate the values of that key received from multiple nodes.
The signature of a reducer function is `(Value, Value) -> Value`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `state_schema` | The schema class that defines the state.<br>**TYPE:**`type[StateT]` |
| `context_schema` | The schema class that defines the runtime context.<br>Use this to expose immutable context data to your nodes, like `user_id`, `db_conn`, etc.<br>**TYPE:**`type[ContextT] | None`**DEFAULT:**`None` |
| `input_schema` | The schema class that defines the input to the graph.<br>**TYPE:**`type[InputT] | None`**DEFAULT:**`None` |
| `output_schema` | The schema class that defines the output from the graph.<br>**TYPE:**`type[OutputT] | None`**DEFAULT:**`None` |

`config_schema` Deprecated

The `config_schema` parameter is deprecated in v0.6.0 and support will be removed in v2.0.0.
Please use `context_schema` instead to specify the schema for run-scoped context.

Example

```
from langchain_core.runnables import RunnableConfig
from typing_extensions import Annotated, TypedDict
from langgraph.checkpoint.memory import InMemorySaver
from langgraph.graph import StateGraph
from langgraph.runtime import Runtime

def reducer(a: list, b: int | None) -> list:
    if b is not None:
        return a + [b]
    return a

class State(TypedDict):
    x: Annotated[list, reducer]

class Context(TypedDict):
    r: float

graph = StateGraph(state_schema=State, context_schema=Context)

def node(state: State, runtime: Runtime[Context]) -> dict:
    r = runtime.context.get("r", 1.0)
    x = state"x"
    next_value = x * r * (1 - x)
    return {"x": next_value}

graph.add_node("A", node)
graph.set_entry_point("A")
graph.set_finish_point("A")
compiled = graph.compile()

step1 = compiled.invoke({"x": 0.5}, context={"r": 3.0})
# {'x': [0.5, 0.75]}
```

| METHOD | DESCRIPTION |
| --- | --- |
| `add_node` | Add a new node to the state graph. |
| `add_edge` | Add a directed edge from the start node (or list of start nodes) to the end node. |
| `add_conditional_edges` | Add a conditional edge from the starting node to any number of destination nodes. |
| `add_sequence` | Add a sequence of nodes that will be executed in the provided order. |
| `compile` | Compiles the state graph into a `CompiledStateGraph` object. |

### ``add\_node ¶

```
add_node(
    node: str | StateNode[NodeInputT, ContextT],
    action: StateNode[NodeInputT, ContextT] | None = None,
    *,
    defer: bool = False,
    metadata: dict[str, Any] | None = None,
    input_schema: type[NodeInputT] | None = None,
    retry_policy: RetryPolicy | Sequence[RetryPolicy] | None = None,
    cache_policy: CachePolicy | None = None,
    destinations: dict[str, str] | tuple[str, ...] | None = None,
    **kwargs: Unpack[DeprecatedKwargs],
) -> Self
```

Add a new node to the state graph.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `node` | The function or runnable this node will run.<br>If a string is provided, it will be used as the node name, and action will be used as the function or runnable.<br>**TYPE:**`str | StateNode[NodeInputT, ContextT]` |
| `action` | The action associated with the node.<br>Will be used as the node function or runnable if `node` is a string (node name).<br>**TYPE:**`StateNode[NodeInputT, ContextT] | None`**DEFAULT:**`None` |
| `defer` | Whether to defer the execution of the node until the run is about to end.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `metadata` | The metadata associated with the node.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `input_schema` | The input schema for the node. (default: the graph's state schema)<br>**TYPE:**`type[NodeInputT] | None`**DEFAULT:**`None` |
| `retry_policy` | The retry policy for the node.<br>If a sequence is provided, the first matching policy will be applied.<br>**TYPE:**`RetryPolicy | Sequence[RetryPolicy] | None`**DEFAULT:**`None` |
| `cache_policy` | The cache policy for the node.<br>**TYPE:**`CachePolicy | None`**DEFAULT:**`None` |
| `destinations` | Destinations that indicate where a node can route to.<br>This is useful for edgeless graphs with nodes that return `Command` objects.<br>If a `dict` is provided, the keys will be used as the target node names and the values will be used as the labels for the edges.<br>If a `tuple` is provided, the values will be used as the target node names.<br>Note<br>This is only used for graph rendering and doesn't have any effect on the graph execution.<br>**TYPE:**`dict[str, str] | tuple[str, ...] | None`**DEFAULT:**`None` |

Example

```
from typing_extensions import TypedDict

from langchain_core.runnables import RunnableConfig
from langgraph.graph import START, StateGraph

class State(TypedDict):
    x: int

def my_node(state: State, config: RunnableConfig) -> State:
    return {"x": state["x"] + 1}

builder = StateGraph(State)
builder.add_node(my_node)  # node name will be 'my_node'
builder.add_edge(START, "my_node")
graph = builder.compile()
graph.invoke({"x": 1})
# {'x': 2}
```

Customize the name:

```
builder = StateGraph(State)
builder.add_node("my_fair_node", my_node)
builder.add_edge(START, "my_fair_node")
graph = builder.compile()
graph.invoke({"x": 1})
# {'x': 2}
```

| RETURNS | DESCRIPTION |
| --- | --- |
| `Self` | The instance of the state graph, allowing for method chaining.<br>**TYPE:**`Self` |

### ``add\_edge ¶

```
add_edge(start_key: str | list[str], end_key: str) -> Self
```

Add a directed edge from the start node (or list of start nodes) to the end node.

When a single start node is provided, the graph will wait for that node to complete
before executing the end node. When multiple start nodes are provided,
the graph will wait for ALL of the start nodes to complete before executing the end node.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `start_key` | The key(s) of the start node(s) of the edge.<br>**TYPE:**`str | list[str]` |
| `end_key` | The key of the end node of the edge.<br>**TYPE:**`str` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the start key is `'END'` or if the start key or end key is not present in the graph. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Self` | The instance of the state graph, allowing for method chaining.<br>**TYPE:**`Self` |

### ``add\_conditional\_edges ¶

```
add_conditional_edges(
    source: str,
    path: Callable[..., Hashable | Sequence[Hashable]]
    | Callable[..., Awaitable[Hashable | Sequence[Hashable]]]
    | Runnable[Any, Hashable | Sequence[Hashable]],
    path_map: dict[Hashable, str] | list[str] | None = None,
) -> Self
```

Add a conditional edge from the starting node to any number of destination nodes.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `source` | The starting node. This conditional edge will run when<br>exiting this node.<br>**TYPE:**`str` |
| `path` | The callable that determines the next<br>node or nodes. If not specifying `path_map` it should return one or<br>more nodes. If it returns `'END'`, the graph will stop execution.<br>**TYPE:**`Callable[..., Hashable | Sequence[Hashable]] | Callable[..., Awaitable[Hashable | Sequence[Hashable]]] | Runnable[Any, Hashable | Sequence[Hashable]]` |
| `path_map` | Optional mapping of paths to node<br>names. If omitted the paths returned by `path` should be node names.<br>**TYPE:**`dict[Hashable, str] | list[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Self` | The instance of the graph, allowing for method chaining.<br>**TYPE:**`Self` |

Warning

Without type hints on the `path` function's return value (e.g., `-> Literal["foo", "__end__"]:`)
or a path\_map, the graph visualization assumes the edge could transition to any node in the graph.

### ``add\_sequence ¶

```
add_sequence(
    nodes: Sequence[\
        StateNode[NodeInputT, ContextT] | tuple[str, StateNode[NodeInputT, ContextT]]\
    ],
) -> Self
```

Add a sequence of nodes that will be executed in the provided order.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `nodes` | A sequence of `StateNode` (callables that accept a `state` arg) or `(name, StateNode)` tuples.<br>If no names are provided, the name will be inferred from the node object (e.g. a `Runnable` or a `Callable` name).<br>Each node will be executed in the order provided.<br>**TYPE:**`Sequence[StateNode[NodeInputT, ContextT] | tuple[str, StateNode[NodeInputT, ContextT]]]` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the sequence is empty. |
| `ValueError` | If the sequence contains duplicate node names. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Self` | The instance of the state graph, allowing for method chaining.<br>**TYPE:**`Self` |

### ``compile ¶

```
compile(
    checkpointer: Checkpointer = None,
    *,
    cache: BaseCache | None = None,
    store: BaseStore | None = None,
    interrupt_before: All | list[str] | None = None,
    interrupt_after: All | list[str] | None = None,
    debug: bool = False,
    name: str | None = None,
) -> CompiledStateGraph[StateT, ContextT, InputT, OutputT]
```

Compiles the state graph into a `CompiledStateGraph` object.

The compiled graph implements the `Runnable` interface and can be invoked,
streamed, batched, and run asynchronously.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `checkpointer` | A checkpoint saver object or flag.<br>If provided, this `Checkpointer` serves as a fully versioned "short-term memory" for the graph,<br>allowing it to be paused, resumed, and replayed from any point.<br>If `None`, it may inherit the parent graph's checkpointer when used as a subgraph.<br>If `False`, it will not use or inherit any checkpointer.<br>**TYPE:**`Checkpointer`**DEFAULT:**`None` |
| `interrupt_before` | An optional list of node names to interrupt before.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | An optional list of node names to interrupt after.<br>**TYPE:**`All | list[str] | None`**DEFAULT:**`None` |
| `debug` | A flag indicating whether to enable debug mode.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `name` | The name to use for the compiled graph.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `CompiledStateGraph` | The compiled state graph.<br>**TYPE:**`CompiledStateGraph[StateT, ContextT, InputT, OutputT]` |

## ``langgraph.graph.state.CompiledStateGraph ¶

Bases: `Pregel[StateT, ContextT, InputT, OutputT]`, `Generic[StateT, ContextT, InputT, OutputT]`

| METHOD | DESCRIPTION |
| --- | --- |
| `stream` | Stream graph steps for a single input. |
| `astream` | Asynchronously stream graph steps for a single input. |
| `invoke` | Run the graph with a single input and config. |
| `ainvoke` | Asynchronously invoke the graph on a single input. |
| `get_state` | Get the current state of the graph. |
| `aget_state` | Get the current state of the graph. |
| `get_state_history` | Get the history of the state of the graph. |
| `aget_state_history` | Asynchronously get the history of the state of the graph. |
| `update_state` | Update the state of the graph with the given values, as if they came from |
| `aupdate_state` | Asynchronously update the state of the graph with the given values, as if they came from |
| `bulk_update_state` | Apply updates to the graph state in bulk. Requires a checkpointer to be set. |
| `abulk_update_state` | Asynchronously apply updates to the graph state in bulk. Requires a checkpointer to be set. |
| `get_graph` | Return a drawable representation of the computation graph. |
| `aget_graph` | Return a drawable representation of the computation graph. |
| `get_subgraphs` | Get the subgraphs of the graph. |
| `aget_subgraphs` | Get the subgraphs of the graph. |
| `with_config` | Create a copy of the Pregel object with an updated config. |

### ``stream ¶

```
stream(
    input: InputT | Command | None,
    config: RunnableConfig | None = None,
    *,
    context: ContextT | None = None,
    stream_mode: StreamMode | Sequence[StreamMode] | None = None,
    print_mode: StreamMode | Sequence[StreamMode] = (),
    output_keys: str | Sequence[str] | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    durability: Durability | None = None,
    subgraphs: bool = False,
    debug: bool | None = None,
    **kwargs: Unpack[DeprecatedKwargs],
) -> Iterator[dict[str, Any] | Any]
```

Stream graph steps for a single input.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the graph.<br>**TYPE:**`InputT | Command | None` |
| `config` | The configuration to use for the run.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `context` | The static context to use for the run.<br>Added in version 0.6.0<br>**TYPE:**`ContextT | None`**DEFAULT:**`None` |
| `stream_mode` | The mode to stream output, defaults to `self.stream_mode`.<br>Options are:<br>- `"values"`: Emit all values in the state after each step, including interrupts.<br>When used with functional API, values are emitted once at the end of the workflow.<br>- `"updates"`: Emit only the node or task names and updates returned by the nodes or tasks after each step.<br>If multiple updates are made in the same step (e.g. multiple nodes are run) then those updates are emitted separately.<br>- `"custom"`: Emit custom data from inside nodes or tasks using `StreamWriter`.<br>- `"messages"`: Emit LLM messages token-by-token together with metadata for any LLM invocations inside nodes or tasks.<br>Will be emitted as 2-tuples `(LLM token, metadata)`.<br>- `"checkpoints"`: Emit an event when a checkpoint is created, in the same format as returned by `get_state()`.<br>- `"tasks"`: Emit events when tasks start and finish, including their results and errors.<br>You can pass a list as the `stream_mode` parameter to stream multiple modes at once.<br>The streamed outputs will be tuples of `(mode, data)`.<br>See LangGraph streaming guide for more details.<br>**TYPE:**`StreamMode | Sequence[StreamMode] | None`**DEFAULT:**`None` |
| `print_mode` | Accepts the same values as `stream_mode`, but only prints the output to the console, for debugging purposes. Does not affect the output of the graph in any way.<br>**TYPE:**`StreamMode | Sequence[StreamMode]`**DEFAULT:**`()` |
| `output_keys` | The keys to stream, defaults to all non-context channels.<br>**TYPE:**`str | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt before, defaults to all nodes in the graph.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to interrupt after, defaults to all nodes in the graph.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `durability` | The durability mode for the graph execution, defaults to `"async"`.<br>Options are:<br>- `"sync"`: Changes are persisted synchronously before the next step starts.<br>- `"async"`: Changes are persisted asynchronously while the next step executes.<br>- `"exit"`: Changes are persisted only when the graph exits.<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |
| `subgraphs` | Whether to stream events from inside subgraphs, defaults to False.<br>If `True`, the events will be emitted as tuples `(namespace, data)`,<br>or `(namespace, mode, data)` if `stream_mode` is a list,<br>where `namespace` is a tuple with the path to the node where a subgraph is invoked,<br>e.g. `("parent_node:<task_id>", "child_node:<task_id>")`.<br>See LangGraph streaming guide for more details.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `dict[str, Any] | Any` | The output of each step in the graph. The output shape depends on the `stream_mode`. |

### ``astream`async`¶

```
astream(
    input: InputT | Command | None,
    config: RunnableConfig | None = None,
    *,
    context: ContextT | None = None,
    stream_mode: StreamMode | Sequence[StreamMode] | None = None,
    print_mode: StreamMode | Sequence[StreamMode] = (),
    output_keys: str | Sequence[str] | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    durability: Durability | None = None,
    subgraphs: bool = False,
    debug: bool | None = None,
    **kwargs: Unpack[DeprecatedKwargs],
) -> AsyncIterator[dict[str, Any] | Any]
```

Asynchronously stream graph steps for a single input.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the graph.<br>**TYPE:**`InputT | Command | None` |
| `config` | The configuration to use for the run.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `context` | The static context to use for the run.<br>Added in version 0.6.0<br>**TYPE:**`ContextT | None`**DEFAULT:**`None` |
| `stream_mode` | The mode to stream output, defaults to `self.stream_mode`.<br>Options are:<br>- `"values"`: Emit all values in the state after each step, including interrupts.<br>When used with functional API, values are emitted once at the end of the workflow.<br>- `"updates"`: Emit only the node or task names and updates returned by the nodes or tasks after each step.<br>If multiple updates are made in the same step (e.g. multiple nodes are run) then those updates are emitted separately.<br>- `"custom"`: Emit custom data from inside nodes or tasks using `StreamWriter`.<br>- `"messages"`: Emit LLM messages token-by-token together with metadata for any LLM invocations inside nodes or tasks.<br>Will be emitted as 2-tuples `(LLM token, metadata)`.<br>- `"debug"`: Emit debug events with as much information as possible for each step.<br>You can pass a list as the `stream_mode` parameter to stream multiple modes at once.<br>The streamed outputs will be tuples of `(mode, data)`.<br>See LangGraph streaming guide for more details.<br>**TYPE:**`StreamMode | Sequence[StreamMode] | None`**DEFAULT:**`None` |
| `print_mode` | Accepts the same values as `stream_mode`, but only prints the output to the console, for debugging purposes. Does not affect the output of the graph in any way.<br>**TYPE:**`StreamMode | Sequence[StreamMode]`**DEFAULT:**`()` |
| `output_keys` | The keys to stream, defaults to all non-context channels.<br>**TYPE:**`str | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_before` | Nodes to interrupt before, defaults to all nodes in the graph.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | Nodes to interrupt after, defaults to all nodes in the graph.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `durability` | The durability mode for the graph execution, defaults to `"async"`.<br>Options are:<br>- `"sync"`: Changes are persisted synchronously before the next step starts.<br>- `"async"`: Changes are persisted asynchronously while the next step executes.<br>- `"exit"`: Changes are persisted only when the graph exits.<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |
| `subgraphs` | Whether to stream events from inside subgraphs, defaults to False.<br>If `True`, the events will be emitted as tuples `(namespace, data)`,<br>or `(namespace, mode, data)` if `stream_mode` is a list,<br>where `namespace` is a tuple with the path to the node where a subgraph is invoked,<br>e.g. `("parent_node:<task_id>", "child_node:<task_id>")`.<br>See LangGraph streaming guide for more details.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[dict[str, Any] | Any]` | The output of each step in the graph. The output shape depends on the `stream_mode`. |

### ``invoke ¶

```
invoke(
    input: InputT | Command | None,
    config: RunnableConfig | None = None,
    *,
    context: ContextT | None = None,
    stream_mode: StreamMode = "values",
    print_mode: StreamMode | Sequence[StreamMode] = (),
    output_keys: str | Sequence[str] | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    durability: Durability | None = None,
    **kwargs: Any,
) -> dict[str, Any] | Any
```

Run the graph with a single input and config.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input data for the graph. It can be a dictionary or any other type.<br>**TYPE:**`InputT | Command | None` |
| `config` | The configuration for the graph run.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `context` | The static context to use for the run.<br>Added in version 0.6.0<br>**TYPE:**`ContextT | None`**DEFAULT:**`None` |
| `stream_mode` | The stream mode for the graph run.<br>**TYPE:**`StreamMode`**DEFAULT:**`'values'` |
| `print_mode` | Accepts the same values as `stream_mode`, but only prints the output to the console, for debugging purposes. Does not affect the output of the graph in any way.<br>**TYPE:**`StreamMode | Sequence[StreamMode]`**DEFAULT:**`()` |
| `output_keys` | The output keys to retrieve from the graph run.<br>**TYPE:**`str | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_before` | The nodes to interrupt the graph run before.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | The nodes to interrupt the graph run after.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `durability` | The durability mode for the graph execution, defaults to `"async"`.<br>Options are:<br>- `"sync"`: Changes are persisted synchronously before the next step starts.<br>- `"async"`: Changes are persisted asynchronously while the next step executes.<br>- `"exit"`: Changes are persisted only when the graph exits.<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the graph run.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any] | Any` | The output of the graph run. If `stream_mode` is `"values"`, it returns the latest output. |
| `dict[str, Any] | Any` | If `stream_mode` is not `"values"`, it returns a list of output chunks. |

### ``ainvoke`async`¶

```
ainvoke(
    input: InputT | Command | None,
    config: RunnableConfig | None = None,
    *,
    context: ContextT | None = None,
    stream_mode: StreamMode = "values",
    print_mode: StreamMode | Sequence[StreamMode] = (),
    output_keys: str | Sequence[str] | None = None,
    interrupt_before: All | Sequence[str] | None = None,
    interrupt_after: All | Sequence[str] | None = None,
    durability: Durability | None = None,
    **kwargs: Any,
) -> dict[str, Any] | Any
```

Asynchronously invoke the graph on a single input.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input data for the computation. It can be a dictionary or any other type.<br>**TYPE:**`InputT | Command | None` |
| `config` | The configuration for the computation.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `context` | The static context to use for the run.<br>Added in version 0.6.0<br>**TYPE:**`ContextT | None`**DEFAULT:**`None` |
| `stream_mode` | The stream mode for the computation.<br>**TYPE:**`StreamMode`**DEFAULT:**`'values'` |
| `print_mode` | Accepts the same values as `stream_mode`, but only prints the output to the console, for debugging purposes. Does not affect the output of the graph in any way.<br>**TYPE:**`StreamMode | Sequence[StreamMode]`**DEFAULT:**`()` |
| `output_keys` | The output keys to include in the result.<br>**TYPE:**`str | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_before` | The nodes to interrupt before.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `interrupt_after` | The nodes to interrupt after.<br>**TYPE:**`All | Sequence[str] | None`**DEFAULT:**`None` |
| `durability` | The durability mode for the graph execution, defaults to `"async"`.<br>Options are:<br>- `"sync"`: Changes are persisted synchronously before the next step starts.<br>- `"async"`: Changes are persisted asynchronously while the next step executes.<br>- `"exit"`: Changes are persisted only when the graph exits.<br>**TYPE:**`Durability | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any] | Any` | The result of the computation. If `stream_mode` is `"values"`, it returns the latest value. |
| `dict[str, Any] | Any` | If `stream_mode` is `"chunks"`, it returns a list of chunks. |

### ``get\_state ¶

```
get_state(config: RunnableConfig, *, subgraphs: bool = False) -> StateSnapshot
```

Get the current state of the graph.

### ``aget\_state`async`¶

```
aget_state(config: RunnableConfig, *, subgraphs: bool = False) -> StateSnapshot
```

Get the current state of the graph.

### ``get\_state\_history ¶

```
get_state_history(
    config: RunnableConfig,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> Iterator[StateSnapshot]
```

Get the history of the state of the graph.

### ``aget\_state\_history`async`¶

```
aget_state_history(
    config: RunnableConfig,
    *,
    filter: dict[str, Any] | None = None,
    before: RunnableConfig | None = None,
    limit: int | None = None,
) -> AsyncIterator[StateSnapshot]
```

Asynchronously get the history of the state of the graph.

### ``update\_state ¶

```
update_state(
    config: RunnableConfig,
    values: dict[str, Any] | Any | None,
    as_node: str | None = None,
    task_id: str | None = None,
) -> RunnableConfig
```

Update the state of the graph with the given values, as if they came from
node `as_node`. If `as_node` is not provided, it will be set to the last node
that updated the state, if not ambiguous.

### ``aupdate\_state`async`¶

```
aupdate_state(
    config: RunnableConfig,
    values: dict[str, Any] | Any,
    as_node: str | None = None,
    task_id: str | None = None,
) -> RunnableConfig
```

Asynchronously update the state of the graph with the given values, as if they came from
node `as_node`. If `as_node` is not provided, it will be set to the last node
that updated the state, if not ambiguous.

### ``bulk\_update\_state ¶

```
bulk_update_state(
    config: RunnableConfig, supersteps: Sequence[Sequence[StateUpdate]]
) -> RunnableConfig
```

Apply updates to the graph state in bulk. Requires a checkpointer to be set.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to apply the updates to.<br>**TYPE:**`RunnableConfig` |
| `supersteps` | A list of supersteps, each including a list of updates to apply sequentially to a graph state.<br>Each update is a tuple of the form `(values, as_node, task_id)` where `task_id` is optional.<br>**TYPE:**`Sequence[Sequence[StateUpdate]]` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If no checkpointer is set or no updates are provided. |
| `InvalidUpdateError` | If an invalid update is provided. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | The updated config.<br>**TYPE:**`RunnableConfig` |

### ``abulk\_update\_state`async`¶

```
abulk_update_state(
    config: RunnableConfig, supersteps: Sequence[Sequence[StateUpdate]]
) -> RunnableConfig
```

Asynchronously apply updates to the graph state in bulk. Requires a checkpointer to be set.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to apply the updates to.<br>**TYPE:**`RunnableConfig` |
| `supersteps` | A list of supersteps, each including a list of updates to apply sequentially to a graph state.<br>Each update is a tuple of the form `(values, as_node, task_id)` where `task_id` is optional.<br>**TYPE:**`Sequence[Sequence[StateUpdate]]` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If no checkpointer is set or no updates are provided. |
| `InvalidUpdateError` | If an invalid update is provided. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableConfig` | The updated config.<br>**TYPE:**`RunnableConfig` |

### ``get\_graph ¶

```
get_graph(config: RunnableConfig | None = None, *, xray: int | bool = False) -> Graph
```

Return a drawable representation of the computation graph.

### ``aget\_graph`async`¶

```
aget_graph(config: RunnableConfig | None = None, *, xray: int | bool = False) -> Graph
```

Return a drawable representation of the computation graph.

### ``get\_subgraphs ¶

```
get_subgraphs(
    *, namespace: str | None = None, recurse: bool = False
) -> Iterator[tuple[str, PregelProtocol]]
```

Get the subgraphs of the graph.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `namespace` | The namespace to filter the subgraphs by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `recurse` | Whether to recurse into the subgraphs.<br>If `False`, only the immediate subgraphs will be returned.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Iterator[tuple[str, PregelProtocol]]` | An iterator of the `(namespace, subgraph)` pairs. |

### ``aget\_subgraphs`async`¶

```
aget_subgraphs(
    *, namespace: str | None = None, recurse: bool = False
) -> AsyncIterator[tuple[str, PregelProtocol]]
```

Get the subgraphs of the graph.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `namespace` | The namespace to filter the subgraphs by.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `recurse` | Whether to recurse into the subgraphs.<br>If `False`, only the immediate subgraphs will be returned.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[tuple[str, PregelProtocol]]` | An iterator of the `(namespace, subgraph)` pairs. |

### ``with\_config ¶

```
with_config(config: RunnableConfig | None = None, **kwargs: Any) -> Self
```

Create a copy of the Pregel object with an updated config.

## ``langgraph.graph.message ¶

| FUNCTION | DESCRIPTION |
| --- | --- |
| `add_messages` | Merges two lists of messages, updating existing messages by ID. |

### ``add\_messages ¶

```
add_messages(
    left: Messages,
    right: Messages,
    *,
    format: Literal["langchain-openai"] | None = None,
) -> Messages
```

Merges two lists of messages, updating existing messages by ID.

By default, this ensures the state is "append-only", unless the
new message has the same ID as an existing message.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `left` | The base list of `Messages`.<br>**TYPE:**`Messages` |
| `right` | The list of `Messages` (or single `Message`) to merge<br>into the base list.<br>**TYPE:**`Messages` |
| `format` | The format to return messages in. If `None` then `Messages` will be<br>returned as is. If `langchain-openai` then `Messages` will be returned as<br>`BaseMessage` objects with their contents formatted to match OpenAI message<br>format, meaning contents can be string, `'text'` blocks, or `'image_url'` blocks<br>and tool responses are returned as their own `ToolMessage` objects.<br>Requirement<br>Must have `langchain-core>=0.3.11` installed to use this feature.<br>**TYPE:**`Literal['langchain-openai'] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Messages` | A new list of messages with the messages from `right` merged into `left`. |
| `Messages` | If a message in `right` has the same ID as a message in `left`, the<br>message from `right` will replace the message from `left`. |

Example

Basic usage

```
from langchain_core.messages import AIMessage, HumanMessage

msgs1 = [HumanMessage(content="Hello", id="1")]
msgs2 = [AIMessage(content="Hi there!", id="2")]
add_messages(msgs1, msgs2)
# [HumanMessage(content='Hello', id='1'), AIMessage(content='Hi there!', id='2')]
```

Overwrite existing message

```
msgs1 = [HumanMessage(content="Hello", id="1")]
msgs2 = [HumanMessage(content="Hello again", id="1")]
add_messages(msgs1, msgs2)
# [HumanMessage(content='Hello again', id='1')]
```

Use in a StateGraph

```
from typing import Annotated
from typing_extensions import TypedDict
from langgraph.graph import StateGraph

class State(TypedDict):
    messages: Annotated[list, add_messages]

builder = StateGraph(State)
builder.add_node("chatbot", lambda state: {"messages": [("assistant", "Hello")]})
builder.set_entry_point("chatbot")
builder.set_finish_point("chatbot")
graph = builder.compile()
graph.invoke({})
# {'messages': [AIMessage(content='Hello', id=...)]}
```

Use OpenAI message format

```
from typing import Annotated
from typing_extensions import TypedDict
from langgraph.graph import StateGraph, add_messages

class State(TypedDict):
    messages: Annotated[list, add_messages(format="langchain-openai")]

def chatbot_node(state: State) -> list:
    return {
        "messages": [\
            {\
                "role": "user",\
                "content": [\
                    {\
                        "type": "text",\
                        "text": "Here's an image:",\
                        "cache_control": {"type": "ephemeral"},\
                    },\
                    {\
                        "type": "image",\
                        "source": {\
                            "type": "base64",\
                            "media_type": "image/jpeg",\
                            "data": "1234",\
                        },\
                    },\
                ],\
            },\
        ]
    }

builder = StateGraph(State)
builder.add_node("chatbot", chatbot_node)
builder.set_entry_point("chatbot")
builder.set_finish_point("chatbot")
graph = builder.compile()
graph.invoke({"messages": []})
# {
#     'messages': [\
#         HumanMessage(\
#             content=[\
#                 {"type": "text", "text": "Here's an image:"},\
#                 {\
#                     "type": "image_url",\
#                     "image_url": {"url": "data:image/jpeg;base64,1234"},\
#                 },\
#             ],\
#         ),\
#     ]
# }
```

Back to top