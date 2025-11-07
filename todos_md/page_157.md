<!-- URL: page_157 -->

Skip to content

Edit this page

# LangSmith utilities ¶

`langchain-classic` documentation

These docs cover the `langchain-classic` package. This package will be maintained for security vulnerabilities until December 2026. Users are encouraged to migrate to the `langchain` package for the latest features and improvements. See docs for `langchain`

## ``langchain\_classic.smith ¶

**LangSmith** utilities.

This module provides utilities for connecting to
LangSmith.

**Evaluation**

LangSmith helps you evaluate Chains and other language model application components
using a number of LangChain evaluators.
An example of this is shown below, assuming you've created a LangSmith dataset
called `<my_dataset_name>`:

```
from langsmith import Client
from langchain_openai import ChatOpenAI
from langchain_classic.chains import LLMChain
from langchain_classic.smith import RunEvalConfig, run_on_dataset

# Chains may have memory. Passing in a constructor function lets the
# evaluation framework avoid cross-contamination between runs.
def construct_chain():
    model = ChatOpenAI(temperature=0)
    chain = LLMChain.from_string(model, "What's the answer to {your_input_key}")
    return chain

# Load off-the-shelf evaluators via config or the EvaluatorType (string or enum)
evaluation_config = RunEvalConfig(
    evaluators=[\
        "qa",  # "Correctness" against a reference answer\
        "embedding_distance",\
        RunEvalConfig.Criteria("helpfulness"),\
        RunEvalConfig.Criteria(\
            {\
                "fifth-grader-score": "Do you have to be smarter than a fifth "\
                "grader to answer this question?"\
            }\
        ),\
    ]
)

client = Client()
run_on_dataset(
    client,
    "<my_dataset_name>",
    construct_chain,
    evaluation=evaluation_config,
)
```

You can also create custom evaluators by subclassing the
`StringEvaluator <langchain.evaluation.schema.StringEvaluator>`
or LangSmith's `RunEvaluator` classes.

```
from typing import Optional
from langchain_classic.evaluation import StringEvaluator

class MyStringEvaluator(StringEvaluator):
    @property
    def requires_input(self) -> bool:
        return False

    @property
    def requires_reference(self) -> bool:
        return True

    @property
    def evaluation_name(self) -> str:
        return "exact_match"

    def _evaluate_strings(
        self, prediction, reference=None, input=None, **kwargs
    ) -> dict:
        return {"score": prediction == reference}

evaluation_config = RunEvalConfig(
    custom_evaluators=[MyStringEvaluator()],
)

run_on_dataset(
    client,
    "<my_dataset_name>",
    construct_chain,
    evaluation=evaluation_config,
)
```

**Primary Functions**

- `arun_on_dataset <langchain.smith.evaluation.runner_utils.arun_on_dataset>`:
Asynchronous function to evaluate a chain, agent, or other LangChain component over
a dataset.
- `run_on_dataset <langchain.smith.evaluation.runner_utils.run_on_dataset>`:
Function to evaluate a chain, agent, or other LangChain component over a dataset.
- `RunEvalConfig <langchain.smith.evaluation.config.RunEvalConfig>`:
Class representing the configuration for running evaluation.
You can select evaluators by
`EvaluatorType <langchain.evaluation.schema.EvaluatorType>` or config,
or you can pass in `custom_evaluators`.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `arun_on_dataset` | Run on dataset. |
| `run_on_dataset` | Run on dataset. |

### ``RunEvalConfig ¶

Bases: `BaseModel`

Configuration for a run evaluation.

#### ``evaluators`class-attribute``instance-attribute`¶

```
evaluators: list[SINGLE_EVAL_CONFIG_TYPE | CUSTOM_EVALUATOR_TYPE] = Field(
    default_factory=list
)
```

Configurations for which evaluators to apply to the dataset run.
Each can be the string of an
`EvaluatorType <langchain.evaluation.schema.EvaluatorType>`, such
as `EvaluatorType.QA`, the evaluator type string ("qa"), or a configuration for a
given evaluator
(e.g.,
`RunEvalConfig.QA <langchain.smith.evaluation.config.RunEvalConfig.QA>`).

#### ``custom\_evaluators`class-attribute``instance-attribute`¶

```
custom_evaluators: list[CUSTOM_EVALUATOR_TYPE] | None = None
```

Custom evaluators to apply to the dataset run.

#### ``batch\_evaluators`class-attribute``instance-attribute`¶

```
batch_evaluators: list[BATCH_EVALUATOR_LIKE] | None = None
```

Evaluators that run on an aggregate/batch level.

These generate one or more metrics that are assigned to the full test run.
As a result, they are not associated with individual traces.

#### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

#### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

#### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

#### ``eval\_llm`class-attribute``instance-attribute`¶

```
eval_llm: BaseLanguageModel | None = None
```

The language model to pass to any evaluators that require one.

#### ``Criteria ¶

Bases: `SingleKeyEvalConfig`

Configuration for a reference-free criteria evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `criteria` | The criteria to evaluate.<br>**TYPE:**`CRITERIA_TYPE | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``LabeledCriteria ¶

Bases: `SingleKeyEvalConfig`

Configuration for a labeled (with references) criteria evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `criteria` | The criteria to evaluate.<br>**TYPE:**`CRITERIA_TYPE | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``EmbeddingDistance ¶

Bases: `SingleKeyEvalConfig`

Configuration for an embedding distance evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `embeddings` | The embeddings to use for computing the distance.<br>**TYPE:**`Embeddings | None` |
| `distance_metric` | The distance metric to use for computing the distance.<br>**TYPE:**`EmbeddingDistance | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``StringDistance ¶

Bases: `SingleKeyEvalConfig`

Configuration for a string distance evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `distance` | The string distance metric to use (`damerau_levenshtein`,<br>`levenshtein`, `jaro`, or `jaro_winkler`).<br>**TYPE:**`StringDistance | None` |
| `normalize_score` | Whether to normalize the distance to between 0 and 1.<br>Applies only to the Levenshtein and Damerau-Levenshtein distances.<br>**TYPE:**`bool` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``QA ¶

Bases: `SingleKeyEvalConfig`

Configuration for a QA evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `prompt` | The prompt template to use for generating the question.<br>**TYPE:**`BasePromptTemplate | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``ContextQA ¶

Bases: `SingleKeyEvalConfig`

Configuration for a context-based QA evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `prompt` | The prompt template to use for generating the question.<br>**TYPE:**`BasePromptTemplate | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``CoTQA ¶

Bases: `SingleKeyEvalConfig`

Configuration for a context-based QA evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `prompt` | The prompt template to use for generating the question.<br>**TYPE:**`BasePromptTemplate | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``JsonValidity ¶

Bases: `SingleKeyEvalConfig`

Configuration for a json validity evaluator.

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``JsonEqualityEvaluator ¶

Bases: `EvalConfig`

Configuration for a json equality evaluator.

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``ExactMatch ¶

Bases: `SingleKeyEvalConfig`

Configuration for an exact match string evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `ignore_case` | Whether to ignore case when comparing strings.<br>**TYPE:**`bool` |
| `ignore_punctuation` | Whether to ignore punctuation when comparing strings.<br>**TYPE:**`bool` |
| `ignore_numbers` | Whether to ignore numbers when comparing strings.<br>**TYPE:**`bool` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``RegexMatch ¶

Bases: `SingleKeyEvalConfig`

Configuration for a regex match string evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `flags` | The flags to pass to the regex. Example: `re.IGNORECASE`.<br>**TYPE:**`int` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``ScoreString ¶

Bases: `SingleKeyEvalConfig`

Configuration for a score string evaluator.

This is like the criteria evaluator but it is configured by
default to return a score on the scale from 1-10.

It is recommended to normalize these scores
by setting `normalize_by` to 10.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `criteria` | The criteria to evaluate.<br>**TYPE:**`CRITERIA_TYPE | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |
| `normalize_by` | If you want to normalize the score, the denominator to use.<br>If not provided, the score will be between 1 and 10.<br>**TYPE:**`float | None` |
| `prompt` | The prompt template to use for evaluation.<br>**TYPE:**`BasePromptTemplate | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``LabeledScoreString ¶

Bases: `ScoreString`

Configuration for a labeled score string evaluator.

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

### ``arun\_on\_dataset`async`¶

```
arun_on_dataset(
    client: Client | None,
    dataset_name: str,
    llm_or_chain_factory: MODEL_OR_CHAIN_FACTORY,
    *,
    evaluation: RunEvalConfig | None = None,
    dataset_version: datetime | str | None = None,
    concurrency_level: int = 5,
    project_name: str | None = None,
    project_metadata: dict[str, Any] | None = None,
    verbose: bool = False,
    revision_id: str | None = None,
    **kwargs: Any,
) -> dict[str, Any]
```

Run on dataset.

Run the Chain or language model on a dataset and store traces
to the specified project name.

For the (usually faster) async version of this function,
see `arun_on_dataset`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `dataset_name` | Name of the dataset to run the chain on.<br>**TYPE:**`str` |
| `llm_or_chain_factory` | Language model or Chain constructor to run<br>over the dataset. The Chain constructor is used to permit<br>independent calls on each example without carrying over state.<br>**TYPE:**`MODEL_OR_CHAIN_FACTORY` |
| `evaluation` | Configuration for evaluators to run on the<br>results of the chain.<br>**TYPE:**`RunEvalConfig | None`**DEFAULT:**`None` |
| `dataset_version` | Optional version of the dataset.<br>**TYPE:**`datetime | str | None`**DEFAULT:**`None` |
| `concurrency_level` | The number of async tasks to run concurrently.<br>**TYPE:**`int`**DEFAULT:**`5` |
| `project_name` | Name of the project to store the traces in.<br>Defaults to `{dataset_name}-{chain class name}-{datetime}`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `project_metadata` | Optional metadata to add to the project.<br>Useful for storing information the test variant.<br>(prompt version, model version, etc.)<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `client` | LangSmith client to use to access the dataset and to<br>log feedback and run traces.<br>**TYPE:**`Client | None` |
| `verbose` | Whether to print progress.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `revision_id` | Optional revision identifier to assign this test run to<br>track the performance of different versions of your system.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `**kwargs` | Should not be used, but is provided for backwards compatibility.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | `dict` containing the run's project name and the resulting model outputs. |

Examples:

```
from langsmith import Client
from langchain_openai import ChatOpenAI
from langchain_classic.chains import LLMChain
from langchain_classic.smith import smith_eval.RunEvalConfig, run_on_dataset

# Chains may have memory. Passing in a constructor function lets the
# evaluation framework avoid cross-contamination between runs.
def construct_chain():
    model = ChatOpenAI(temperature=0)
    chain = LLMChain.from_string(
        model,
        "What's the answer to {your_input_key}"
    )
    return chain

# Load off-the-shelf evaluators via config or the EvaluatorType (string or enum)
evaluation_config = smith_eval.RunEvalConfig(
    evaluators=[\
        "qa",  # "Correctness" against a reference answer\
        "embedding_distance",\
        smith_eval.RunEvalConfig.Criteria("helpfulness"),\
        smith_eval.RunEvalConfig.Criteria({\
            "fifth-grader-score": "Do you have to be smarter than a fifth "\
            "grader to answer this question?"\
        }),\
    ]
)

client = Client()
await arun_on_dataset(
    client,
    dataset_name="<my_dataset_name>",
    llm_or_chain_factory=construct_chain,
    evaluation=evaluation_config,
)
```

You can also create custom evaluators by subclassing the `StringEvaluator or
LangSmith's`RunEvaluator\` classes.

```
from typing import Optional
from langchain_classic.evaluation import StringEvaluator

class MyStringEvaluator(StringEvaluator):
    @property
    def requires_input(self) -> bool:
        return False

    @property
    def requires_reference(self) -> bool:
        return True

    @property
    def evaluation_name(self) -> str:
        return "exact_match"

    def _evaluate_strings(
        self, prediction, reference=None, input=None, **kwargs
    ) -> dict:
        return {"score": prediction == reference}

evaluation_config = smith_eval.RunEvalConfig(
    custom_evaluators=[MyStringEvaluator()],
)

