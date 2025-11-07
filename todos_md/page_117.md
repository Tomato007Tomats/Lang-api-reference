<!-- URL: page_117 -->

Skip to content

Edit this page

# Embeddings integration tests ¶

## ``langchain\_tests.integration\_tests.EmbeddingsIntegrationTests ¶

Bases: `EmbeddingsTests`

Base class for embeddings integration tests.

Test subclasses must implement the `embeddings_class` property to specify the
embeddings model to be tested. You can also override the
`embedding_model_params` property to specify initialization parameters.

```
from typing import Type

from langchain_tests.integration_tests import EmbeddingsIntegrationTests
from my_package.embeddings import MyEmbeddingsModel

class TestMyEmbeddingsModelIntegration(EmbeddingsIntegrationTests):
    @property
    def embeddings_class(self) -> Type[MyEmbeddingsModel]:
        # Return the embeddings model class to test here
        return MyEmbeddingsModel

    @property
    def embedding_model_params(self) -> dict:
        # Return initialization parameters for the model.
        return {"model": "model-001"}
```

Note

API references for individual test methods include troubleshooting tips.

| METHOD | DESCRIPTION |
| --- | --- |
| `test_no_overrides_DO_NOT_OVERRIDE` | Test that no standard tests are overridden. |
| `model` | Embeddings model fixture. |
| `test_embed_query` | Test embedding a string query. |
| `test_embed_documents` | Test embedding a list of strings. |
| `test_aembed_query` | Test embedding a string query async. |
| `test_aembed_documents` | Test embedding a list of strings async. |

### ``embeddings\_class`abstractmethod``property`¶

```
embeddings_class: type[Embeddings]
```

Embeddings class.

### ``embedding\_model\_params`property`¶

```
embedding_model_params: dict
```

Embeddings model parameters.

### ``test\_no\_overrides\_DO\_NOT\_OVERRIDE ¶

```
test_no_overrides_DO_NOT_OVERRIDE() -> None
```

Test that no standard tests are overridden.

### ``model ¶

```
model() -> Embeddings
```

Embeddings model fixture.

### ``test\_embed\_query ¶

```
test_embed_query(model: Embeddings) -> None
```

Test embedding a string query.

Troubleshooting

If this test fails, check that:

1. The model will generate a list of floats when calling `.embed_query`
    on a string.
2. The length of the list is consistent across different inputs.

### ``test\_embed\_documents ¶

```
test_embed_documents(model: Embeddings) -> None
```

Test embedding a list of strings.

Troubleshooting

If this test fails, check that:

1. The model will generate a list of lists of floats when calling
    `embed_documents` on a list of strings.
2. The length of each list is the same.

### ``test\_aembed\_query`async`¶

```
test_aembed_query(model: Embeddings) -> None
```

Test embedding a string query async.

Troubleshooting

If this test fails, check that:

1. The model will generate a list of floats when calling `aembed_query`
    on a string.
2. The length of the list is consistent across different inputs.

### ``test\_aembed\_documents`async`¶

```
test_aembed_documents(model: Embeddings) -> None
```

Test embedding a list of strings async.

Troubleshooting

If this test fails, check that:

1. The model will generate a list of lists of floats when calling
    `aembed_documents` on a list of strings.
2. The length of each list is the same.

Back to top