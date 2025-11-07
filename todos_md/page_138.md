<!-- URL: page_138 -->

Skip to content

Edit this page

# Pregel

## ``langgraph.pregel.NodeBuilder ¶

| METHOD | DESCRIPTION |
| --- | --- |
| `subscribe_only` | Subscribe to a single channel. |
| `subscribe_to` | Add channels to subscribe to. Node will be invoked when any of these |
| `read_from` | Adds the specified channels to read from, without subscribing to them. |
| `do` | Adds the specified node. |
| `write_to` | Add channel writes. |
| `meta` | Add tags or metadata to the node. |
| `build` | Builds the node. |

### ``subscribe\_only ¶

```
subscribe_only(channel: str) -> Self
```

Subscribe to a single channel.

### ``subscribe\_to ¶

```
subscribe_to(*channels: str, read: bool = True) -> Self
```

Add channels to subscribe to. Node will be invoked when any of these
channels are updated, with a dict of the channel values as input.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `channels` | Channel name(s) to subscribe to<br>**TYPE:**`str`**DEFAULT:**`()` |
| `read` | If `True`, the channels will be included in the input to the node.<br>Otherwise, they will trigger the node without being sent in input.<br>**TYPE:**`bool`**DEFAULT:**`True` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Self` | Self for chaining |

### ``read\_from ¶

```
read_from(*channels: str) -> Self
```

Adds the specified channels to read from, without subscribing to them.

### ``do ¶

```
do(node: RunnableLike) -> Self
```

Adds the specified node.

### ``write\_to ¶

```
write_to(*channels: str | ChannelWriteEntry, **kwargs: _WriteValue) -> Self
```

Add channel writes.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `*channels` | Channel names to write to<br>**TYPE:**`str | ChannelWriteEntry`**DEFAULT:**`()` |
| `**kwargs` | Channel name and value mappings<br>**TYPE:**`_WriteValue`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Self` | Self for chaining |

### ``meta ¶

```
meta(*tags: str, **metadata: Any) -> Self
```

Add tags or metadata to the node.

### ``build ¶

```
build() -> PregelNode
```

Builds the node.

## ``langgraph.pregel.Pregel ¶

Bases: `PregelProtocol[StateT, ContextT, InputT, OutputT]`, `Generic[StateT, ContextT, InputT, OutputT]`

Pregel manages the runtime behavior for LangGraph applications.

#### Overview ¶

Pregel combines **actors**
and **channels** into a single application.
**Actors** read data from channels and write data to channels.
Pregel organizes the execution of the application into multiple steps,
following the **Pregel Algorithm**/ **Bulk Synchronous Parallel** model.

Each step consists of three phases:

- **Plan**: Determine which **actors** to execute in this step. For example,
in the first step, select the **actors** that subscribe to the special
**input** channels; in subsequent steps,
select the **actors** that subscribe to channels updated in the previous step.
- **Execution**: Execute all selected **actors** in parallel,
until all complete, or one fails, or a timeout is reached. During this
phase, channel updates are invisible to actors until the next step.
- **Update**: Update the channels with the values written by the **actors**
in this step.

Repeat until no **actors** are selected for execution, or a maximum number of
steps is reached.

#### Actors ¶

An **actor** is a `PregelNode`.
It subscribes to channels, reads data from them, and writes data to them.
It can be thought of as an **actor** in the Pregel algorithm.
`PregelNodes` implement LangChain's
Runnable interface.

#### Channels ¶

Channels are used to communicate between actors (`PregelNodes`).
Each channel has a value type, an update type, and an update function – which
takes a sequence of updates and
modifies the stored value. Channels can be used to send data from one chain to
another, or to send data from a chain to itself in a future step. LangGraph
provides a number of built-in channels:

##### Basic channels: LastValue and Topic ¶

- `LastValue`: The default channel, stores the last value sent to the channel,
useful for input and output values, or for sending data from one step to the next
- `Topic`: A configurable PubSub Topic, useful for sending multiple values
between _actors_, or for accumulating output. Can be configured to deduplicate
values, and/or to accumulate values over the course of multiple steps.

##### Advanced channels: Context and BinaryOperatorAggregate ¶

- `Context`: exposes the value of a context manager, managing its lifecycle.
Useful for accessing external resources that require setup and/or teardown. eg.
`client = Context(httpx.Client)`
- `BinaryOperatorAggregate`: stores a persistent value, updated by applying
a binary operator to the current value and each update
sent to the channel, useful for computing aggregates over multiple steps. eg.
`total = BinaryOperatorAggregate(int, operator.add)`

