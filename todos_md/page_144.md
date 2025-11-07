<!-- URL: page_144 -->

Skip to content

Edit this page

# Channels

## ``langgraph.channels.base ¶

### ``BaseChannel ¶

Bases: `Generic[Value, Update, Checkpoint]`, `ABC`

Base class for all channels.

| METHOD | DESCRIPTION |
| --- | --- |
| `copy` | Return a copy of the channel. |
| `checkpoint` | Return a serializable representation of the channel's current state. |
| `from_checkpoint` | Return a new identical channel, optionally initialized from a checkpoint. |
| `get` | Return the current value of the channel. |
| `is_available` | Return `True` if the channel is available (not empty), `False` otherwise. |
| `update` | Update the channel's value with the given sequence of updates. |
| `consume` | Notify the channel that a subscribed task ran. By default, no-op. |
| `finish` | Notify the channel that the Pregel run is finishing. By default, no-op. |

#### ``ValueType`abstractmethod``property`¶

```
ValueType: Any
```

The type of the value stored in the channel.

#### ``UpdateType`abstractmethod``property`¶

```
UpdateType: Any
```

The type of the update received by the channel.

#### ``copy ¶

```
copy() -> Self
```

Return a copy of the channel.
By default, delegates to `checkpoint()` and `from_checkpoint()`.
Subclasses can override this method with a more efficient implementation.

#### ``checkpoint ¶

```
checkpoint() -> Checkpoint | Any
```

Return a serializable representation of the channel's current state.
Raises `EmptyChannelError` if the channel is empty (never updated yet),
or doesn't support checkpoints.

#### ``from\_checkpoint`abstractmethod`¶

```
from_checkpoint(checkpoint: Checkpoint | Any) -> Self
```

Return a new identical channel, optionally initialized from a checkpoint.
If the checkpoint contains complex data structures, they should be copied.

#### ``get`abstractmethod`¶

```
get() -> Value
```

Return the current value of the channel.

Raises `EmptyChannelError` if the channel is empty (never updated yet).

#### ``is\_available ¶

```
is_available() -> bool
```

Return `True` if the channel is available (not empty), `False` otherwise.
Subclasses should override this method to provide a more efficient
implementation than calling `get()` and catching `EmptyChannelError`.

#### ``update`abstractmethod`¶

```
update(values: Sequence[Update]) -> bool
```

Update the channel's value with the given sequence of updates.
The order of the updates in the sequence is arbitrary.
This method is called by Pregel for all channels at the end of each step.
If there are no updates, it is called with an empty sequence.
Raises `InvalidUpdateError` if the sequence of updates is invalid.
Returns `True` if the channel was updated, `False` otherwise.

#### ``consume ¶

```
consume() -> bool
```

Notify the channel that a subscribed task ran. By default, no-op.
A channel can use this method to modify its state, preventing the value
from being consumed again.

Returns `True` if the channel was updated, `False` otherwise.

#### ``finish ¶

```
finish() -> bool
```

Notify the channel that the Pregel run is finishing. By default, no-op.
A channel can use this method to modify its state, preventing finish.

Returns `True` if the channel was updated, `False` otherwise.

## ``langgraph.channels ¶

### ``Topic ¶

Bases: `Generic[Value]`, `BaseChannel[Sequence[Value], Value | list[Value], list[Value]]`

A configurable PubSub Topic.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `typ` | The type of the value stored in the channel.<br>**TYPE:**`type[Value]` |
| `accumulate` | Whether to accumulate values across steps. If `False`, the channel will be emptied after each step.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| METHOD | DESCRIPTION |
| --- | --- |
| `consume` | Notify the channel that a subscribed task ran. By default, no-op. |
| `finish` | Notify the channel that the Pregel run is finishing. By default, no-op. |
| `copy` | Return a copy of the channel. |
| `checkpoint` | Return a serializable representation of the channel's current state. |
| `from_checkpoint` | Return a new identical channel, optionally initialized from a checkpoint. |
| `update` | Update the channel's value with the given sequence of updates. |
| `get` | Return the current value of the channel. |
| `is_available` | Return `True` if the channel is available (not empty), `False` otherwise. |

#### ``ValueType`property`¶

```
ValueType: Any
```

The type of the value stored in the channel.

#### ``UpdateType`property`¶

```
UpdateType: Any
```

The type of the update received by the channel.

