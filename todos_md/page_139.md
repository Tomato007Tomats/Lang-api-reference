<!-- URL: page_139 -->

Skip to content

Edit this page

# Caching

## ``langgraph.cache.base ¶

### ``BaseCache ¶

Bases: `ABC`, `Generic[ValueT]`

Base class for a cache.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize the cache with a serializer. |
| `get` | Get the cached values for the given keys. |
| `aget` | Asynchronously get the cached values for the given keys. |
| `set` | Set the cached values for the given keys and TTLs. |
| `aset` | Asynchronously set the cached values for the given keys and TTLs. |
| `clear` | Delete the cached values for the given namespaces. |
| `aclear` | Asynchronously delete the cached values for the given namespaces. |

#### ``\_\_init\_\_ ¶

```
__init__(*, serde: SerializerProtocol | None = None) -> None
```

Initialize the cache with a serializer.

#### ``get`abstractmethod`¶

```
get(keys: Sequence[FullKey]) -> dict[FullKey, ValueT]
```

Get the cached values for the given keys.

#### ``aget`abstractmethod``async`¶

```
aget(keys: Sequence[FullKey]) -> dict[FullKey, ValueT]
```

Asynchronously get the cached values for the given keys.

#### ``set`abstractmethod`¶

```
set(pairs: Mapping[FullKey, tuple[ValueT, int | None]]) -> None
```

Set the cached values for the given keys and TTLs.

#### ``aset`abstractmethod``async`¶

```
aset(pairs: Mapping[FullKey, tuple[ValueT, int | None]]) -> None
```

Asynchronously set the cached values for the given keys and TTLs.

#### ``clear`abstractmethod`¶

```
clear(namespaces: Sequence[Namespace] | None = None) -> None
```

Delete the cached values for the given namespaces.
If no namespaces are provided, clear all cached values.

#### ``aclear`abstractmethod``async`¶

```
aclear(namespaces: Sequence[Namespace] | None = None) -> None
```

Asynchronously delete the cached values for the given namespaces.
If no namespaces are provided, clear all cached values.

## ``langgraph.cache.memory ¶

### ``InMemoryCache ¶

Bases: `BaseCache[ValueT]`

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize the cache with a serializer. |
| `get` | Get the cached values for the given keys. |
| `aget` | Asynchronously get the cached values for the given keys. |
| `set` | Set the cached values for the given keys. |
| `aset` | Asynchronously set the cached values for the given keys. |
| `clear` | Delete the cached values for the given namespaces. |
| `aclear` | Asynchronously delete the cached values for the given namespaces. |

#### ``\_\_init\_\_ ¶

```
__init__(*, serde: SerializerProtocol | None = None)
```

Initialize the cache with a serializer.

#### ``get ¶

```
get(keys: Sequence[FullKey]) -> dict[FullKey, ValueT]
```

Get the cached values for the given keys.

#### ``aget`async`¶

```
aget(keys: Sequence[FullKey]) -> dict[FullKey, ValueT]
```

Asynchronously get the cached values for the given keys.

#### ``set ¶

```
set(keys: Mapping[FullKey, tuple[ValueT, int | None]]) -> None
```

Set the cached values for the given keys.

#### ``aset`async`¶

```
aset(keys: Mapping[FullKey, tuple[ValueT, int | None]]) -> None
```

Asynchronously set the cached values for the given keys.

#### ``clear ¶

```
clear(namespaces: Sequence[Namespace] | None = None) -> None
```

Delete the cached values for the given namespaces.
If no namespaces are provided, clear all cached values.

#### ``aclear`async`¶

```
aclear(namespaces: Sequence[Namespace] | None = None) -> None
```

Asynchronously delete the cached values for the given namespaces.
If no namespaces are provided, clear all cached values.

## ``langgraph.cache.sqlite ¶

### ``SqliteCache ¶

Bases: `BaseCache[ValueT]`

File-based cache using SQLite.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize the cache with a file path. |
| `get` | Get the cached values for the given keys. |
| `aget` | Asynchronously get the cached values for the given keys. |
| `set` | Set the cached values for the given keys and TTLs. |
| `aset` | Asynchronously set the cached values for the given keys and TTLs. |
| `clear` | Delete the cached values for the given namespaces. |
| `aclear` | Asynchronously delete the cached values for the given namespaces. |

#### ``\_\_init\_\_ ¶

```
__init__(*, path: str, serde: SerializerProtocol | None = None) -> None
```

Initialize the cache with a file path.

#### ``get ¶

```
get(keys: Sequence[FullKey]) -> dict[FullKey, ValueT]
```

Get the cached values for the given keys.

#### ``aget`async`¶

```
aget(keys: Sequence[FullKey]) -> dict[FullKey, ValueT]
```

Asynchronously get the cached values for the given keys.

#### ``set ¶

```
set(mapping: Mapping[FullKey, tuple[ValueT, int | None]]) -> None
```

Set the cached values for the given keys and TTLs.

#### ``aset`async`¶

```
aset(mapping: Mapping[FullKey, tuple[ValueT, int | None]]) -> None
```

Asynchronously set the cached values for the given keys and TTLs.

#### ``clear ¶

```
clear(namespaces: Sequence[Namespace] | None = None) -> None
```

Delete the cached values for the given namespaces.
If no namespaces are provided, clear all cached values.

#### ``aclear`async`¶

```
aclear(namespaces: Sequence[Namespace] | None = None) -> None
```

Asynchronously delete the cached values for the given namespaces.
If no namespaces are provided, clear all cached values.

Back to top