await arun_on_dataset(
    client,
    dataset_name="<my_dataset_name>",
    llm_or_chain_factory=construct_chain,
    evaluation=evaluation_config,
)
```

### ``run\_on\_dataset ¶

```
run_on_dataset(
    client: Client | None,
    dataset_name: str,
    llm_or_chain_factory: MODEL_OR_CHAIN_FACTORY,
    *,
    evaluation: RunEvalConfig | None = None,
    dataset_version: datetime | str | None = None,
    concurrency_level: int = 5,
    project_name: str | None = None,
    project_metadata: dict[str, Any] | None = None,
    verbose: bool = False,
    revision_id: str | None = None,
    **kwargs: Any,
) -> dict[str, Any]
```

Run on dataset.

Run the Chain or language model on a dataset and store traces
to the specified project name.

For the (usually faster) async version of this function,
see `arun_on_dataset`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `dataset_name` | Name of the dataset to run the chain on.<br>**TYPE:**`str` |
| `llm_or_chain_factory` | Language model or Chain constructor to run<br>over the dataset. The Chain constructor is used to permit<br>independent calls on each example without carrying over state.<br>**TYPE:**`MODEL_OR_CHAIN_FACTORY` |
| `evaluation` | Configuration for evaluators to run on the<br>results of the chain.<br>**TYPE:**`RunEvalConfig | None`**DEFAULT:**`None` |
| `dataset_version` | Optional version of the dataset.<br>**TYPE:**`datetime | str | None`**DEFAULT:**`None` |
| `concurrency_level` | The number of async tasks to run concurrently.<br>**TYPE:**`int`**DEFAULT:**`5` |
| `project_name` | Name of the project to store the traces in.<br>Defaults to `{dataset_name}-{chain class name}-{datetime}`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `project_metadata` | Optional metadata to add to the project.<br>Useful for storing information the test variant.<br>(prompt version, model version, etc.)<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `client` | LangSmith client to use to access the dataset and to<br>log feedback and run traces.<br>**TYPE:**`Client | None` |
| `verbose` | Whether to print progress.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `revision_id` | Optional revision identifier to assign this test run to<br>track the performance of different versions of your system.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `**kwargs` | Should not be used, but is provided for backwards compatibility.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | `dict` containing the run's project name and the resulting model outputs. |

Examples:

```
from langsmith import Client
from langchain_openai import ChatOpenAI
from langchain_classic.chains import LLMChain
from langchain_classic.smith import smith_eval.RunEvalConfig, run_on_dataset

# Chains may have memory. Passing in a constructor function lets the
# evaluation framework avoid cross-contamination between runs.
def construct_chain():
    model = ChatOpenAI(temperature=0)
    chain = LLMChain.from_string(
        model,
        "What's the answer to {your_input_key}"
    )
    return chain

# Load off-the-shelf evaluators via config or the EvaluatorType (string or enum)
evaluation_config = smith_eval.RunEvalConfig(
    evaluators=[\
        "qa",  # "Correctness" against a reference answer\
        "embedding_distance",\
        smith_eval.RunEvalConfig.Criteria("helpfulness"),\
        smith_eval.RunEvalConfig.Criteria({\
            "fifth-grader-score": "Do you have to be smarter than a fifth "\
            "grader to answer this question?"\
        }),\
    ]
)

client = Client()
run_on_dataset(
    client,
    dataset_name="<my_dataset_name>",
    llm_or_chain_factory=construct_chain,
    evaluation=evaluation_config,
)
```

You can also create custom evaluators by subclassing the `StringEvaluator` or
LangSmith's `RunEvaluator` classes.

```
from typing import Optional
from langchain_classic.evaluation import StringEvaluator

class MyStringEvaluator(StringEvaluator):
    @property
    def requires_input(self) -> bool:
        return False

    @property
    def requires_reference(self) -> bool:
        return True

    @property
    def evaluation_name(self) -> str:
        return "exact_match"

    def _evaluate_strings(
        self, prediction, reference=None, input=None, **kwargs
    ) -> dict:
        return {"score": prediction == reference}

evaluation_config = smith_eval.RunEvalConfig(
    custom_evaluators=[MyStringEvaluator()],
)

run_on_dataset(
    client,
    dataset_name="<my_dataset_name>",
    llm_or_chain_factory=construct_chain,
    evaluation=evaluation_config,
)
```

## ``langchain\_classic.smith.evaluation.config ¶

Configuration for run evaluators.

### ``EvalConfig ¶

Bases: `BaseModel`

Configuration for a given run evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `evaluator_type` | The type of evaluator to use.<br>**TYPE:**`EvaluatorType` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

#### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

### ``RunEvalConfig ¶

Bases: `BaseModel`

Configuration for a run evaluation.

#### ``evaluators`class-attribute``instance-attribute`¶

```
evaluators: list[SINGLE_EVAL_CONFIG_TYPE | CUSTOM_EVALUATOR_TYPE] = Field(
    default_factory=list
)
```

Configurations for which evaluators to apply to the dataset run.
Each can be the string of an
`EvaluatorType <langchain.evaluation.schema.EvaluatorType>`, such
as `EvaluatorType.QA`, the evaluator type string ("qa"), or a configuration for a
given evaluator
(e.g.,
`RunEvalConfig.QA <langchain.smith.evaluation.config.RunEvalConfig.QA>`).

#### ``custom\_evaluators`class-attribute``instance-attribute`¶

```
custom_evaluators: list[CUSTOM_EVALUATOR_TYPE] | None = None
```

Custom evaluators to apply to the dataset run.

#### ``batch\_evaluators`class-attribute``instance-attribute`¶

```
batch_evaluators: list[BATCH_EVALUATOR_LIKE] | None = None
```

Evaluators that run on an aggregate/batch level.

These generate one or more metrics that are assigned to the full test run.
As a result, they are not associated with individual traces.

#### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

#### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

#### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

#### ``eval\_llm`class-attribute``instance-attribute`¶

```
eval_llm: BaseLanguageModel | None = None
```

The language model to pass to any evaluators that require one.

#### ``Criteria ¶

Bases: `SingleKeyEvalConfig`

Configuration for a reference-free criteria evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `criteria` | The criteria to evaluate.<br>**TYPE:**`CRITERIA_TYPE | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``LabeledCriteria ¶

Bases: `SingleKeyEvalConfig`

Configuration for a labeled (with references) criteria evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `criteria` | The criteria to evaluate.<br>**TYPE:**`CRITERIA_TYPE | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``EmbeddingDistance ¶

Bases: `SingleKeyEvalConfig`

Configuration for an embedding distance evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `embeddings` | The embeddings to use for computing the distance.<br>**TYPE:**`Embeddings | None` |
| `distance_metric` | The distance metric to use for computing the distance.<br>**TYPE:**`EmbeddingDistance | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``StringDistance ¶

Bases: `SingleKeyEvalConfig`

Configuration for a string distance evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `distance` | The string distance metric to use (`damerau_levenshtein`,<br>`levenshtein`, `jaro`, or `jaro_winkler`).<br>**TYPE:**`StringDistance | None` |
| `normalize_score` | Whether to normalize the distance to between 0 and 1.<br>Applies only to the Levenshtein and Damerau-Levenshtein distances.<br>**TYPE:**`bool` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``QA ¶

Bases: `SingleKeyEvalConfig`

Configuration for a QA evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `prompt` | The prompt template to use for generating the question.<br>**TYPE:**`BasePromptTemplate | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``ContextQA ¶

Bases: `SingleKeyEvalConfig`

Configuration for a context-based QA evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `prompt` | The prompt template to use for generating the question.<br>**TYPE:**`BasePromptTemplate | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``CoTQA ¶

Bases: `SingleKeyEvalConfig`