#### ``consume ¶

```
consume() -> bool
```

Notify the channel that a subscribed task ran. By default, no-op.
A channel can use this method to modify its state, preventing the value
from being consumed again.

Returns `True` if the channel was updated, `False` otherwise.

#### ``finish ¶

```
finish() -> bool
```

Notify the channel that the Pregel run is finishing. By default, no-op.
A channel can use this method to modify its state, preventing finish.

Returns `True` if the channel was updated, `False` otherwise.

#### ``copy ¶

```
copy() -> Self
```

Return a copy of the channel.

#### ``checkpoint ¶

```
checkpoint() -> list[Value]
```

Return a serializable representation of the channel's current state.
Raises `EmptyChannelError` if the channel is empty (never updated yet),
or doesn't support checkpoints.

#### ``from\_checkpoint ¶

```
from_checkpoint(checkpoint: list[Value]) -> Self
```

Return a new identical channel, optionally initialized from a checkpoint.
If the checkpoint contains complex data structures, they should be copied.

#### ``update ¶

```
update(values: Sequence[Value | list[Value]]) -> bool
```

Update the channel's value with the given sequence of updates.
The order of the updates in the sequence is arbitrary.
This method is called by Pregel for all channels at the end of each step.
If there are no updates, it is called with an empty sequence.
Raises `InvalidUpdateError` if the sequence of updates is invalid.
Returns `True` if the channel was updated, `False` otherwise.

#### ``get ¶

```
get() -> Sequence[Value]
```

Return the current value of the channel.

Raises `EmptyChannelError` if the channel is empty (never updated yet).

#### ``is\_available ¶

```
is_available() -> bool
```

Return `True` if the channel is available (not empty), `False` otherwise.
Subclasses should override this method to provide a more efficient
implementation than calling `get()` and catching `EmptyChannelError`.

### ``LastValue ¶

Bases: `Generic[Value]`, `BaseChannel[Value, Value, Value]`

Stores the last value received, can receive at most one value per step.

| METHOD | DESCRIPTION |
| --- | --- |
| `consume` | Notify the channel that a subscribed task ran. By default, no-op. |
| `finish` | Notify the channel that the Pregel run is finishing. By default, no-op. |
| `copy` | Return a copy of the channel. |
| `from_checkpoint` | Return a new identical channel, optionally initialized from a checkpoint. |
| `update` | Update the channel's value with the given sequence of updates. |
| `get` | Return the current value of the channel. |
| `is_available` | Return `True` if the channel is available (not empty), `False` otherwise. |
| `checkpoint` | Return a serializable representation of the channel's current state. |

#### ``ValueType`property`¶

```
ValueType: type[Value]
```

The type of the value stored in the channel.

#### ``UpdateType`property`¶

```
UpdateType: type[Value]
```

The type of the update received by the channel.

#### ``consume ¶

```
consume() -> bool
```

Notify the channel that a subscribed task ran. By default, no-op.
A channel can use this method to modify its state, preventing the value
from being consumed again.

Returns `True` if the channel was updated, `False` otherwise.

#### ``finish ¶

```
finish() -> bool
```

Notify the channel that the Pregel run is finishing. By default, no-op.
A channel can use this method to modify its state, preventing finish.

Returns `True` if the channel was updated, `False` otherwise.

#### ``copy ¶

```
copy() -> Self
```

Return a copy of the channel.

#### ``from\_checkpoint ¶

```
from_checkpoint(checkpoint: Value) -> Self
```

Return a new identical channel, optionally initialized from a checkpoint.
If the checkpoint contains complex data structures, they should be copied.

#### ``update ¶

```
update(values: Sequence[Value]) -> bool
```

Update the channel's value with the given sequence of updates.
The order of the updates in the sequence is arbitrary.
This method is called by Pregel for all channels at the end of each step.
If there are no updates, it is called with an empty sequence.
Raises `InvalidUpdateError` if the sequence of updates is invalid.
Returns `True` if the channel was updated, `False` otherwise.

#### ``get ¶

```
get() -> Value
```

Return the current value of the channel.

Raises `EmptyChannelError` if the channel is empty (never updated yet).

#### ``is\_available ¶

```
is_available() -> bool
```

Return `True` if the channel is available (not empty), `False` otherwise.
Subclasses should override this method to provide a more efficient
implementation than calling `get()` and catching `EmptyChannelError`.

