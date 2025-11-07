<!-- URL: page_113 -->

Skip to content

Edit this page

# Expect API

## ``langsmith.\_expect ¶

Make approximate assertions as "expectations" on test results.

This module is designed to be used within test cases decorated with the
`@pytest.mark.decorator` decorator
It allows you to log scores about a test case and optionally make assertions that log as
"expectation" feedback to LangSmith.

Example usage

.. code-block:: python

```
import pytest
from langsmith import expect

@pytest.mark.langsmith
def test_output_semantically_close():
    response = oai_client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[\
            {"role": "system", "content": "You are a helpful assistant."},\
            {"role": "user", "content": "Say hello!"},\
        ],
    )
    response_txt = response.choices[0].message.content
    # Intended usage
    expect.embedding_distance(
        prediction=response_txt,
        reference="Hello!",
    ).to_be_less_than(0.9)

    # Score the test case
    matcher = expect.edit_distance(
        prediction=response_txt,
        reference="Hello!",
    )
    # Apply an assertion and log 'expectation' feedback to LangSmith
    matcher.to_be_less_than(1)

    # You can also directly make assertions on values directly
    expect.value(response_txt).to_contain("Hello!")
    # Or using a custom check
    expect.value(response_txt).against(lambda x: "Hello" in x)

    # You can even use this for basic metric logging within tests

    expect.score(0.8)
    expect.score(0.7, key="similarity").to_be_greater_than(0.7)
```

Back to top