Configuration for a context-based QA evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `prompt` | The prompt template to use for generating the question.<br>**TYPE:**`BasePromptTemplate | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``JsonValidity ¶

Bases: `SingleKeyEvalConfig`

Configuration for a json validity evaluator.

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``JsonEqualityEvaluator ¶

Bases: `EvalConfig`

Configuration for a json equality evaluator.

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``ExactMatch ¶

Bases: `SingleKeyEvalConfig`

Configuration for an exact match string evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `ignore_case` | Whether to ignore case when comparing strings.<br>**TYPE:**`bool` |
| `ignore_punctuation` | Whether to ignore punctuation when comparing strings.<br>**TYPE:**`bool` |
| `ignore_numbers` | Whether to ignore numbers when comparing strings.<br>**TYPE:**`bool` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``RegexMatch ¶

Bases: `SingleKeyEvalConfig`

Configuration for a regex match string evaluator.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `flags` | The flags to pass to the regex. Example: `re.IGNORECASE`.<br>**TYPE:**`int` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``ScoreString ¶

Bases: `SingleKeyEvalConfig`

Configuration for a score string evaluator.

This is like the criteria evaluator but it is configured by
default to return a score on the scale from 1-10.

It is recommended to normalize these scores
by setting `normalize_by` to 10.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `criteria` | The criteria to evaluate.<br>**TYPE:**`CRITERIA_TYPE | None` |
| `llm` | The language model to use for the evaluation chain.<br>**TYPE:**`BaseLanguageModel | None` |
| `normalize_by` | If you want to normalize the score, the denominator to use.<br>If not provided, the score will be between 1 and 10.<br>**TYPE:**`float | None` |
| `prompt` | The prompt template to use for evaluation.<br>**TYPE:**`BasePromptTemplate | None` |

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

#### ``LabeledScoreString ¶

Bases: `ScoreString`

Configuration for a labeled score string evaluator.

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

##### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

##### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

##### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

##### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

### ``SingleKeyEvalConfig ¶

Bases: `EvalConfig`

Configuration for a run evaluator that only requires a single key.

| METHOD | DESCRIPTION |
| --- | --- |
| `get_kwargs` | Get the keyword arguments for the `load_evaluator` call. |

#### ``reference\_key`class-attribute``instance-attribute`¶

```
reference_key: str | None = None
```

The key in the dataset run to use as the reference string.
If not provided, we will attempt to infer automatically.

#### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the traced run's outputs dictionary to use to
represent the prediction. If not provided, it will be inferred
automatically.

#### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the traced run's inputs dictionary to use to represent the
input. If not provided, it will be inferred automatically.

#### ``get\_kwargs ¶

```
get_kwargs() -> dict[str, Any]
```

Get the keyword arguments for the `load_evaluator` call.

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | The keyword arguments for the `load_evaluator` call. |

## ``langchain\_classic.smith.evaluation.progress ¶

A simple progress bar for the console.

### ``ProgressBarCallback ¶

Bases: `BaseCallbackHandler`

A simple progress bar for the console.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize the progress bar. |
| `increment` | Increment the counter and update the progress bar. |
| `on_chain_error` | Run when chain errors. |
| `on_chain_end` | Run when chain ends running. |
| `on_retriever_error` | Run when Retriever errors. |
| `on_retriever_end` | Run when Retriever ends running. |
| `on_llm_error` | Run when LLM errors. |
| `on_llm_end` | Run when LLM ends running. |
| `on_tool_error` | Run when tool errors. |
| `on_tool_end` | Run when the tool ends running. |
| `on_text` | Run on an arbitrary text. |
| `on_retry` | Run on a retry event. |
| `on_custom_event` | Override to define a handler for a custom event. |
| `on_llm_start` | Run when LLM starts running. |
| `on_chat_model_start` | Run when a chat model starts running. |
| `on_retriever_start` | Run when the Retriever starts running. |
| `on_chain_start` | Run when a chain starts running. |
| `on_tool_start` | Run when the tool starts running. |
| `on_agent_action` | Run on agent action. |
| `on_agent_finish` | Run on the agent end. |
| `on_llm_new_token` | Run on new output token. Only available when streaming is enabled. |

#### ``raise\_error`class-attribute``instance-attribute`¶

```
raise_error: bool = False
```

Whether to raise an error if an exception occurs.

#### ``run\_inline`class-attribute``instance-attribute`¶

```
run_inline: bool = False
```

Whether to run the callback inline.

#### ``ignore\_llm`property`¶

```
ignore_llm: bool
```

Whether to ignore LLM callbacks.

#### ``ignore\_retry`property`¶

```
ignore_retry: bool
```

Whether to ignore retry callbacks.

#### ``ignore\_chain`property`¶

```
ignore_chain: bool
```

Whether to ignore chain callbacks.

#### ``ignore\_agent`property`¶

```
ignore_agent: bool
```

Whether to ignore agent callbacks.

#### ``ignore\_retriever`property`¶

```
ignore_retriever: bool
```

Whether to ignore retriever callbacks.

#### ``ignore\_chat\_model`property`¶

```
ignore_chat_model: bool
```

Whether to ignore chat model callbacks.

#### ``ignore\_custom\_event`property`¶

```
ignore_custom_event: bool
```

Ignore custom event.

#### ``\_\_init\_\_ ¶

```
__init__(total: int, ncols: int = 50, end_with: str = '\n')
```

Initialize the progress bar.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `total` | The total number of items to be processed.<br>**TYPE:**`int` |
| `ncols` | The character width of the progress bar.<br>**TYPE:**`int`**DEFAULT:**`50` |
| `end_with` | Last string to print after progress bar reaches end.<br>**TYPE:**`str`**DEFAULT:**`'\n'` |
| `**kwargs` | Additional keyword arguments. |

#### ``increment ¶

```
increment() -> None
```

Increment the counter and update the progress bar.

#### ``on\_chain\_error ¶

```
on_chain_error(
    error: BaseException,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run when chain errors.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `error` | The error that occurred.<br>**TYPE:**`BaseException` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_chain\_end ¶

```
on_chain_end(
    outputs: dict[str, Any],
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run when chain ends running.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `outputs` | The outputs of the chain.<br>**TYPE:**`dict[str, Any]` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_retriever\_error ¶

```
on_retriever_error(
    error: BaseException,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run when Retriever errors.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `error` | The error that occurred.<br>**TYPE:**`BaseException` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_retriever\_end ¶

```
on_retriever_end(
    documents: Sequence[Document],
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run when Retriever ends running.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `documents` | The documents retrieved.<br>**TYPE:**`Sequence[Document]` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_llm\_error ¶

```
on_llm_error(
    error: BaseException,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run when LLM errors.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `error` | The error that occurred.<br>**TYPE:**`BaseException` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_llm\_end ¶

```
on_llm_end(
    response: LLMResult,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run when LLM ends running.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `response` | The response which was generated.<br>**TYPE:**`LLMResult` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_tool\_error ¶

```
on_tool_error(
    error: BaseException,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run when tool errors.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `error` | The error that occurred.<br>**TYPE:**`BaseException` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_tool\_end ¶

```
on_tool_end(
    output: str, *, run_id: UUID, parent_run_id: UUID | None = None, **kwargs: Any
) -> Any
```

Run when the tool ends running.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `output` | The output of the tool.<br>**TYPE:**`Any` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_text ¶

```
on_text(
    text: str, *, run_id: UUID, parent_run_id: UUID | None = None, **kwargs: Any
) -> Any
```

Run on an arbitrary text.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `text` | The text.<br>**TYPE:**`str` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_retry ¶

```
on_retry(
    retry_state: RetryCallState,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run on a retry event.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `retry_state` | The retry state.<br>**TYPE:**`RetryCallState` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_custom\_event ¶

```
on_custom_event(
    name: str,
    data: Any,
    *,
    run_id: UUID,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    **kwargs: Any,
) -> Any
```

Override to define a handler for a custom event.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `name` | The name of the custom event.<br>**TYPE:**`str` |
| `data` | The data for the custom event. Format will match<br>the format specified by the user.<br>**TYPE:**`Any` |
| `run_id` | The ID of the run.<br>**TYPE:**`UUID` |
| `tags` | The tags associated with the custom event<br>(includes inherited tags).<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata associated with the custom event<br>(includes inherited metadata).<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |

#### ``on\_llm\_start ¶

```
on_llm_start(
    serialized: dict[str, Any],
    prompts: list[str],
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    **kwargs: Any,
) -> Any
```

Run when LLM starts running.

Warning

This method is called for non-chat models (regular LLMs). If you're
implementing a handler for a chat model, you should use
`on_chat_model_start` instead.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `serialized` | The serialized LLM.<br>**TYPE:**`dict[str, Any]` |
| `prompts` | The prompts.<br>**TYPE:**`list[str]` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `tags` | The tags.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_chat\_model\_start ¶

```
on_chat_model_start(
    serialized: dict[str, Any],
    messages: list[list[BaseMessage]],
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    **kwargs: Any,
) -> Any
```

Run when a chat model starts running.

Warning

This method is called for chat models. If you're implementing a handler for
a non-chat model, you should use `on_llm_start` instead.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `serialized` | The serialized chat model.<br>**TYPE:**`dict[str, Any]` |
| `messages` | The messages.<br>**TYPE:**`list[list[BaseMessage]]` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `tags` | The tags.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_retriever\_start ¶

```
on_retriever_start(
    serialized: dict[str, Any],
    query: str,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    **kwargs: Any,
) -> Any
```

Run when the Retriever starts running.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `serialized` | The serialized Retriever.<br>**TYPE:**`dict[str, Any]` |
| `query` | The query.<br>**TYPE:**`str` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `tags` | The tags.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_chain\_start ¶

```
on_chain_start(
    serialized: dict[str, Any],
    inputs: dict[str, Any],
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    **kwargs: Any,
) -> Any
```

Run when a chain starts running.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `serialized` | The serialized chain.<br>**TYPE:**`dict[str, Any]` |
| `inputs` | The inputs.<br>**TYPE:**`dict[str, Any]` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `tags` | The tags.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_tool\_start ¶

```
on_tool_start(
    serialized: dict[str, Any],
    input_str: str,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    inputs: dict[str, Any] | None = None,
    **kwargs: Any,
) -> Any
```

Run when the tool starts running.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `serialized` | The serialized chain.<br>**TYPE:**`dict[str, Any]` |
| `input_str` | The input string.<br>**TYPE:**`str` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `tags` | The tags.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | The metadata.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `inputs` | The inputs.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_agent\_action ¶

```
on_agent_action(
    action: AgentAction,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run on agent action.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `action` | The agent action.<br>**TYPE:**`AgentAction` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_agent\_finish ¶

```
on_agent_finish(
    finish: AgentFinish,
    *,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run on the agent end.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `finish` | The agent finish.<br>**TYPE:**`AgentFinish` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``on\_llm\_new\_token ¶

```
on_llm_new_token(
    token: str,
    *,
    chunk: GenerationChunk | ChatGenerationChunk | None = None,
    run_id: UUID,
    parent_run_id: UUID | None = None,
    **kwargs: Any,
) -> Any
```

Run on new output token. Only available when streaming is enabled.

For both chat models and non-chat models (legacy LLMs).

| PARAMETER | DESCRIPTION |
| --- | --- |
| `token` | The new token.<br>**TYPE:**`str` |
| `chunk` | The new generated chunk, containing content and other information.<br>**TYPE:**`GenerationChunk | ChatGenerationChunk | None`**DEFAULT:**`None` |
| `run_id` | The run ID. This is the ID of the current run.<br>**TYPE:**`UUID` |
| `parent_run_id` | The parent run ID. This is the ID of the parent run.<br>**TYPE:**`UUID | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

## ``langchain\_classic.smith.evaluation.runner\_utils ¶

Utilities for running language models or Chains over datasets.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `arun_on_dataset` | Run on dataset. |
| `run_on_dataset` | Run on dataset. |

### ``ChatModelInput ¶

Bases: `TypedDict`

Input for a chat model.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `messages` | List of chat messages. |

### ``EvalError ¶

Bases: `dict`

Your architecture raised an error.

| METHOD | DESCRIPTION |
| --- | --- |
| `__init__` | Initialize the `EvalError` with an error and additional attributes. |
| `__getattr__` | Get an attribute from the `EvalError`. |

#### ``\_\_init\_\_ ¶

```
__init__(Error: BaseException, **kwargs: Any) -> None
```

Initialize the `EvalError` with an error and additional attributes.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `Error` | The error that occurred.<br>**TYPE:**`BaseException` |
| `**kwargs` | Additional attributes to include in the error.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

#### ``\_\_getattr\_\_ ¶

```
__getattr__(name: str) -> Any
```

Get an attribute from the `EvalError`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `name` | The name of the attribute to get.<br>**TYPE:**`str` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Any` | The value of the attribute. |

| RAISES | DESCRIPTION |
| --- | --- |
| `AttributeError` | If the attribute does not exist. |

### ``InputFormatError ¶

Bases: `Exception`

Raised when the input format is invalid.

### ``TestResult ¶

Bases: `dict`

A dictionary of the results of a single test run.

| METHOD | DESCRIPTION |
| --- | --- |
| `get_aggregate_feedback` | Return quantiles for the feedback scores. |
| `to_dataframe` | Convert the results to a dataframe. |

#### ``get\_aggregate\_feedback ¶

```
get_aggregate_feedback() -> DataFrame
```

Return quantiles for the feedback scores.

This method calculates and prints the quantiles for the feedback scores
across all feedback keys.

| RETURNS | DESCRIPTION |
| --- | --- |
| `DataFrame` | A DataFrame containing the quantiles for each feedback key. |

#### ``to\_dataframe ¶

```
to_dataframe() -> DataFrame
```

Convert the results to a dataframe.

### ``arun\_on\_dataset`async`¶

```
arun_on_dataset(
    client: Client | None,
    dataset_name: str,
    llm_or_chain_factory: MODEL_OR_CHAIN_FACTORY,
    *,
    evaluation: RunEvalConfig | None = None,
    dataset_version: datetime | str | None = None,
    concurrency_level: int = 5,
    project_name: str | None = None,
    project_metadata: dict[str, Any] | None = None,
    verbose: bool = False,
    revision_id: str | None = None,
    **kwargs: Any,
) -> dict[str, Any]
```

Run on dataset.

Run the Chain or language model on a dataset and store traces
to the specified project name.

For the (usually faster) async version of this function,
see `arun_on_dataset`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `dataset_name` | Name of the dataset to run the chain on.<br>**TYPE:**`str` |
| `llm_or_chain_factory` | Language model or Chain constructor to run<br>over the dataset. The Chain constructor is used to permit<br>independent calls on each example without carrying over state.<br>**TYPE:**`MODEL_OR_CHAIN_FACTORY` |
| `evaluation` | Configuration for evaluators to run on the<br>results of the chain.<br>**TYPE:**`RunEvalConfig | None`**DEFAULT:**`None` |
| `dataset_version` | Optional version of the dataset.<br>**TYPE:**`datetime | str | None`**DEFAULT:**`None` |
| `concurrency_level` | The number of async tasks to run concurrently.<br>**TYPE:**`int`**DEFAULT:**`5` |
| `project_name` | Name of the project to store the traces in.<br>Defaults to `{dataset_name}-{chain class name}-{datetime}`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `project_metadata` | Optional metadata to add to the project.<br>Useful for storing information the test variant.<br>(prompt version, model version, etc.)<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `client` | LangSmith client to use to access the dataset and to<br>log feedback and run traces.<br>**TYPE:**`Client | None` |
| `verbose` | Whether to print progress.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `revision_id` | Optional revision identifier to assign this test run to<br>track the performance of different versions of your system.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `**kwargs` | Should not be used, but is provided for backwards compatibility.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | `dict` containing the run's project name and the resulting model outputs. |

Examples:

```
from langsmith import Client
from langchain_openai import ChatOpenAI
from langchain_classic.chains import LLMChain
from langchain_classic.smith import smith_eval.RunEvalConfig, run_on_dataset

# Chains may have memory. Passing in a constructor function lets the
# evaluation framework avoid cross-contamination between runs.
def construct_chain():
    model = ChatOpenAI(temperature=0)
    chain = LLMChain.from_string(
        model,
        "What's the answer to {your_input_key}"
    )
    return chain

# Load off-the-shelf evaluators via config or the EvaluatorType (string or enum)
evaluation_config = smith_eval.RunEvalConfig(
    evaluators=[\
        "qa",  # "Correctness" against a reference answer\
        "embedding_distance",\
        smith_eval.RunEvalConfig.Criteria("helpfulness"),\
        smith_eval.RunEvalConfig.Criteria({\
            "fifth-grader-score": "Do you have to be smarter than a fifth "\
            "grader to answer this question?"\
        }),\
    ]
)

client = Client()
await arun_on_dataset(
    client,
    dataset_name="<my_dataset_name>",
    llm_or_chain_factory=construct_chain,
    evaluation=evaluation_config,
)
```

You can also create custom evaluators by subclassing the `StringEvaluator or
LangSmith's`RunEvaluator\` classes.

```
from typing import Optional
from langchain_classic.evaluation import StringEvaluator

class MyStringEvaluator(StringEvaluator):
    @property
    def requires_input(self) -> bool:
        return False

    @property
    def requires_reference(self) -> bool:
        return True

    @property
    def evaluation_name(self) -> str:
        return "exact_match"

    def _evaluate_strings(
        self, prediction, reference=None, input=None, **kwargs
    ) -> dict:
        return {"score": prediction == reference}

evaluation_config = smith_eval.RunEvalConfig(
    custom_evaluators=[MyStringEvaluator()],
)

await arun_on_dataset(
    client,
    dataset_name="<my_dataset_name>",
    llm_or_chain_factory=construct_chain,
    evaluation=evaluation_config,
)
```

### ``run\_on\_dataset ¶

```
run_on_dataset(
    client: Client | None,
    dataset_name: str,
    llm_or_chain_factory: MODEL_OR_CHAIN_FACTORY,
    *,
    evaluation: RunEvalConfig | None = None,
    dataset_version: datetime | str | None = None,
    concurrency_level: int = 5,
    project_name: str | None = None,
    project_metadata: dict[str, Any] | None = None,
    verbose: bool = False,
    revision_id: str | None = None,
    **kwargs: Any,
) -> dict[str, Any]
```

Run on dataset.

Run the Chain or language model on a dataset and store traces
to the specified project name.

For the (usually faster) async version of this function,
see `arun_on_dataset`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `dataset_name` | Name of the dataset to run the chain on.<br>**TYPE:**`str` |
| `llm_or_chain_factory` | Language model or Chain constructor to run<br>over the dataset. The Chain constructor is used to permit<br>independent calls on each example without carrying over state.<br>**TYPE:**`MODEL_OR_CHAIN_FACTORY` |
| `evaluation` | Configuration for evaluators to run on the<br>results of the chain.<br>**TYPE:**`RunEvalConfig | None`**DEFAULT:**`None` |
| `dataset_version` | Optional version of the dataset.<br>**TYPE:**`datetime | str | None`**DEFAULT:**`None` |
| `concurrency_level` | The number of async tasks to run concurrently.<br>**TYPE:**`int`**DEFAULT:**`5` |
| `project_name` | Name of the project to store the traces in.<br>Defaults to `{dataset_name}-{chain class name}-{datetime}`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `project_metadata` | Optional metadata to add to the project.<br>Useful for storing information the test variant.<br>(prompt version, model version, etc.)<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `client` | LangSmith client to use to access the dataset and to<br>log feedback and run traces.<br>**TYPE:**`Client | None` |
| `verbose` | Whether to print progress.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `revision_id` | Optional revision identifier to assign this test run to<br>track the performance of different versions of your system.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `**kwargs` | Should not be used, but is provided for backwards compatibility.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | `dict` containing the run's project name and the resulting model outputs. |

Examples:

```
from langsmith import Client
from langchain_openai import ChatOpenAI
from langchain_classic.chains import LLMChain
from langchain_classic.smith import smith_eval.RunEvalConfig, run_on_dataset

# Chains may have memory. Passing in a constructor function lets the
# evaluation framework avoid cross-contamination between runs.
def construct_chain():
    model = ChatOpenAI(temperature=0)
    chain = LLMChain.from_string(
        model,
        "What's the answer to {your_input_key}"
    )
    return chain

# Load off-the-shelf evaluators via config or the EvaluatorType (string or enum)
evaluation_config = smith_eval.RunEvalConfig(
    evaluators=[\
        "qa",  # "Correctness" against a reference answer\
        "embedding_distance",\
        smith_eval.RunEvalConfig.Criteria("helpfulness"),\
        smith_eval.RunEvalConfig.Criteria({\
            "fifth-grader-score": "Do you have to be smarter than a fifth "\
            "grader to answer this question?"\
        }),\
    ]
)

client = Client()
run_on_dataset(
    client,
    dataset_name="<my_dataset_name>",
    llm_or_chain_factory=construct_chain,
    evaluation=evaluation_config,
)
```

You can also create custom evaluators by subclassing the `StringEvaluator` or
LangSmith's `RunEvaluator` classes.

```
from typing import Optional
from langchain_classic.evaluation import StringEvaluator

class MyStringEvaluator(StringEvaluator):
    @property
    def requires_input(self) -> bool:
        return False

    @property
    def requires_reference(self) -> bool:
        return True

    @property
    def evaluation_name(self) -> str:
        return "exact_match"

    def _evaluate_strings(
        self, prediction, reference=None, input=None, **kwargs
    ) -> dict:
        return {"score": prediction == reference}

evaluation_config = smith_eval.RunEvalConfig(
    custom_evaluators=[MyStringEvaluator()],
)

run_on_dataset(
    client,
    dataset_name="<my_dataset_name>",
    llm_or_chain_factory=construct_chain,
    evaluation=evaluation_config,
)
```

## ``langchain\_classic.smith.evaluation.string\_run\_evaluator ¶

Run evaluator wrapper for string evaluators.

### ``ChainStringRunMapper ¶

Bases: `StringRunMapper`

Extract items to evaluate from the run object from a chain.

| METHOD | DESCRIPTION |
| --- | --- |
| `map` | Maps the Run to a dictionary. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the object to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `__call__` | Maps the Run to a dictionary. |

#### ``input\_key`class-attribute``instance-attribute`¶

```
input_key: str | None = None
```

The key from the model Run's inputs to use as the eval input.
If not provided, will use the only input key or raise an
error if there are multiple.

#### ``prediction\_key`class-attribute``instance-attribute`¶

```
prediction_key: str | None = None
```

The key from the model Run's outputs to use as the eval prediction.
If not provided, will use the only output key or raise an error
if there are multiple.

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``output\_keys`property`¶

```
output_keys: list[str]
```

The keys to extract from the run.

#### ``map ¶

```
map(run: Run) -> dict[str, str]
```

Maps the Run to a dictionary.

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the object to JSON.

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the class has deprecated attributes. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON serializable object or a `SerializedNotImplemented` object. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``\_\_call\_\_ ¶

```
__call__(run: Run) -> dict[str, str]
```

Maps the Run to a dictionary.

### ``LLMStringRunMapper ¶

Bases: `StringRunMapper`

Extract items to evaluate from the run object.

| METHOD | DESCRIPTION |
| --- | --- |
| `serialize_chat_messages` | Extract the input messages from the run. |
| `serialize_inputs` | Serialize inputs. |
| `serialize_outputs` | Serialize outputs. |
| `map` | Maps the Run to a dictionary. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the object to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `__call__` | Maps the Run to a dictionary. |

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``output\_keys`property`¶

```
output_keys: list[str]
```

The keys to extract from the run.

#### ``serialize\_chat\_messages ¶

```
serialize_chat_messages(messages: list[dict] | list[list[dict]]) -> str
```

Extract the input messages from the run.

#### ``serialize\_inputs ¶

```
serialize_inputs(inputs: dict) -> str
```

Serialize inputs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | The inputs from the run, expected to contain prompts or messages.<br>**TYPE:**`dict` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The serialized input text from the prompts or messages. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If neither prompts nor messages are found in the inputs. |

#### ``serialize\_outputs ¶

```
serialize_outputs(outputs: dict) -> str
```

Serialize outputs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `outputs` | The outputs from the run, expected to contain generations.<br>**TYPE:**`dict` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The serialized output text from the first generation. |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If no generations are found in the outputs, |

#### ``map ¶

```
map(run: Run) -> dict[str, str]
```

Maps the Run to a dictionary.

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the object to JSON.

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the class has deprecated attributes. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON serializable object or a `SerializedNotImplemented` object. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``\_\_call\_\_ ¶

```
__call__(run: Run) -> dict[str, str]
```

Maps the Run to a dictionary.

### ``StringExampleMapper ¶

Bases: `Serializable`

Map an example, or row in the dataset, to the inputs of an evaluation.

| METHOD | DESCRIPTION |
| --- | --- |
| `serialize_chat_messages` | Extract the input messages from the run. |
| `map` | Maps the Example, or dataset row to a dictionary. |
| `__call__` | Maps the Run and Example to a dictionary. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the object to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |

#### ``output\_keys`property`¶

```
output_keys: list[str]
```

The keys to extract from the run.

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``serialize\_chat\_messages ¶

```
serialize_chat_messages(messages: list[dict]) -> str
```

Extract the input messages from the run.

#### ``map ¶

```
map(example: Example) -> dict[str, str]
```

Maps the Example, or dataset row to a dictionary.

#### ``\_\_call\_\_ ¶

```
__call__(example: Example) -> dict[str, str]
```

Maps the Run and Example to a dictionary.

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the object to JSON.

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the class has deprecated attributes. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON serializable object or a `SerializedNotImplemented` object. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

### ``StringRunEvaluatorChain ¶

Bases: `Chain`, `RunEvaluator`

Evaluate Run and optional examples.

| METHOD | DESCRIPTION |
| --- | --- |
| `evaluate_run` | Evaluate an example. |
| `aevaluate_run` | Evaluate an example. |
| `from_run_and_data_type` | Create a StringRunEvaluatorChain. |
| `get_name` | Get the name of the `Runnable`. |
| `get_input_schema` | Get a Pydantic model that can be used to validate input to the `Runnable`. |
| `get_input_jsonschema` | Get a JSON schema that represents the input to the `Runnable`. |
| `get_output_schema` | Get a Pydantic model that can be used to validate output to the `Runnable`. |
| `get_output_jsonschema` | Get a JSON schema that represents the output of the `Runnable`. |
| `config_schema` | The type of config this `Runnable` accepts specified as a Pydantic model. |
| `get_config_jsonschema` | Get a JSON schema that represents the config of the `Runnable`. |
| `get_graph` | Return a graph representation of this `Runnable`. |
| `get_prompts` | Return a list of prompts used by this `Runnable`. |
| `__or__` | Runnable "or" operator. |
| `__ror__` | Runnable "reverse-or" operator. |
| `pipe` | Pipe `Runnable` objects. |
| `pick` | Pick keys from the output `dict` of this `Runnable`. |
| `assign` | Assigns new fields to the `dict` output of this `Runnable`. |
| `invoke` | Transform a single input into an output. |
| `ainvoke` | Transform a single input into an output. |
| `batch` | Default implementation runs invoke in parallel using a thread pool executor. |
| `batch_as_completed` | Run `invoke` in parallel on a list of inputs. |
| `abatch` | Default implementation runs `ainvoke` in parallel using `asyncio.gather`. |
| `abatch_as_completed` | Run `ainvoke` in parallel on a list of inputs. |
| `stream` | Default implementation of `stream`, which calls `invoke`. |
| `astream` | Default implementation of `astream`, which calls `ainvoke`. |
| `astream_log` | Stream all output from a `Runnable`, as reported to the callback system. |
| `astream_events` | Generate a stream of events. |
| `transform` | Transform inputs to outputs. |
| `atransform` | Transform inputs to outputs. |
| `bind` | Bind arguments to a `Runnable`, returning a new `Runnable`. |
| `with_config` | Bind config to a `Runnable`, returning a new `Runnable`. |
| `with_listeners` | Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`. |
| `with_alisteners` | Bind async lifecycle listeners to a `Runnable`. |
| `with_types` | Bind input and output types to a `Runnable`, returning a new `Runnable`. |
| `with_retry` | Create a new `Runnable` that retries the original `Runnable` on exceptions. |
| `map` | Return a new `Runnable` that maps a list of inputs to a list of outputs. |
| `with_fallbacks` | Add fallbacks to a `Runnable`, returning a new `Runnable`. |
| `as_tool` | Create a `BaseTool` from a `Runnable`. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the `Runnable` to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `configurable_fields` | Configure particular `Runnable` fields at runtime. |
| `configurable_alternatives` | Configure alternatives for `Runnable` objects that can be set at runtime. |
| `raise_callback_manager_deprecation` | Raise deprecation warning if callback\_manager is used. |
| `set_verbose` | Set the chain verbosity. |
| `__call__` | Execute the chain. |
| `acall` | Asynchronously execute the chain. |
| `prep_outputs` | Validate and prepare chain outputs, and save info about this run to memory. |
| `aprep_outputs` | Validate and prepare chain outputs, and save info about this run to memory. |
| `prep_inputs` | Prepare chain inputs, including adding inputs from memory. |
| `aprep_inputs` | Prepare chain inputs, including adding inputs from memory. |
| `run` | Convenience method for executing chain. |
| `arun` | Convenience method for executing chain. |
| `dict` | Dictionary representation of chain. |
| `save` | Save the chain. |
| `apply` | Call the chain on all inputs in the list. |

#### ``run\_mapper`instance-attribute`¶

```
run_mapper: StringRunMapper
```

Maps the Run to a dictionary with 'input' and 'prediction' strings.

#### ``example\_mapper`class-attribute``instance-attribute`¶

```
example_mapper: StringExampleMapper | None = None
```

Maps the Example (dataset row) to a dictionary
with a 'reference' string.

#### ``name`instance-attribute`¶

```
name: str
```

The name of the evaluation metric.

#### ``string\_evaluator`instance-attribute`¶

```
string_evaluator: StringEvaluator
```

The evaluation chain.

#### ``input\_keys`property`¶

```
input_keys: list[str]
```

Keys expected to be in the chain input.

#### ``output\_keys`property`¶

```
output_keys: list[str]
```

Keys expected to be in the chain output.

#### ``InputType`property`¶

```
InputType: type[Input]
```

Input type.

The type of input this `Runnable` accepts specified as a type annotation.

| RAISES | DESCRIPTION |
| --- | --- |
| `TypeError` | If the input type cannot be inferred. |

#### ``OutputType`property`¶

```
OutputType: type[Output]
```

Output Type.

The type of output this `Runnable` produces specified as a type annotation.

| RAISES | DESCRIPTION |
| --- | --- |
| `TypeError` | If the output type cannot be inferred. |

#### ``input\_schema`property`¶

```
input_schema: type[BaseModel]
```

The type of input this `Runnable` accepts specified as a Pydantic model.

#### ``output\_schema`property`¶

```
output_schema: type[BaseModel]
```

Output schema.

The type of output this `Runnable` produces specified as a Pydantic model.

#### ``config\_specs`property`¶

```
config_specs: list[ConfigurableFieldSpec]
```

List configurable fields for this `Runnable`.

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``memory`class-attribute``instance-attribute`¶

```
memory: BaseMemory | None = None
```

Optional memory object.
Memory is a class that gets called at the start
and at the end of every chain. At the start, memory loads variables and passes
them along in the chain. At the end, it saves any returned variables.
There are many different types of memory - please see memory docs
for the full catalog.

#### ``callbacks`class-attribute``instance-attribute`¶

```
callbacks: Callbacks = Field(default=None, exclude=True)
```

Optional list of callback handlers (or callback manager).
Callback handlers are called throughout the lifecycle of a call to a chain,
starting with on\_chain\_start, ending with on\_chain\_end or on\_chain\_error.
Each custom chain can optionally call additional callback methods, see Callback docs
for full details.

#### ``verbose`class-attribute``instance-attribute`¶

```
verbose: bool = Field(default_factory=_get_verbosity)
```

Whether or not run in verbose mode. In verbose mode, some intermediate logs
will be printed to the console. Defaults to the global `verbose` value,
accessible via `langchain.globals.get_verbose()`.

#### ``tags`class-attribute``instance-attribute`¶

```
tags: list[str] | None = None
```

Optional list of tags associated with the chain.
These tags will be associated with each call to this chain,
and passed as arguments to the handlers defined in `callbacks`.
You can use these to eg identify a specific instance of a chain with its use case.

#### ``metadata`class-attribute``instance-attribute`¶

```
metadata: dict[str, Any] | None = None
```

Optional metadata associated with the chain.
This metadata will be associated with each call to this chain,
and passed as arguments to the handlers defined in `callbacks`.
You can use these to eg identify a specific instance of a chain with its use case.

#### ``callback\_manager`class-attribute``instance-attribute`¶

```
callback_manager: BaseCallbackManager | None = Field(default=None, exclude=True)
```

\[DEPRECATED\] Use `callbacks` instead.

#### ``evaluate\_run ¶

```
evaluate_run(
    run: Run, example: Example | None = None, evaluator_run_id: UUID | None = None
) -> EvaluationResult
```

Evaluate an example.

#### ``aevaluate\_run`async`¶

```
aevaluate_run(
    run: Run, example: Example | None = None, evaluator_run_id: UUID | None = None
) -> EvaluationResult
```

Evaluate an example.

#### ``from\_run\_and\_data\_type`classmethod`¶

```
from_run_and_data_type(
    evaluator: StringEvaluator,
    run_type: str,
    data_type: DataType,
    input_key: str | None = None,
    prediction_key: str | None = None,
    reference_key: str | None = None,
    tags: list[str] | None = None,
) -> StringRunEvaluatorChain
```

Create a StringRunEvaluatorChain.

Create a StringRunEvaluatorChain from an evaluator and the run and dataset
types.

This method provides an easy way to instantiate a StringRunEvaluatorChain, by
taking an evaluator and information about the type of run and the data.
The method supports LLM and chain runs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `evaluator` | The string evaluator to use.<br>**TYPE:**`StringEvaluator` |
| `run_type` | The type of run being evaluated.<br>Supported types are LLM and Chain.<br>**TYPE:**`str` |
| `data_type` | The type of dataset used in the run.<br>**TYPE:**`DataType` |
| `input_key` | The key used to map the input from the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `prediction_key` | The key used to map the prediction from the run.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `reference_key` | The key used to map the reference from the dataset.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `tags` | List of tags to attach to the evaluation chain.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `StringRunEvaluatorChain` | The instantiated evaluation chain. |

#### ``get\_name ¶

```
get_name(suffix: str | None = None, *, name: str | None = None) -> str
```

Get the name of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `suffix` | An optional suffix to append to the name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `name` | An optional name to use instead of the `Runnable`'s name.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `str` | The name of the `Runnable`. |

#### ``get\_input\_schema ¶

```
get_input_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate input to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic input schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an input schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate input. |

#### ``get\_input\_jsonschema ¶

```
get_input_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the input to the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the input to the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_input_jsonschema())
```

Added in version 0.3.0

#### ``get\_output\_schema ¶

```
get_output_schema(config: RunnableConfig | None = None) -> type[BaseModel]
```

Get a Pydantic model that can be used to validate output to the `Runnable`.

`Runnable` objects that leverage the `configurable_fields` and
`configurable_alternatives` methods will have a dynamic output schema that
depends on which configuration the `Runnable` is invoked with.

This method allows to get an output schema for a specific configuration.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate output. |

#### ``get\_output\_jsonschema ¶

```
get_output_jsonschema(config: RunnableConfig | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the output of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | A config to use when generating the schema.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the output of the `Runnable`. |

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

runnable = RunnableLambda(add_one)

print(runnable.get_output_jsonschema())
```

Added in version 0.3.0

#### ``config\_schema ¶

```
config_schema(*, include: Sequence[str] | None = None) -> type[BaseModel]
```

The type of config this `Runnable` accepts specified as a Pydantic model.

To mark a field as configurable, see the `configurable_fields`
and `configurable_alternatives` methods.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `include` | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `type[BaseModel]` | A Pydantic model that can be used to validate config. |

#### ``get\_config\_jsonschema ¶

```
get_config_jsonschema(*, include: Sequence[str] | None = None) -> dict[str, Any]
```

Get a JSON schema that represents the config of the `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `include` | A list of fields to include in the config schema.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A JSON schema that represents the config of the `Runnable`. |

Added in version 0.3.0

#### ``get\_graph ¶

```
get_graph(config: RunnableConfig | None = None) -> Graph
```

Return a graph representation of this `Runnable`.

#### ``get\_prompts ¶

```
get_prompts(config: RunnableConfig | None = None) -> list[BasePromptTemplate]
```

Return a list of prompts used by this `Runnable`.

#### ``\_\_or\_\_ ¶

```
__or__(
    other: Runnable[Any, Other]
    | Callable[[Iterator[Any]], Iterator[Other]]
    | Callable[[AsyncIterator[Any]], AsyncIterator[Other]]
    | Callable[[Any], Other]
    | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any],
) -> RunnableSerializable[Input, Other]
```

Runnable "or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `other` | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Any, Other] | Callable[[Iterator[Any]], Iterator[Other]] | Callable[[AsyncIterator[Any]], AsyncIterator[Other]] | Callable[[Any], Other] | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``\_\_ror\_\_ ¶

```
__ror__(
    other: Runnable[Other, Any]
    | Callable[[Iterator[Other]], Iterator[Any]]
    | Callable[[AsyncIterator[Other]], AsyncIterator[Any]]
    | Callable[[Other], Any]
    | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any],
) -> RunnableSerializable[Other, Output]
```

Runnable "reverse-or" operator.

Compose this `Runnable` with another object to create a
`RunnableSequence`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `other` | Another `Runnable` or a `Runnable`-like object.<br>**TYPE:**`Runnable[Other, Any] | Callable[[Iterator[Other]], Iterator[Any]] | Callable[[AsyncIterator[Other]], AsyncIterator[Any]] | Callable[[Other], Any] | Mapping[str, Runnable[Other, Any] | Callable[[Other], Any] | Any]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Other, Output]` | A new `Runnable`. |

#### ``pipe ¶

```
pipe(
    *others: Runnable[Any, Other] | Callable[[Any], Other], name: str | None = None
) -> RunnableSerializable[Input, Other]
```

Pipe `Runnable` objects.

Compose this `Runnable` with `Runnable`-like objects to make a
`RunnableSequence`.

Equivalent to `RunnableSequence(self, *others)` or `self | others[0] | ...`

Example

```
from langchain_core.runnables import RunnableLambda

def add_one(x: int) -> int:
    return x + 1

def mul_two(x: int) -> int:
    return x * 2

runnable_1 = RunnableLambda(add_one)
runnable_2 = RunnableLambda(mul_two)
sequence = runnable_1.pipe(runnable_2)
# Or equivalently:
# sequence = runnable_1 | runnable_2
# sequence = RunnableSequence(first=runnable_1, last=runnable_2)
sequence.invoke(1)
await sequence.ainvoke(1)
# -> 4

sequence.batch([1, 2, 3])
await sequence.abatch([1, 2, 3])
# -> [4, 6, 8]
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `*others` | Other `Runnable` or `Runnable`-like objects to compose<br>**TYPE:**`Runnable[Any, Other] | Callable[[Any], Other]`**DEFAULT:**`()` |
| `name` | An optional name for the resulting `RunnableSequence`.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Other]` | A new `Runnable`. |

#### ``pick ¶

```
pick(keys: str | list[str]) -> RunnableSerializable[Any, Any]
```

Pick keys from the output `dict` of this `Runnable`.

Pick a single key:

```
import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)
chain = RunnableMap(str=as_str, json=as_json)

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3]}

