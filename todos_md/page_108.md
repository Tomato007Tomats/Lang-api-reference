<!-- URL: page_108 -->

Skip to content

Edit this page

# Runtime

## ``langgraph.runtime.Runtime`dataclass`¶

Bases: `Generic[ContextT]`

Convenience class that bundles run-scoped context and other runtime utilities.

Added in version v0.6.0

Example:

```
from typing import TypedDict
from langgraph.graph import StateGraph
from dataclasses import dataclass
from langgraph.runtime import Runtime
from langgraph.store.memory import InMemoryStore

@dataclass
class Context:
    user_id: str

class State(TypedDict, total=False):
    response: str

store = InMemoryStore()
store.put(("users",), "user_123", {"name": "Alice"})

def personalized_greeting(state: State, runtime: Runtime[Context]) -> State:
    '''Generate personalized greeting using runtime context and store.'''
    user_id = runtime.context.user_id
    name = "unknown_user"
    if runtime.store:
        if memory := runtime.store.get(("users",), user_id):
            name = memory.value["name"]

    response = f"Hello {name}! Nice to see you again."
    return {"response": response}

graph = (
    StateGraph(state_schema=State, context_schema=Context)
    .add_node("personalized_greeting", personalized_greeting)
    .set_entry_point("personalized_greeting")
    .set_finish_point("personalized_greeting")
    .compile(store=store)
)

result = graph.invoke({}, context=Context(user_id="user_123"))
print(result)
# > {'response': 'Hello Alice! Nice to see you again.'}
```

### ``context`class-attribute``instance-attribute`¶

```
context: ContextT = field(default=None)
```

Static context for the graph run, like `user_id`, `db_conn`, etc.

Can also be thought of as 'run dependencies'.

### ``store`class-attribute``instance-attribute`¶

```
store: BaseStore | None = field(default=None)
```

Store for the graph run, enabling persistence and memory.

### ``stream\_writer`class-attribute``instance-attribute`¶

```
stream_writer: StreamWriter = field(default=_no_op_stream_writer)
```

Function that writes to the custom stream.

### ``previous`class-attribute``instance-attribute`¶

```
previous: Any = field(default=None)
```

The previous return value for the given thread.

Only available with the functional API when a checkpointer is provided.

## ``langgraph.runtime ¶

| FUNCTION | DESCRIPTION |
| --- | --- |
| `get_runtime` | Get the runtime for the current graph run. |

### ``get\_runtime ¶

```
get_runtime(context_schema: type[ContextT] | None = None) -> Runtime[ContextT]
```

Get the runtime for the current graph run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `context_schema` | Optional schema used for type hinting the return type of the runtime.<br>**TYPE:**`type[ContextT] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runtime[ContextT]` | The runtime for the current graph run. |

Back to top