<!-- URL: page_145 -->

Skip to content

Edit this page

# Globals ¶

`langchain-classic` documentation

These docs cover the `langchain-classic` package. This package will be maintained for security vulnerabilities until December 2026. Users are encouraged to migrate to the `langchain` package for the latest features and improvements. See docs for `langchain`

## ``langchain\_classic.globals ¶

Global values and configuration that apply to all of LangChain.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `get_debug` | Get the value of the `debug` global setting. |
| `get_llm_cache` | Get the value of the `llm_cache` global setting. |
| `get_verbose` | Get the value of the `verbose` global setting. |
| `set_debug` | Set a new value for the `debug` global setting. |
| `set_llm_cache` | Set a new LLM cache, overwriting the previous value, if any. |
| `set_verbose` | Set a new value for the `verbose` global setting. |

### ``get\_debug ¶

```
get_debug() -> bool
```

Get the value of the `debug` global setting.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | The value of the `debug` global setting. |

### ``get\_llm\_cache ¶

```
get_llm_cache() -> BaseCache | None
```

Get the value of the `llm_cache` global setting.

| RETURNS | DESCRIPTION |
| --- | --- |
| `BaseCache | None` | The value of the `llm_cache` global setting. |

### ``get\_verbose ¶

```
get_verbose() -> bool
```

Get the value of the `verbose` global setting.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | The value of the `verbose` global setting. |

### ``set\_debug ¶

```
set_debug(value: bool) -> None
```

Set a new value for the `debug` global setting.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `value` | The new value for the `debug` global setting.<br>**TYPE:**`bool` |

### ``set\_llm\_cache ¶

```
set_llm_cache(value: BaseCache | None) -> None
```

Set a new LLM cache, overwriting the previous value, if any.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `value` | The new LLM cache to use. If `None`, the LLM cache is disabled.<br>**TYPE:**`BaseCache | None` |

### ``set\_verbose ¶

```
set_verbose(value: bool) -> None
```

Set a new value for the `verbose` global setting.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `value` | The new value for the `verbose` global setting.<br>**TYPE:**`bool` |

Back to top