json_only_chain = chain.pick("json")
json_only_chain.invoke("[1, 2, 3]")
# -> [1, 2, 3]
```

Pick a list of keys:

```
from typing import Any

import json

from langchain_core.runnables import RunnableLambda, RunnableMap

as_str = RunnableLambda(str)
as_json = RunnableLambda(json.loads)

def as_bytes(x: Any) -> bytes:
    return bytes(x, "utf-8")

chain = RunnableMap(str=as_str, json=as_json, bytes=RunnableLambda(as_bytes))

chain.invoke("[1, 2, 3]")
# -> {"str": "[1, 2, 3]", "json": [1, 2, 3], "bytes": b"[1, 2, 3]"}

json_and_bytes_chain = chain.pick(["json", "bytes"])
json_and_bytes_chain.invoke("[1, 2, 3]")
# -> {"json": [1, 2, 3], "bytes": b"[1, 2, 3]"}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `keys` | A key or list of keys to pick from the output dict.<br>**TYPE:**`str | list[str]` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | a new `Runnable`. |

#### ``assign ¶

```
assign(
    **kwargs: Runnable[dict[str, Any], Any]
    | Callable[[dict[str, Any]], Any]
    | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]],
) -> RunnableSerializable[Any, Any]
```

Assigns new fields to the `dict` output of this `Runnable`.