#### ``checkpoint ¶

```
checkpoint() -> Value
```

Return a serializable representation of the channel's current state.
Raises `EmptyChannelError` if the channel is empty (never updated yet),
or doesn't support checkpoints.

### ``EphemeralValue ¶

Bases: `Generic[Value]`, `BaseChannel[Value, Value, Value]`

Stores the value received in the step immediately preceding, clears after.

| METHOD | DESCRIPTION |
| --- | --- |
| `consume` | Notify the channel that a subscribed task ran. By default, no-op. |
| `finish` | Notify the channel that the Pregel run is finishing. By default, no-op. |
| `copy` | Return a copy of the channel. |
| `from_checkpoint` | Return a new identical channel, optionally initialized from a checkpoint. |
| `update` | Update the channel's value with the given sequence of updates. |
| `get` | Return the current value of the channel. |
| `is_available` | Return `True` if the channel is available (not empty), `False` otherwise. |
| `checkpoint` | Return a serializable representation of the channel's current state. |

#### ``ValueType`property`¶

```
ValueType: type[Value]
```

The type of the value stored in the channel.

#### ``UpdateType`property`¶

```
UpdateType: type[Value]
```

The type of the update received by the channel.

#### ``consume ¶

```
consume() -> bool
```

Notify the channel that a subscribed task ran. By default, no-op.
A channel can use this method to modify its state, preventing the value
from being consumed again.

Returns `True` if the channel was updated, `False` otherwise.

#### ``finish ¶

```
finish() -> bool
```

Notify the channel that the Pregel run is finishing. By default, no-op.
A channel can use this method to modify its state, preventing finish.

Returns `True` if the channel was updated, `False` otherwise.

#### ``copy ¶

```
copy() -> Self
```

Return a copy of the channel.

#### ``from\_checkpoint ¶

```
from_checkpoint(checkpoint: Value) -> Self
```

Return a new identical channel, optionally initialized from a checkpoint.
If the checkpoint contains complex data structures, they should be copied.

#### ``update ¶

```
update(values: Sequence[Value]) -> bool
```

Update the channel's value with the given sequence of updates.
The order of the updates in the sequence is arbitrary.
This method is called by Pregel for all channels at the end of each step.
If there are no updates, it is called with an empty sequence.
Raises `InvalidUpdateError` if the sequence of updates is invalid.
Returns `True` if the channel was updated, `False` otherwise.

#### ``get ¶

```
get() -> Value
```

Return the current value of the channel.

Raises `EmptyChannelError` if the channel is empty (never updated yet).

#### ``is\_available ¶

```
is_available() -> bool
```

Return `True` if the channel is available (not empty), `False` otherwise.
Subclasses should override this method to provide a more efficient
implementation than calling `get()` and catching `EmptyChannelError`.

#### ``checkpoint ¶

```
checkpoint() -> Value
```

Return a serializable representation of the channel's current state.
Raises `EmptyChannelError` if the channel is empty (never updated yet),
or doesn't support checkpoints.

### ``BinaryOperatorAggregate ¶

Bases: `Generic[Value]`, `BaseChannel[Value, Value, Value]`

Stores the result of applying a binary operator to the current value and each new value.

```
import operator

total = Channels.BinaryOperatorAggregate(int, operator.add)
```

| METHOD | DESCRIPTION |
| --- | --- |
| `consume` | Notify the channel that a subscribed task ran. By default, no-op. |
| `finish` | Notify the channel that the Pregel run is finishing. By default, no-op. |
| `copy` | Return a copy of the channel. |
| `from_checkpoint` | Return a new identical channel, optionally initialized from a checkpoint. |
| `update` | Update the channel's value with the given sequence of updates. |
| `get` | Return the current value of the channel. |
| `is_available` | Return `True` if the channel is available (not empty), `False` otherwise. |
| `checkpoint` | Return a serializable representation of the channel's current state. |

#### ``ValueType`property`¶

```
ValueType: type[Value]
```

The type of the value stored in the channel.

#### ``UpdateType`property`¶

```
UpdateType: type[Value]
```

The type of the update received by the channel.

#### ``consume ¶

```
consume() -> bool
```

Notify the channel that a subscribed task ran. By default, no-op.
A channel can use this method to modify its state, preventing the value
from being consumed again.

Returns `True` if the channel was updated, `False` otherwise.

