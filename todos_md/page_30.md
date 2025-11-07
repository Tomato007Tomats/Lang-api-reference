<!-- URL: page_30 -->

Skip to content

Edit this page

# Exceptions

## ``langchain\_core.exceptions.OutputParserException ¶

Bases: `ValueError`, `LangChainException`

Exception that output parsers should raise to signify a parsing error.

This exists to differentiate parsing errors from other code or execution errors
that also may arise inside the output parser.

`OutputParserException` will be available to catch and handle in ways to fix the
parsing error, while other errors will be raised.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Create an `OutputParserException`. |

### ``\_\_init\_\_ ¶

```
__init__(
    error: Any,
    observation: str | None = None,
    llm_output: str | None = None,
    send_to_llm: bool = False,
)
```

Create an `OutputParserException`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `error` | The error that's being re-raised or an error message.<br>**TYPE:**`Any` |
| `observation` | String explanation of error which can be passed to a model to<br>try and remediate the issue.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `llm_output` | String model output which is error-ing.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `send_to_llm` | Whether to send the observation and llm\_output back to an Agent<br>after an `OutputParserException` has been raised.<br>This gives the underlying model driving the agent the context that the<br>previous output was improperly structured, in the hopes that it will<br>update the output to the correct format.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If `send_to_llm` is `True` but either observation or<br>`llm_output` are not provided. |

Back to top