```
from langchain_core.language_models.fake import FakeStreamingListLLM
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import SystemMessagePromptTemplate
from langchain_core.runnables import Runnable
from operator import itemgetter

prompt = (
    SystemMessagePromptTemplate.from_template("You are a nice assistant.")
    + "{question}"
)
model = FakeStreamingListLLM(responses=["foo-lish"])

chain: Runnable = prompt | model | {"str": StrOutputParser()}

chain_with_assign = chain.assign(hello=itemgetter("str") | model)

print(chain_with_assign.input_schema.model_json_schema())
# {'title': 'PromptInput', 'type': 'object', 'properties':
{'question': {'title': 'Question', 'type': 'string'}}}
print(chain_with_assign.output_schema.model_json_schema())
# {'title': 'RunnableSequenceOutput', 'type': 'object', 'properties':
{'str': {'title': 'Str',
'type': 'string'}, 'hello': {'title': 'Hello', 'type': 'string'}}}
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | A mapping of keys to `Runnable` or `Runnable`-like objects<br>that will be invoked with the entire output dict of this `Runnable`.<br>**TYPE:**`Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any] | Mapping[str, Runnable[dict[str, Any], Any] | Callable[[dict[str, Any]], Any]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Any, Any]` | A new `Runnable`. |

#### ``invoke ¶

```
invoke(
    input: dict[str, Any], config: RunnableConfig | None = None, **kwargs: Any
) -> dict[str, Any]
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``ainvoke`async`¶

```
ainvoke(
    input: dict[str, Any], config: RunnableConfig | None = None, **kwargs: Any
) -> dict[str, Any]
```

Transform a single input into an output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``batch ¶

```
batch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs invoke in parallel using a thread pool executor.

The default implementation of batch works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| `config` | A config to use when invoking the `Runnable`. The config supports<br>standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work<br>to do in parallel, and other keys. Please refer to the<br>`RunnableConfig` for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``batch\_as\_completed ¶

```
batch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> Iterator[tuple[int, Output | Exception]]
```

Run `invoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `tuple[int, Output | Exception]` | Tuples of the index of the input and the output from the `Runnable`. |

#### ``abatch`async`¶

```
abatch(
    inputs: list[Input],
    config: RunnableConfig | list[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> list[Output]
```

Default implementation runs `ainvoke` in parallel using `asyncio.gather`.

The default implementation of `batch` works well for IO bound runnables.

Subclasses must override this method if they can batch more efficiently;
e.g., if the underlying `Runnable` uses an API which supports a batch mode.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`list[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | list[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[Output]` | A list of outputs from the `Runnable`. |

