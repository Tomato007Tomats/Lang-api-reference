<!-- URL: page_16 -->

Skip to content

Edit this page

# Testing

## ``langsmith.testing ¶

LangSmith pytest testing module.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `log_feedback` | Log run feedback from within a pytest test run. |
| `log_inputs` | Log run inputs from within a pytest test run. |
| `log_outputs` | Log run outputs from within a pytest test run. |
| `log_reference_outputs` | Log example reference outputs from within a pytest test run. |
| `trace_feedback` | Trace the computation of a pytest run feedback as its own run. |

### ``log\_feedback ¶

```
log_feedback(
    feedback: dict | list[dict] | None = None,
    /,
    *,
    key: str,
    score: int | bool | float | None = None,
    value: str | int | float | bool | None = None,
    **kwargs: Any,
) -> None
```

Log run feedback from within a pytest test run.

.. warning::

```
This API is in beta and might change in future versions.
```

Should only be used in pytest tests decorated with @pytest.mark.langsmith.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `key` | Feedback name.<br>**TYPE:**`str` |
| `score` | Numerical feedback value.<br>**TYPE:**`int | bool | float | None`**DEFAULT:**`None` |
| `value` | Categorical feedback value<br>**TYPE:**`str | int | float | bool | None`**DEFAULT:**`None` |
| `kwargs` | Any other Client.create\_feedback args.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

Example

.. code-block:: python

```
import pytest
from langsmith import testing as t

@pytest.mark.langsmith
def test_foo() -> None:
    x = 0
    y = 1
    expected = 2
    result = foo(x, y)
    t.log_feedback(key="right_type", score=isinstance(result, int))
    assert result == expected
```

### ``log\_inputs ¶

```
log_inputs(inputs: dict) -> None
```

Log run inputs from within a pytest test run.

.. warning::

```
This API is in beta and might change in future versions.
```

Should only be used in pytest tests decorated with @pytest.mark.langsmith.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | Inputs to log.<br>**TYPE:**`dict` |

Example

.. code-block:: python

```
from langsmith import testing as t

@pytest.mark.langsmith
def test_foo() -> None:
    x = 0
    y = 1
    t.log_inputs({"x": x, "y": y})
    assert foo(x, y) == 2
```

### ``log\_outputs ¶

```
log_outputs(outputs: dict) -> None
```

Log run outputs from within a pytest test run.

.. warning::

```
This API is in beta and might change in future versions.
```

Should only be used in pytest tests decorated with @pytest.mark.langsmith.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `outputs` | Outputs to log.<br>**TYPE:**`dict` |

Example

.. code-block:: python

```
from langsmith import testing as t

@pytest.mark.langsmith
def test_foo() -> None:
    x = 0
    y = 1
    result = foo(x, y)
    t.log_outputs({"foo": result})
    assert result == 2
```

### ``log\_reference\_outputs ¶

```
log_reference_outputs(reference_outputs: dict) -> None
```

Log example reference outputs from within a pytest test run.

.. warning::

```
This API is in beta and might change in future versions.
```

Should only be used in pytest tests decorated with @pytest.mark.langsmith.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `outputs` | Reference outputs to log. |

Example

.. code-block:: python

```
from langsmith import testing

@pytest.mark.langsmith
def test_foo() -> None:
    x = 0
    y = 1
    expected = 2
    testing.log_reference_outputs({"foo": expected})
    assert foo(x, y) == expected
```

### ``trace\_feedback ¶

```
trace_feedback(*, name: str = 'Feedback') -> Generator[RunTree | None, None, None]
```

Trace the computation of a pytest run feedback as its own run.

.. warning::

```
This API is in beta and might change in future versions.
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `name` | Feedback run name. Defaults to "Feedback".<br>**TYPE:**`str`**DEFAULT:**`'Feedback'` |

Example

.. code-block:: python

```
import openai
import pytest

from langsmith import testing as t
from langsmith import wrappers

oai_client = wrappers.wrap_openai(openai.Client())

@pytest.mark.langsmith
def test_openai_says_hello():
    # Traced code will be included in the test case
    text = "Say hello!"
    response = oai_client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[\
            {"role": "system", "content": "You are a helpful assistant."},\
            {"role": "user", "content": text},\
        ],
    )
    t.log_inputs({"text": text})
    t.log_outputs({"response": response.choices[0].message.content})
    t.log_reference_outputs({"response": "hello!"})

    # Use this context manager to trace any steps used for generating evaluation
    # feedback separately from the main application logic
    with t.trace_feedback():
        grade = oai_client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[\
                {\
                    "role": "system",\
                    "content": "Return 1 if 'hello' is in the user message and 0 otherwise.",\
                },\
                {\
                    "role": "user",\
                    "content": response.choices[0].message.content,\
                },\
            ],
        )
        # Make sure to log relevant feedback within the context for the
        # trace to be associated with this feedback.
        t.log_feedback(
            key="llm_judge", score=float(grade.choices[0].message.content)
        )

    assert "hello" in response.choices[0].message.content.lower()
```

Back to top