#### Examples ¶

Most users will interact with Pregel via a
StateGraph (Graph API) or via an
entrypoint (Functional API).

However, for **advanced** use cases, Pregel can be used directly. If you're
not sure whether you need to use Pregel directly, then the answer is probably no
\- you should use the Graph API or Functional API instead. These are higher-level
interfaces that will compile down to Pregel under the hood.

Here are some examples to give you a sense of how it works:

Single node application

```
from langgraph.channels import EphemeralValue
from langgraph.pregel import Pregel, NodeBuilder

node1 = (
    NodeBuilder().subscribe_only("a")
    .do(lambda x: x + x)
    .write_to("b")
)

app = Pregel(
    nodes={"node1": node1},
    channels={
        "a": EphemeralValue(str),
        "b": EphemeralValue(str),
    },
    input_channels=["a"],
    output_channels=["b"],
)

app.invoke({"a": "foo"})
```

```
{'b': 'foofoo'}
```

Using multiple nodes and multiple output channels

```
from langgraph.channels import LastValue, EphemeralValue
from langgraph.pregel import Pregel, NodeBuilder

node1 = (
    NodeBuilder().subscribe_only("a")
    .do(lambda x: x + x)
    .write_to("b")
)

node2 = (
    NodeBuilder().subscribe_to("b")
    .do(lambda x: x["b"] + x["b"])
    .write_to("c")
)

app = Pregel(
    nodes={"node1": node1, "node2": node2},
    channels={
        "a": EphemeralValue(str),
        "b": LastValue(str),
        "c": EphemeralValue(str),
    },
    input_channels=["a"],
    output_channels=["b", "c"],
)

app.invoke({"a": "foo"})
```

```
{'b': 'foofoo', 'c': 'foofoofoofoo'}
```

Using a Topic channel

```
from langgraph.channels import LastValue, EphemeralValue, Topic
from langgraph.pregel import Pregel, NodeBuilder

node1 = (
    NodeBuilder().subscribe_only("a")
    .do(lambda x: x + x)
    .write_to("b", "c")
)

node2 = (
    NodeBuilder().subscribe_only("b")
    .do(lambda x: x + x)
    .write_to("c")
)

app = Pregel(
    nodes={"node1": node1, "node2": node2},
    channels={
        "a": EphemeralValue(str),
        "b": EphemeralValue(str),
        "c": Topic(str, accumulate=True),
    },
    input_channels=["a"],
    output_channels=["c"],
)

app.invoke({"a": "foo"})
```

```
{"c": ["foofoo", "foofoofoofoo"]}
```

Using a BinaryOperatorAggregate channel

```
from langgraph.channels import EphemeralValue, BinaryOperatorAggregate
from langgraph.pregel import Pregel, NodeBuilder

node1 = (
    NodeBuilder().subscribe_only("a")
    .do(lambda x: x + x)
    .write_to("b", "c")
)

node2 = (
    NodeBuilder().subscribe_only("b")
    .do(lambda x: x + x)
    .write_to("c")
)

def reducer(current, update):
    if current:
        return current + " | " + update
    else:
        return update

app = Pregel(
    nodes={"node1": node1, "node2": node2},
    channels={
        "a": EphemeralValue(str),
        "b": EphemeralValue(str),
        "c": BinaryOperatorAggregate(str, operator=reducer),
    },
    input_channels=["a"],
    output_channels=["c"],
)

app.invoke({"a": "foo"})
```

```
{'c': 'foofoo | foofoofoofoo'}
```

Introducing a cycle

This example demonstrates how to introduce a cycle in the graph, by having
a chain write to a channel it subscribes to. Execution will continue
until a None value is written to the channel.

```
from langgraph.channels import EphemeralValue
from langgraph.pregel import Pregel, NodeBuilder, ChannelWriteEntry

example_node = (
    NodeBuilder()
    .subscribe_only("value")
    .do(lambda x: x + x if len(x) < 10 else None)
    .write_to(ChannelWriteEntry(channel="value", skip_none=True))
)

app = Pregel(
    nodes={"example_node": example_node},
    channels={
        "value": EphemeralValue(str),
    },
    input_channels=["value"],
    output_channels=["value"],
)

app.invoke({"value": "a"})
```

```
{'value': 'aaaaaaaaaaaaaaaa'}
```

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

Back to top