#### ``abatch\_as\_completed`async`¶

```
abatch_as_completed(
    inputs: Sequence[Input],
    config: RunnableConfig | Sequence[RunnableConfig] | None = None,
    *,
    return_exceptions: bool = False,
    **kwargs: Any | None,
) -> AsyncIterator[tuple[int, Output | Exception]]
```

Run `ainvoke` in parallel on a list of inputs.

Yields results as they complete.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | A list of inputs to the `Runnable`.<br>**TYPE:**`Sequence[Input]` |
| `config` | A config to use when invoking the `Runnable`.<br>The config supports standard keys like `'tags'`, `'metadata'` for<br>tracing purposes, `'max_concurrency'` for controlling how much work to<br>do in parallel, and other keys. Please refer to the `RunnableConfig`<br>for more details.<br>**TYPE:**`RunnableConfig | Sequence[RunnableConfig] | None`**DEFAULT:**`None` |
| `return_exceptions` | Whether to return exceptions instead of raising them.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[tuple[int, Output | Exception]]` | A tuple of the index of the input and the output from the `Runnable`. |

#### ``stream ¶

```
stream(
    input: Input, config: RunnableConfig | None = None, **kwargs: Any | None
) -> Iterator[Output]
```

Default implementation of `stream`, which calls `invoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``astream`async`¶

```
astream(
    input: Input, config: RunnableConfig | None = None, **kwargs: Any | None
) -> AsyncIterator[Output]
```

Default implementation of `astream`, which calls `ainvoke`.

Subclasses must override this method if they support streaming output.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Input` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``astream\_log`async`¶

```
astream_log(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    diff: bool = True,
    with_streamed_output_list: bool = True,
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]
```

Stream all output from a `Runnable`, as reported to the callback system.

This includes all inner runs of LLMs, Retrievers, Tools, etc.

Output is streamed as Log objects, which include a list of
Jsonpatch ops that describe how the state of the run has changed in each
step, and the final state of the run.

The Jsonpatch ops can be applied in order to construct state.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Any` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `diff` | Whether to yield diffs between each step or the current state.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `with_streamed_output_list` | Whether to yield the `streamed_output` list.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `include_names` | Only include logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_types` | Only include logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_tags` | Only include logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_names` | Exclude logs with these names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_types` | Exclude logs with these types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_tags` | Exclude logs with these tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[RunLogPatch] | AsyncIterator[RunLog]` | A `RunLogPatch` or `RunLog` object. |

#### ``astream\_events`async`¶

```
astream_events(
    input: Any,
    config: RunnableConfig | None = None,
    *,
    version: Literal["v1", "v2"] = "v2",
    include_names: Sequence[str] | None = None,
    include_types: Sequence[str] | None = None,
    include_tags: Sequence[str] | None = None,
    exclude_names: Sequence[str] | None = None,
    exclude_types: Sequence[str] | None = None,
    exclude_tags: Sequence[str] | None = None,
    **kwargs: Any,
) -> AsyncIterator[StreamEvent]
```

Generate a stream of events.

Use to create an iterator over `StreamEvent` that provide real-time information
about the progress of the `Runnable`, including `StreamEvent` from intermediate
results.

A `StreamEvent` is a dictionary with the following schema:

- `event`: Event names are of the format:
`on_[runnable_type]_(start|stream|end)`.
- `name`: The name of the `Runnable` that generated the event.
- `run_id`: Randomly generated ID associated with the given execution of the
`Runnable` that emitted the event. A child `Runnable` that gets invoked as
part of the execution of a parent `Runnable` is assigned its own unique ID.
- `parent_ids`: The IDs of the parent runnables that generated the event. The
root `Runnable` will have an empty list. The order of the parent IDs is from
the root to the immediate parent. Only available for v2 version of the API.
The v1 version of the API will return an empty list.
- `tags`: The tags of the `Runnable` that generated the event.
- `metadata`: The metadata of the `Runnable` that generated the event.
- `data`: The data associated with the event. The contents of this field
depend on the type of event. See the table below for more details.

Below is a table that illustrates some events that might be emitted by various
chains. Metadata fields have been omitted from the table for brevity.
Chain definitions have been included after the table.

Note

This reference table is for the v2 version of the schema.

| event | name | chunk | input | output |
| --- | --- | --- | --- | --- |
| `on_chat_model_start` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` |  |
| `on_chat_model_stream` | `'[model name]'` | `AIMessageChunk(content="hello")` |  |  |
| `on_chat_model_end` | `'[model name]'` |  | `{"messages": [[SystemMessage, HumanMessage]]}` | `AIMessageChunk(content="hello world")` |
| `on_llm_start` | `'[model name]'` |  | `{'input': 'hello'}` |  |
| `on_llm_stream` | `'[model name]'` | `'Hello'` |  |  |
| `on_llm_end` | `'[model name]'` |  | `'Hello human!'` |  |
| `on_chain_start` | `'format_docs'` |  |  |  |
| `on_chain_stream` | `'format_docs'` | `'hello world!, goodbye world!'` |  |  |
| `on_chain_end` | `'format_docs'` |  | `[Document(...)]` | `'hello world!, goodbye world!'` |
| `on_tool_start` | `'some_tool'` |  | `{"x": 1, "y": "2"}` |  |
| `on_tool_end` | `'some_tool'` |  |  | `{"x": 1, "y": "2"}` |
| `on_retriever_start` | `'[retriever name]'` |  | `{"query": "hello"}` |  |
| `on_retriever_end` | `'[retriever name]'` |  | `{"query": "hello"}` | `[Document(...), ..]` |
| `on_prompt_start` | `'[template_name]'` |  | `{"question": "hello"}` |  |
| `on_prompt_end` | `'[template_name]'` |  | `{"question": "hello"}` | `ChatPromptValue(messages: [SystemMessage, ...])` |

In addition to the standard events, users can also dispatch custom events (see example below).

Custom events will be only be surfaced with in the v2 version of the API!

A custom event has following format:

| Attribute | Type | Description |
| --- | --- | --- |
| `name` | `str` | A user defined name for the event. |
| `data` | `Any` | The data associated with the event. This can be anything, though we suggest making it JSON serializable. |

Here are declarations associated with the standard events shown above:

`format_docs`:

```
def format_docs(docs: list[Document]) -> str:
    '''Format the docs.'''
    return ", ".join([doc.page_content for doc in docs])

format_docs = RunnableLambda(format_docs)
```

`some_tool`:

```
@tool
def some_tool(x: int, y: str) -> dict:
    '''Some_tool.'''
    return {"x": x, "y": y}
```

`prompt`:

```
template = ChatPromptTemplate.from_messages(
    [\
        ("system", "You are Cat Agent 007"),\
        ("human", "{question}"),\
    ]
).with_config({"run_name": "my_template", "tags": ["my_template"]})
```

For instance:

```
from langchain_core.runnables import RunnableLambda

async def reverse(s: str) -> str:
    return s[::-1]

chain = RunnableLambda(func=reverse)

events = [event async for event in chain.astream_events("hello", version="v2")]

# Will produce the following events
# (run_id, and parent_ids has been omitted for brevity):
[\
    {\
        "data": {"input": "hello"},\
        "event": "on_chain_start",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"chunk": "olleh"},\
        "event": "on_chain_stream",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
    {\
        "data": {"output": "olleh"},\
        "event": "on_chain_end",\
        "metadata": {},\
        "name": "reverse",\
        "tags": [],\
    },\
]
```

Example: Dispatch Custom Event

```
from langchain_core.callbacks.manager import (
    adispatch_custom_event,
)
from langchain_core.runnables import RunnableLambda, RunnableConfig
import asyncio

async def slow_thing(some_input: str, config: RunnableConfig) -> str:
    """Do something that takes a long time."""
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 1 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    await adispatch_custom_event(
        "progress_event",
        {"message": "Finished step 2 of 3"},
        config=config # Must be included for python < 3.10
    )
    await asyncio.sleep(1) # Placeholder for some slow operation
    return "Done"

slow_thing = RunnableLambda(slow_thing)

async for event in slow_thing.astream_events("some_input", version="v2"):
    print(event)
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | The input to the `Runnable`.<br>**TYPE:**`Any` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `version` | The version of the schema to use either `'v2'` or `'v1'`.<br>Users should use `'v2'`.<br>`'v1'` is for backwards compatibility and will be deprecated<br>in `0.4.0`.<br>No default will be assigned until the API is stabilized.<br>custom events will only be surfaced in `'v2'`.<br>**TYPE:**`Literal['v1', 'v2']`**DEFAULT:**`'v2'` |
| `include_names` | Only include events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_types` | Only include events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `include_tags` | Only include events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_names` | Exclude events from `Runnable` objects with matching names.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_types` | Exclude events from `Runnable` objects with matching types.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `exclude_tags` | Exclude events from `Runnable` objects with matching tags.<br>**TYPE:**`Sequence[str] | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>These will be passed to `astream_log` as this implementation<br>of `astream_events` is built on top of `astream_log`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[StreamEvent]` | An async stream of `StreamEvent`. |

| RAISES | DESCRIPTION |
| --- | --- |
| `NotImplementedError` | If the version is not `'v1'` or `'v2'`. |

#### ``transform ¶

```
transform(
    input: Iterator[Input], config: RunnableConfig | None = None, **kwargs: Any | None
) -> Iterator[Output]
```

Transform inputs to outputs.

Default implementation of transform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | An iterator of inputs to the `Runnable`.<br>**TYPE:**`Iterator[Input]` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `Output` | The output of the `Runnable`. |

#### ``atransform`async`¶

```
atransform(
    input: AsyncIterator[Input],
    config: RunnableConfig | None = None,
    **kwargs: Any | None,
) -> AsyncIterator[Output]
```

Transform inputs to outputs.

Default implementation of atransform, which buffers input and calls `astream`.

Subclasses must override this method if they can start producing output while
input is still being generated.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input` | An async iterator of inputs to the `Runnable`.<br>**TYPE:**`AsyncIterator[Input]` |
| `config` | The config to use for the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any | None`**DEFAULT:**`{}` |

| YIELDS | DESCRIPTION |
| --- | --- |
| `AsyncIterator[Output]` | The output of the `Runnable`. |

#### ``bind ¶

```
bind(**kwargs: Any) -> Runnable[Input, Output]
```

Bind arguments to a `Runnable`, returning a new `Runnable`.

Useful when a `Runnable` in a chain requires an argument that is not
in the output of the previous `Runnable` or included in the user input.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | The arguments to bind to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the arguments bound. |

Example

```
from langchain_ollama import ChatOllama
from langchain_core.output_parsers import StrOutputParser

model = ChatOllama(model="llama3.1")

# Without bind
chain = model | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two three four five.'

# With bind
chain = model.bind(stop=["three"]) | StrOutputParser()

chain.invoke("Repeat quoted words exactly: 'One two three four five.'")
# Output is 'One two'
```

#### ``with\_config ¶

```
with_config(
    config: RunnableConfig | None = None, **kwargs: Any
) -> Runnable[Input, Output]
```

Bind config to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `config` | The config to bind to the `Runnable`.<br>**TYPE:**`RunnableConfig | None`**DEFAULT:**`None` |
| `**kwargs` | Additional keyword arguments to pass to the `Runnable`.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the config bound. |

#### ``with\_listeners ¶

```
with_listeners(
    *,
    on_start: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
    on_end: Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None = None,
    on_error: Callable[[Run], None]
    | Callable[[Run, RunnableConfig], None]
    | None = None,
) -> Runnable[Input, Output]
```

Bind lifecycle listeners to a `Runnable`, returning a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `on_start` | Called before the `Runnable` starts running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| `on_end` | Called after the `Runnable` finishes running, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |
| `on_error` | Called if the `Runnable` throws an error, with the `Run`<br>object.<br>**TYPE:**`Callable[[Run], None] | Callable[[Run, RunnableConfig], None] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda
from langchain_core.tracers.schemas import Run

import time

def test_runnable(time_to_sleep: int):
    time.sleep(time_to_sleep)

def fn_start(run_obj: Run):
    print("start_time:", run_obj.start_time)

def fn_end(run_obj: Run):
    print("end_time:", run_obj.end_time)

chain = RunnableLambda(test_runnable).with_listeners(
    on_start=fn_start, on_end=fn_end
)
chain.invoke(2)
```

#### ``with\_alisteners ¶

```
with_alisteners(
    *,
    on_start: AsyncListener | None = None,
    on_end: AsyncListener | None = None,
    on_error: AsyncListener | None = None,
) -> Runnable[Input, Output]
```

Bind async lifecycle listeners to a `Runnable`.

Returns a new `Runnable`.

The Run object contains information about the run, including its `id`,
`type`, `input`, `output`, `error`, `start_time`, `end_time`, and
any tags or metadata added to the run.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `on_start` | Called asynchronously before the `Runnable` starts running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| `on_end` | Called asynchronously after the `Runnable` finishes running,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |
| `on_error` | Called asynchronously if the `Runnable` throws an error,<br>with the `Run` object.<br>**TYPE:**`AsyncListener | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the listeners bound. |