#### ``finish ¶

```
finish() -> bool
```

Notify the channel that the Pregel run is finishing. By default, no-op.
A channel can use this method to modify its state, preventing finish.

Returns `True` if the channel was updated, `False` otherwise.

#### ``copy ¶

```
copy() -> Self
```

Return a copy of the channel.

#### ``from\_checkpoint ¶

```
from_checkpoint(checkpoint: Value) -> Self
```

Return a new identical channel, optionally initialized from a checkpoint.
If the checkpoint contains complex data structures, they should be copied.

#### ``update ¶

```
update(values: Sequence[Value]) -> bool
```

Update the channel's value with the given sequence of updates.
The order of the updates in the sequence is arbitrary.
This method is called by Pregel for all channels at the end of each step.
If there are no updates, it is called with an empty sequence.
Raises `InvalidUpdateError` if the sequence of updates is invalid.
Returns `True` if the channel was updated, `False` otherwise.

#### ``get ¶

```
get() -> Value
```

Return the current value of the channel.

Raises `EmptyChannelError` if the channel is empty (never updated yet).

#### ``is\_available ¶

```
is_available() -> bool
```

Return `True` if the channel is available (not empty), `False` otherwise.
Subclasses should override this method to provide a more efficient
implementation than calling `get()` and catching `EmptyChannelError`.

#### ``checkpoint ¶

```
checkpoint() -> Value
```

Return a serializable representation of the channel's current state.
Raises `EmptyChannelError` if the channel is empty (never updated yet),
or doesn't support checkpoints.

### ``AnyValue ¶

Bases: `Generic[Value]`, `BaseChannel[Value, Value, Value]`

Stores the last value received, assumes that if multiple values are
received, they are all equal.

| METHOD | DESCRIPTION |
| --- | --- |
| `consume` | Notify the channel that a subscribed task ran. By default, no-op. |
| `finish` | Notify the channel that the Pregel run is finishing. By default, no-op. |
| `copy` | Return a copy of the channel. |
| `from_checkpoint` | Return a new identical channel, optionally initialized from a checkpoint. |
| `update` | Update the channel's value with the given sequence of updates. |
| `get` | Return the current value of the channel. |
| `is_available` | Return `True` if the channel is available (not empty), `False` otherwise. |
| `checkpoint` | Return a serializable representation of the channel's current state. |

#### ``ValueType`property`¶

```
ValueType: type[Value]
```

The type of the value stored in the channel.

#### ``UpdateType`property`¶

```
UpdateType: type[Value]
```

The type of the update received by the channel.

#### ``consume ¶

```
consume() -> bool
```

Notify the channel that a subscribed task ran. By default, no-op.
A channel can use this method to modify its state, preventing the value
from being consumed again.

Returns `True` if the channel was updated, `False` otherwise.

#### ``finish ¶

```
finish() -> bool
```

Notify the channel that the Pregel run is finishing. By default, no-op.
A channel can use this method to modify its state, preventing finish.

Returns `True` if the channel was updated, `False` otherwise.

#### ``copy ¶

```
copy() -> Self
```

Return a copy of the channel.

#### ``from\_checkpoint ¶

```
from_checkpoint(checkpoint: Value) -> Self
```

Return a new identical channel, optionally initialized from a checkpoint.
If the checkpoint contains complex data structures, they should be copied.

#### ``update ¶

```
update(values: Sequence[Value]) -> bool
```

Update the channel's value with the given sequence of updates.
The order of the updates in the sequence is arbitrary.
This method is called by Pregel for all channels at the end of each step.
If there are no updates, it is called with an empty sequence.
Raises `InvalidUpdateError` if the sequence of updates is invalid.
Returns `True` if the channel was updated, `False` otherwise.

#### ``get ¶

```
get() -> Value
```

Return the current value of the channel.

Raises `EmptyChannelError` if the channel is empty (never updated yet).

#### ``is\_available ¶

```
is_available() -> bool
```

Return `True` if the channel is available (not empty), `False` otherwise.
Subclasses should override this method to provide a more efficient
implementation than calling `get()` and catching `EmptyChannelError`.

#### ``checkpoint ¶

```
checkpoint() -> Value
```

Return a serializable representation of the channel's current state.
Raises `EmptyChannelError` if the channel is empty (never updated yet),
or doesn't support checkpoints.

Back to top