Example

```
from langchain_core.runnables import RunnableLambda, Runnable
from datetime import datetime, timezone
import time
import asyncio

def format_t(timestamp: float) -> str:
    return datetime.fromtimestamp(timestamp, tz=timezone.utc).isoformat()

async def test_runnable(time_to_sleep: int):
    print(f"Runnable[{time_to_sleep}s]: starts at {format_t(time.time())}")
    await asyncio.sleep(time_to_sleep)
    print(f"Runnable[{time_to_sleep}s]: ends at {format_t(time.time())}")

async def fn_start(run_obj: Runnable):
    print(f"on start callback starts at {format_t(time.time())}")
    await asyncio.sleep(3)
    print(f"on start callback ends at {format_t(time.time())}")

async def fn_end(run_obj: Runnable):
    print(f"on end callback starts at {format_t(time.time())}")
    await asyncio.sleep(2)
    print(f"on end callback ends at {format_t(time.time())}")

runnable = RunnableLambda(test_runnable).with_alisteners(
    on_start=fn_start,
    on_end=fn_end
)
async def concurrent_runs():
    await asyncio.gather(runnable.ainvoke(2), runnable.ainvoke(3))

asyncio.run(concurrent_runs())
Result:
on start callback starts at 2025-03-01T07:05:22.875378+00:00
on start callback starts at 2025-03-01T07:05:22.875495+00:00
on start callback ends at 2025-03-01T07:05:25.878862+00:00
on start callback ends at 2025-03-01T07:05:25.878947+00:00
Runnable[2s]: starts at 2025-03-01T07:05:25.879392+00:00
Runnable[3s]: starts at 2025-03-01T07:05:25.879804+00:00
Runnable[2s]: ends at 2025-03-01T07:05:27.881998+00:00
on end callback starts at 2025-03-01T07:05:27.882360+00:00
Runnable[3s]: ends at 2025-03-01T07:05:28.881737+00:00
on end callback starts at 2025-03-01T07:05:28.882428+00:00
on end callback ends at 2025-03-01T07:05:29.883893+00:00
on end callback ends at 2025-03-01T07:05:30.884831+00:00
```

#### ``with\_types ¶

```
with_types(
    *, input_type: type[Input] | None = None, output_type: type[Output] | None = None
) -> Runnable[Input, Output]
```

Bind input and output types to a `Runnable`, returning a new `Runnable`.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `input_type` | The input type to bind to the `Runnable`.<br>**TYPE:**`type[Input] | None`**DEFAULT:**`None` |
| `output_type` | The output type to bind to the `Runnable`.<br>**TYPE:**`type[Output] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new `Runnable` with the types bound. |

#### ``with\_retry ¶

```
with_retry(
    *,
    retry_if_exception_type: tuple[type[BaseException], ...] = (Exception,),
    wait_exponential_jitter: bool = True,
    exponential_jitter_params: ExponentialJitterParams | None = None,
    stop_after_attempt: int = 3,
) -> Runnable[Input, Output]
```

Create a new `Runnable` that retries the original `Runnable` on exceptions.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `retry_if_exception_type` | A tuple of exception types to retry on.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `wait_exponential_jitter` | Whether to add jitter to the wait<br>time between retries.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `stop_after_attempt` | The maximum number of attempts to make before<br>giving up.<br>**TYPE:**`int`**DEFAULT:**`3` |
| `exponential_jitter_params` | Parameters for<br>`tenacity.wait_exponential_jitter`. Namely: `initial`, `max`,<br>`exp_base`, and `jitter` (all `float` values).<br>**TYPE:**`ExponentialJitterParams | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[Input, Output]` | A new Runnable that retries the original Runnable on exceptions. |

Example

```
from langchain_core.runnables import RunnableLambda

count = 0

def _lambda(x: int) -> None:
    global count
    count = count + 1
    if x == 1:
        raise ValueError("x is 1")
    else:
        pass

runnable = RunnableLambda(_lambda)
try:
    runnable.with_retry(
        stop_after_attempt=2,
        retry_if_exception_type=(ValueError,),
    ).invoke(1)
except ValueError:
    pass

assert count == 2
```

#### ``map ¶

```
map() -> Runnable[list[Input], list[Output]]
```

Return a new `Runnable` that maps a list of inputs to a list of outputs.

Calls `invoke` with each input.

| RETURNS | DESCRIPTION |
| --- | --- |
| `Runnable[list[Input], list[Output]]` | A new `Runnable` that maps a list of inputs to a list of outputs. |

Example

```
from langchain_core.runnables import RunnableLambda

def _lambda(x: int) -> int:
    return x + 1

runnable = RunnableLambda(_lambda)
print(runnable.map().invoke([1, 2, 3]))  # [2, 3, 4]
```

#### ``with\_fallbacks ¶

```
with_fallbacks(
    fallbacks: Sequence[Runnable[Input, Output]],
    *,
    exceptions_to_handle: tuple[type[BaseException], ...] = (Exception,),
    exception_key: str | None = None,
) -> RunnableWithFallbacks[Input, Output]
```

Add fallbacks to a `Runnable`, returning a new `Runnable`.

The new `Runnable` will try the original `Runnable`, and then each fallback
in order, upon failures.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fallbacks` | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| `exceptions_to_handle` | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `exception_key` | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

Example

```
from typing import Iterator

from langchain_core.runnables import RunnableGenerator

def _generate_immediate_error(input: Iterator) -> Iterator[str]:
    raise ValueError()
    yield ""

def _generate(input: Iterator) -> Iterator[str]:
    yield from "foo bar"

runnable = RunnableGenerator(_generate_immediate_error).with_fallbacks(
    [RunnableGenerator(_generate)]
)
print("".join(runnable.stream({})))  # foo bar
```

| PARAMETER | DESCRIPTION |
| --- | --- |
| `fallbacks` | A sequence of runnables to try if the original `Runnable`<br>fails.<br>**TYPE:**`Sequence[Runnable[Input, Output]]` |
| `exceptions_to_handle` | A tuple of exception types to handle.<br>**TYPE:**`tuple[type[BaseException], ...]`**DEFAULT:**`(Exception,)` |
| `exception_key` | If `string` is specified then handled exceptions will be<br>passed to fallbacks as part of the input under the specified key.<br>If `None`, exceptions will not be passed to fallbacks.<br>If used, the base `Runnable` and its fallbacks must accept a<br>dictionary as input.<br>**TYPE:**`str | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableWithFallbacks[Input, Output]` | A new `Runnable` that will try the original `Runnable`, and then each<br>Fallback in order, upon failures. |

#### ``as\_tool ¶

```
as_tool(
    args_schema: type[BaseModel] | None = None,
    *,
    name: str | None = None,
    description: str | None = None,
    arg_types: dict[str, type] | None = None,
) -> BaseTool
```

Create a `BaseTool` from a `Runnable`.

`as_tool` will instantiate a `BaseTool` with a name, description, and
`args_schema` from a `Runnable`. Where possible, schemas are inferred
from `runnable.get_input_schema`. Alternatively (e.g., if the
`Runnable` takes a dict as input and the specific dict keys are not typed),
the schema can be specified directly with `args_schema`. You can also
pass `arg_types` to just specify the required arguments and their types.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `args_schema` | The schema for the tool.<br>**TYPE:**`type[BaseModel] | None`**DEFAULT:**`None` |
| `name` | The name of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `description` | The description of the tool.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `arg_types` | A dictionary of argument names to types.<br>**TYPE:**`dict[str, type] | None`**DEFAULT:**`None` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `BaseTool` | A `BaseTool` instance. |

Typed dict input:

```
from typing_extensions import TypedDict
from langchain_core.runnables import RunnableLambda

class Args(TypedDict):
    a: int
    b: list[int]

def f(x: Args) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool()
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `args_schema`:

```
from typing import Any
from pydantic import BaseModel, Field
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

class FSchema(BaseModel):
    """Apply a function to an integer and list of integers."""

    a: int = Field(..., description="Integer")
    b: list[int] = Field(..., description="List of ints")

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(FSchema)
as_tool.invoke({"a": 3, "b": [1, 2]})
```

`dict` input, specifying schema via `arg_types`:

```
from typing import Any
from langchain_core.runnables import RunnableLambda

def f(x: dict[str, Any]) -> str:
    return str(x["a"] * max(x["b"]))

runnable = RunnableLambda(f)
as_tool = runnable.as_tool(arg_types={"a": int, "b": list[int]})
as_tool.invoke({"a": 3, "b": [1, 2]})
```

String input:

```
from langchain_core.runnables import RunnableLambda

def f(x: str) -> str:
    return x + "a"

def g(x: str) -> str:
    return x + "z"

runnable = RunnableLambda(f) | g
as_tool = runnable.as_tool()
as_tool.invoke("b")
```

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the `Runnable` to JSON.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON-serializable representation of the `Runnable`. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``configurable\_fields ¶

```
configurable_fields(
    **kwargs: AnyConfigurableField,
) -> RunnableSerializable[Input, Output]
```

Configure particular `Runnable` fields at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | A dictionary of `ConfigurableField` instances to configure.<br>**TYPE:**`AnyConfigurableField`**DEFAULT:**`{}` |

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If a configuration key is not found in the `Runnable`. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the fields configured. |

```
from langchain_core.runnables import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatOpenAI(max_tokens=20).configurable_fields(
    max_tokens=ConfigurableField(
        id="output_token_number",
        name="Max tokens in the output",
        description="The maximum number of tokens in the output",
    )
)

# max_tokens = 20
print("max_tokens_20: ", model.invoke("tell me something about chess").content)

# max_tokens = 200
print(
    "max_tokens_200: ",
    model.with_config(configurable={"output_token_number": 200})
    .invoke("tell me something about chess")
    .content,
)
```

#### ``configurable\_alternatives ¶

```
configurable_alternatives(
    which: ConfigurableField,
    *,
    default_key: str = "default",
    prefix_keys: bool = False,
    **kwargs: Runnable[Input, Output] | Callable[[], Runnable[Input, Output]],
) -> RunnableSerializable[Input, Output]
```

Configure alternatives for `Runnable` objects that can be set at runtime.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `which` | The `ConfigurableField` instance that will be used to select the<br>alternative.<br>**TYPE:**`ConfigurableField` |
| `default_key` | The default key to use if no alternative is selected.<br>**TYPE:**`str`**DEFAULT:**`'default'` |
| `prefix_keys` | Whether to prefix the keys with the `ConfigurableField` id.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `**kwargs` | A dictionary of keys to `Runnable` instances or callables that<br>return `Runnable` instances.<br>**TYPE:**`Runnable[Input, Output] | Callable[[], Runnable[Input, Output]]`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunnableSerializable[Input, Output]` | A new `Runnable` with the alternatives configured. |

```
from langchain_anthropic import ChatAnthropic
from langchain_core.runnables.utils import ConfigurableField
from langchain_openai import ChatOpenAI

model = ChatAnthropic(
    model_name="claude-sonnet-4-5-20250929"
).configurable_alternatives(
    ConfigurableField(id="llm"),
    default_key="anthropic",
    openai=ChatOpenAI(),
)

# uses the default model ChatAnthropic
print(model.invoke("which organization created you?").content)

# uses ChatOpenAI
print(
    model.with_config(configurable={"llm": "openai"})
    .invoke("which organization created you?")
    .content
)
```

#### ``raise\_callback\_manager\_deprecation`classmethod`¶

```
raise_callback_manager_deprecation(values: dict) -> Any
```

Raise deprecation warning if callback\_manager is used.

#### ``set\_verbose`classmethod`¶

```
set_verbose(verbose: bool | None) -> bool
```

Set the chain verbosity.

Defaults to the global setting if not specified by the user.

#### ``\_\_call\_\_ ¶

```
__call__(
    inputs: dict[str, Any] | Any,
    return_only_outputs: bool = False,
    callbacks: Callbacks = None,
    *,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    run_name: str | None = None,
    include_run_info: bool = False,
) -> dict[str, Any]
```

Execute the chain.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | Dictionary of inputs, or single input if chain expects<br>only one param. Should contain all inputs specified in<br>`Chain.input_keys` except for inputs that will be set by the chain's<br>memory.<br>**TYPE:**`dict[str, Any] | Any` |
| `return_only_outputs` | Whether to return only outputs in the<br>response. If `True`, only new keys generated by this chain will be<br>returned. If `False`, both input keys and new keys generated by this<br>chain will be returned.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `callbacks` | Callbacks to use for this chain run. These will be called in<br>addition to callbacks passed to the chain during construction, but only<br>these runtime callbacks will propagate to calls to other objects.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `tags` | List of string tags to pass to all callbacks. These will be passed in<br>addition to tags passed to the chain during construction, but only<br>these runtime tags will propagate to calls to other objects.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | Optional metadata associated with the chain.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `run_name` | Optional name for this run of the chain.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `include_run_info` | Whether to include run info in the response. Defaults<br>to False.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A dict of named outputs. Should contain all outputs specified in<br>`Chain.output_keys`. |

#### ``acall`async`¶

```
acall(
    inputs: dict[str, Any] | Any,
    return_only_outputs: bool = False,
    callbacks: Callbacks = None,
    *,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    run_name: str | None = None,
    include_run_info: bool = False,
) -> dict[str, Any]
```

Asynchronously execute the chain.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | Dictionary of inputs, or single input if chain expects<br>only one param. Should contain all inputs specified in<br>`Chain.input_keys` except for inputs that will be set by the chain's<br>memory.<br>**TYPE:**`dict[str, Any] | Any` |
| `return_only_outputs` | Whether to return only outputs in the<br>response. If `True`, only new keys generated by this chain will be<br>returned. If `False`, both input keys and new keys generated by this<br>chain will be returned.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `callbacks` | Callbacks to use for this chain run. These will be called in<br>addition to callbacks passed to the chain during construction, but only<br>these runtime callbacks will propagate to calls to other objects.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `tags` | List of string tags to pass to all callbacks. These will be passed in<br>addition to tags passed to the chain during construction, but only<br>these runtime tags will propagate to calls to other objects.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | Optional metadata associated with the chain.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `run_name` | Optional name for this run of the chain.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `include_run_info` | Whether to include run info in the response. Defaults<br>to False.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, Any]` | A dict of named outputs. Should contain all outputs specified in<br>`Chain.output_keys`. |

#### ``prep\_outputs ¶

```
prep_outputs(
    inputs: dict[str, str], outputs: dict[str, str], return_only_outputs: bool = False
) -> dict[str, str]
```

Validate and prepare chain outputs, and save info about this run to memory.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | Dictionary of chain inputs, including any inputs added by chain<br>memory.<br>**TYPE:**`dict[str, str]` |
| `outputs` | Dictionary of initial chain outputs.<br>**TYPE:**`dict[str, str]` |
| `return_only_outputs` | Whether to only return the chain outputs. If `False`,<br>inputs are also added to the final outputs.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, str]` | A dict of the final chain outputs. |

#### ``aprep\_outputs`async`¶

```
aprep_outputs(
    inputs: dict[str, str], outputs: dict[str, str], return_only_outputs: bool = False
) -> dict[str, str]
```

Validate and prepare chain outputs, and save info about this run to memory.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | Dictionary of chain inputs, including any inputs added by chain<br>memory.<br>**TYPE:**`dict[str, str]` |
| `outputs` | Dictionary of initial chain outputs.<br>**TYPE:**`dict[str, str]` |
| `return_only_outputs` | Whether to only return the chain outputs. If `False`,<br>inputs are also added to the final outputs.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, str]` | A dict of the final chain outputs. |

#### ``prep\_inputs ¶

```
prep_inputs(inputs: dict[str, Any] | Any) -> dict[str, str]
```

Prepare chain inputs, including adding inputs from memory.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | Dictionary of raw inputs, or single input if chain expects<br>only one param. Should contain all inputs specified in<br>`Chain.input_keys` except for inputs that will be set by the chain's<br>memory.<br>**TYPE:**`dict[str, Any] | Any` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, str]` | A dictionary of all inputs, including those added by the chain's memory. |

#### ``aprep\_inputs`async`¶

```
aprep_inputs(inputs: dict[str, Any] | Any) -> dict[str, str]
```

Prepare chain inputs, including adding inputs from memory.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `inputs` | Dictionary of raw inputs, or single input if chain expects<br>only one param. Should contain all inputs specified in<br>`Chain.input_keys` except for inputs that will be set by the chain's<br>memory.<br>**TYPE:**`dict[str, Any] | Any` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict[str, str]` | A dictionary of all inputs, including those added by the chain's memory. |

#### ``run ¶

```
run(
    *args: Any,
    callbacks: Callbacks = None,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    **kwargs: Any,
) -> Any
```

Convenience method for executing chain.

The main difference between this method and `Chain.__call__` is that this
method expects inputs to be passed directly in as positional arguments or
keyword arguments, whereas `Chain.__call__` expects a single input dictionary
with all the inputs

| PARAMETER | DESCRIPTION |
| --- | --- |
| `*args` | If the chain expects a single input, it can be passed in as the<br>sole positional argument.<br>**TYPE:**`Any`**DEFAULT:**`()` |
| `callbacks` | Callbacks to use for this chain run. These will be called in<br>addition to callbacks passed to the chain during construction, but only<br>these runtime callbacks will propagate to calls to other objects.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `tags` | List of string tags to pass to all callbacks. These will be passed in<br>addition to tags passed to the chain during construction, but only<br>these runtime tags will propagate to calls to other objects.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | Optional metadata associated with the chain.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `**kwargs` | If the chain expects multiple inputs, they can be passed in<br>directly as keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Any` | The chain output. |

Example

```
# Suppose we have a single-input chain that takes a 'question' string:
chain.run("What's the temperature in Boise, Idaho?")
# -> "The temperature in Boise is..."

# Suppose we have a multi-input chain that takes a 'question' string
# and 'context' string:
question = "What's the temperature in Boise, Idaho?"
context = "Weather report for Boise, Idaho on 07/03/23..."
chain.run(question=question, context=context)
# -> "The temperature in Boise is..."
```

#### ``arun`async`¶

```
arun(
    *args: Any,
    callbacks: Callbacks = None,
    tags: list[str] | None = None,
    metadata: dict[str, Any] | None = None,
    **kwargs: Any,
) -> Any
```

Convenience method for executing chain.

The main difference between this method and `Chain.__call__` is that this
method expects inputs to be passed directly in as positional arguments or
keyword arguments, whereas `Chain.__call__` expects a single input dictionary
with all the inputs

| PARAMETER | DESCRIPTION |
| --- | --- |
| `*args` | If the chain expects a single input, it can be passed in as the<br>sole positional argument.<br>**TYPE:**`Any`**DEFAULT:**`()` |
| `callbacks` | Callbacks to use for this chain run. These will be called in<br>addition to callbacks passed to the chain during construction, but only<br>these runtime callbacks will propagate to calls to other objects.<br>**TYPE:**`Callbacks`**DEFAULT:**`None` |
| `tags` | List of string tags to pass to all callbacks. These will be passed in<br>addition to tags passed to the chain during construction, but only<br>these runtime tags will propagate to calls to other objects.<br>**TYPE:**`list[str] | None`**DEFAULT:**`None` |
| `metadata` | Optional metadata associated with the chain.<br>**TYPE:**`dict[str, Any] | None`**DEFAULT:**`None` |
| `**kwargs` | If the chain expects multiple inputs, they can be passed in<br>directly as keyword arguments.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `Any` | The chain output. |

Example

```
# Suppose we have a single-input chain that takes a 'question' string:
await chain.arun("What's the temperature in Boise, Idaho?")
# -> "The temperature in Boise is..."

# Suppose we have a multi-input chain that takes a 'question' string
# and 'context' string:
question = "What's the temperature in Boise, Idaho?"
context = "Weather report for Boise, Idaho on 07/03/23..."
await chain.arun(question=question, context=context)
# -> "The temperature in Boise is..."
```

#### ``dict ¶

```
dict(**kwargs: Any) -> dict
```

Dictionary representation of chain.

Expects `Chain._chain_type` property to be implemented and for memory to be
null.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `**kwargs` | Keyword arguments passed to default `pydantic.BaseModel.dict`<br>method.<br>**TYPE:**`Any`**DEFAULT:**`{}` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `dict` | A dictionary representation of the chain. |

Example

```
chain.model_dump(exclude_unset=True)
# -> {"_type": "foo", "verbose": False, ...}
```

#### ``save ¶

```
save(file_path: Path | str) -> None
```

Save the chain.

Expects `Chain._chain_type` property to be implemented and for memory to be
null.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `file_path` | Path to file to save the chain to.<br>**TYPE:**`Path | str` |

Example

```
chain.save(file_path="path/chain.yaml")
```

#### ``apply ¶

```
apply(
    input_list: list[dict[str, Any]], callbacks: Callbacks = None
) -> list[dict[str, str]]
```

Call the chain on all inputs in the list.

### ``StringRunMapper ¶

Bases: `Serializable`

Extract items to evaluate from the run object.

| METHOD | DESCRIPTION |
| --- | --- |
| `map` | Maps the Run to a dictionary. |
| `__call__` | Maps the Run to a dictionary. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the object to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |

#### ``output\_keys`property`¶

```
output_keys: list[str]
```

The keys to extract from the run.

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``map`abstractmethod`¶

```
map(run: Run) -> dict[str, str]
```

Maps the Run to a dictionary.

#### ``\_\_call\_\_ ¶

```
__call__(run: Run) -> dict[str, str]
```

Maps the Run to a dictionary.

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the object to JSON.

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the class has deprecated attributes. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON serializable object or a `SerializedNotImplemented` object. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

### ``ToolStringRunMapper ¶

Bases: `StringRunMapper`

Map an input to the tool.

| METHOD | DESCRIPTION |
| --- | --- |
| `map` | Maps the Run to a dictionary. |
| `__init__` |  |
| `is_lc_serializable` | Is this class serializable? |
| `get_lc_namespace` | Get the namespace of the LangChain object. |
| `lc_id` | Return a unique identifier for this class for serialization purposes. |
| `to_json` | Serialize the object to JSON. |
| `to_json_not_implemented` | Serialize a "not implemented" object. |
| `__call__` | Maps the Run to a dictionary. |

#### ``lc\_secrets`property`¶

```
lc_secrets: dict[str, str]
```

A map of constructor argument names to secret ids.

For example, `{"openai_api_key": "OPENAI_API_KEY"}`

#### ``lc\_attributes`property`¶

```
lc_attributes: dict
```

List of attribute names that should be included in the serialized kwargs.

These attributes must be accepted by the constructor.

Default is an empty dictionary.

#### ``output\_keys`property`¶

```
output_keys: list[str]
```

The keys to extract from the run.

#### ``map ¶

```
map(run: Run) -> dict[str, str]
```

Maps the Run to a dictionary.

#### ``\_\_init\_\_ ¶

```
__init__(*args: Any, **kwargs: Any) -> None
```

#### ``is\_lc\_serializable`classmethod`¶

```
is_lc_serializable() -> bool
```

Is this class serializable?

By design, even if a class inherits from `Serializable`, it is not serializable
by default. This is to prevent accidental serialization of objects that should
not be serialized.

| RETURNS | DESCRIPTION |
| --- | --- |
| `bool` | Whether the class is serializable. Default is `False`. |

#### ``get\_lc\_namespace`classmethod`¶

```
get_lc_namespace() -> list[str]
```

Get the namespace of the LangChain object.

For example, if the class is `langchain.llms.openai.OpenAI`, then the
namespace is `["langchain", "llms", "openai"]`

| RETURNS | DESCRIPTION |
| --- | --- |
| `list[str]` | The namespace. |

#### ``lc\_id`classmethod`¶

```
lc_id() -> list[str]
```

Return a unique identifier for this class for serialization purposes.

The unique identifier is a list of strings that describes the path
to the object.

For example, for the class `langchain.llms.openai.OpenAI`, the id is
`["langchain", "llms", "openai", "OpenAI"]`.

#### ``to\_json ¶

```
to_json() -> SerializedConstructor | SerializedNotImplemented
```

Serialize the object to JSON.

| RAISES | DESCRIPTION |
| --- | --- |
| `ValueError` | If the class has deprecated attributes. |

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedConstructor | SerializedNotImplemented` | A JSON serializable object or a `SerializedNotImplemented` object. |

#### ``to\_json\_not\_implemented ¶

```
to_json_not_implemented() -> SerializedNotImplemented
```

Serialize a "not implemented" object.

| RETURNS | DESCRIPTION |
| --- | --- |
| `SerializedNotImplemented` | `SerializedNotImplemented`. |

#### ``\_\_call\_\_ ¶

```
__call__(run: Run) -> dict[str, str]
```

Maps the Run to a dictionary.

## ``langchain\_classic.smith.evaluation.name\_generation ¶

| FUNCTION | DESCRIPTION |
| --- | --- |
| `random_name` | Generate a random name. |

### ``random\_name ¶

```
random_name() -> str
```

Generate a random name.

Back to top