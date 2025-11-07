<!-- URL: page_161 -->

Skip to content

Edit this page

# Evaluation

## ``langsmith.evaluation ¶

Evaluation Helpers.

| FUNCTION | DESCRIPTION |
| --- | --- |
| `aevaluate` | Evaluate an async target system on a given dataset. |
| `aevaluate_existing` | Evaluate existing experiment runs asynchronously. |
| `evaluate` | Evaluate a target system on a given dataset. |
| `evaluate_comparative` | Evaluate existing experiment runs against each other. |
| `evaluate_existing` | Evaluate existing experiment runs. |
| `run_evaluator` | Create a run evaluator from a function. |

### ``EvaluationResult ¶

Bases: `BaseModel`

Evaluation result.

| METHOD | DESCRIPTION |
| --- | --- |
| `check_value_non_numeric` | Check that the value is not numeric. |

#### ``key`instance-attribute`¶

```
key: str
```

The aspect, metric name, or label for this evaluation.

#### ``score`class-attribute``instance-attribute`¶

```
score: SCORE_TYPE = None
```

The numeric score for this evaluation.

#### ``value`class-attribute``instance-attribute`¶

```
value: VALUE_TYPE = None
```

The value for this evaluation, if not numeric.

#### ``comment`class-attribute``instance-attribute`¶

```
comment: str | None = None
```

An explanation regarding the evaluation.

#### ``correction`class-attribute``instance-attribute`¶

```
correction: dict | None = None
```

What the correct value should be, if applicable.

#### ``evaluator\_info`class-attribute``instance-attribute`¶

```
evaluator_info: dict = Field(default_factory=dict)
```

Additional information about the evaluator.

#### ``feedback\_config`class-attribute``instance-attribute`¶

```
feedback_config: FeedbackConfig | dict | None = None
```

The configuration used to generate this feedback.

#### ``source\_run\_id`class-attribute``instance-attribute`¶

```
source_run_id: UUID | str | None = None
```

The ID of the trace of the evaluator itself.

#### ``target\_run\_id`class-attribute``instance-attribute`¶

```
target_run_id: UUID | str | None = None
```

The ID of the trace this evaluation is applied to.

If none provided, the evaluation feedback is applied to the
root trace being.

#### ``extra`class-attribute``instance-attribute`¶

```
extra: dict | None = None
```

Metadata for the evaluator run.

#### ``Config ¶

Pydantic model configuration.

#### ``check\_value\_non\_numeric ¶

```
check_value_non_numeric(v, values)
```

Check that the value is not numeric.

### ``EvaluationResults ¶

Bases: `TypedDict`

Batch evaluation results.

This makes it easy for your evaluator to return multiple
metrics at once.

#### ``results`instance-attribute`¶

```
results: list[EvaluationResult]
```

The evaluation results.

### ``RunEvaluator ¶

Evaluator interface class.

| METHOD | DESCRIPTION |
| --- | --- |
| `evaluate_run` | Evaluate an example. |
| `aevaluate_run` | Evaluate an example asynchronously. |

#### ``evaluate\_run`abstractmethod`¶

```
evaluate_run(
    run: Run, example: Example | None = None, evaluator_run_id: UUID | None = None
) -> EvaluationResult | EvaluationResults
```

Evaluate an example.

#### ``aevaluate\_run`async`¶

```
aevaluate_run(
    run: Run, example: Example | None = None, evaluator_run_id: UUID | None = None
) -> EvaluationResult | EvaluationResults
```

Evaluate an example asynchronously.

### ``LangChainStringEvaluator ¶

A class for wrapping a LangChain StringEvaluator.

Requires the `langchain` package to be installed.

| ATTRIBUTE | DESCRIPTION |
| --- | --- |
| `evaluator` | The underlying StringEvaluator OR the name<br>of the evaluator to load.<br>**TYPE:**`StringEvaluator` |

| METHOD | DESCRIPTION |
| --- | --- |
| `as_run_evaluator` | Convert the LangChainStringEvaluator to a RunEvaluator. |

Examples:

Creating a simple LangChainStringEvaluator:

```
>>> evaluator = LangChainStringEvaluator("exact_match")
```

Converting a LangChainStringEvaluator to a RunEvaluator:

```
>>> from langsmith.evaluation import LangChainStringEvaluator
>>> from langchain_openai import ChatOpenAI
>>> evaluator = LangChainStringEvaluator(
...     "criteria",
...     config={
...         "criteria": {
...             "usefulness": "The prediction is useful if"
...             " it is correct and/or asks a useful followup question."
...         },
...         "llm": ChatOpenAI(model="gpt-4o"),
...     },
... )
>>> run_evaluator = evaluator.as_run_evaluator()
>>> run_evaluator
<DynamicRunEvaluator ...>
```

Customizing the LLM model used by the evaluator:

```
>>> from langsmith.evaluation import LangChainStringEvaluator
>>> from langchain_anthropic import ChatAnthropic
>>> evaluator = LangChainStringEvaluator(
...     "criteria",
...     config={
...         "criteria": {
...             "usefulness": "The prediction is useful if"
...             " it is correct and/or asks a useful followup question."
...         },
...         "llm": ChatAnthropic(model="claude-3-opus-20240229"),
...     },
... )
>>> run_evaluator = evaluator.as_run_evaluator()
>>> run_evaluator
<DynamicRunEvaluator ...>
```

Using the `evaluate` API with different evaluators:

```
>>> def prepare_data(run: Run, example: Example):
...     # Convert the evaluation data into the format expected by the evaluator
...     # Only required for datasets with multiple inputs/output keys
...     return {
...         "prediction": run.outputs["prediction"],
...         "reference": example.outputs["answer"],
...         "input": str(example.inputs),
...     }
>>> import re
>>> from langchain_anthropic import ChatAnthropic
>>> import langsmith
>>> from langsmith.evaluation import LangChainStringEvaluator, evaluate
>>> criteria_evaluator = LangChainStringEvaluator(
...     "criteria",
...     config={
...         "criteria": {
...             "usefulness": "The prediction is useful if it is correct"
...             " and/or asks a useful followup question."
...         },
...         "llm": ChatAnthropic(model="claude-3-opus-20240229"),
...     },
...     prepare_data=prepare_data,
... )
>>> embedding_evaluator = LangChainStringEvaluator("embedding_distance")
>>> exact_match_evaluator = LangChainStringEvaluator("exact_match")
>>> regex_match_evaluator = LangChainStringEvaluator(
...     "regex_match", config={"flags": re.IGNORECASE}, prepare_data=prepare_data
... )
>>> scoring_evaluator = LangChainStringEvaluator(
...     "labeled_score_string",
...     config={
...         "criteria": {
...             "accuracy": "Score 1: Completely inaccurate\nScore 5: Somewhat accurate\nScore 10: Completely accurate"
...         },
...         "normalize_by": 10,
...         "llm": ChatAnthropic(model="claude-3-opus-20240229"),
...     },
...     prepare_data=prepare_data,
... )
>>> string_distance_evaluator = LangChainStringEvaluator(
...     "string_distance",
...     config={"distance_metric": "levenshtein"},
...     prepare_data=prepare_data,
... )
>>> from langsmith import Client
>>> client = Client()
>>> results = evaluate(
...     lambda inputs: {"prediction": "foo"},
...     data=client.list_examples(dataset_name="Evaluate Examples", limit=1),
...     evaluators=[\
...         embedding_evaluator,\
...         criteria_evaluator,\
...         exact_match_evaluator,\
...         regex_match_evaluator,\
...         scoring_evaluator,\
...         string_distance_evaluator,\
...     ],
... )
View the evaluation results for experiment:...
```

#### ``\_\_init\_\_ ¶

```
__init__(
    evaluator: StringEvaluator | str,
    *,
    config: dict | None = None,
    prepare_data: Callable[[Run, Optional[Example]], SingleEvaluatorInput]
    | None = None,
)
```

Initialize a LangChainStringEvaluator.

See: https://api.python.langchain.com/en/latest/evaluation/langchain.evaluation.schema.StringEvaluator.html#langchain-evaluation-schema-stringevaluator

| PARAMETER | DESCRIPTION |
| --- | --- |
| `evaluator` | The underlying StringEvaluator.<br>**TYPE:**`StringEvaluator` |

#### ``as\_run\_evaluator ¶

```
as_run_evaluator() -> RunEvaluator
```

Convert the LangChainStringEvaluator to a RunEvaluator.

This is the object used in the LangSmith `evaluate` API.

| RETURNS | DESCRIPTION |
| --- | --- |
| `RunEvaluator` | The converted RunEvaluator.<br>**TYPE:**`RunEvaluator` |

### ``aevaluate`async`¶

```
aevaluate(
    target: ATARGET_T | AsyncIterable[dict] | Runnable | str | UUID | TracerSession,
    /,
    data: DATA_T | AsyncIterable[Example] | Iterable[Example] | None = None,
    evaluators: Sequence[EVALUATOR_T | AEVALUATOR_T] | None = None,
    summary_evaluators: Sequence[SUMMARY_EVALUATOR_T] | None = None,
    metadata: dict | None = None,
    experiment_prefix: str | None = None,
    description: str | None = None,
    max_concurrency: int | None = 0,
    num_repetitions: int = 1,
    client: Client | None = None,
    blocking: bool = True,
    experiment: TracerSession | str | UUID | None = None,
    upload_results: bool = True,
    error_handling: Literal["log", "ignore"] = "log",
    **kwargs: Any,
) -> AsyncExperimentResults
```

Evaluate an async target system on a given dataset.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `target` | The target system or experiment(s) to evaluate. Can be an async function<br>that takes a dict and returns a dict, a langchain Runnable, an<br>existing experiment ID, or a two-tuple of experiment IDs.<br>**TYPE:**`AsyncCallable[[dict], dict] | AsyncIterable[dict] | Runnable | EXPERIMENT_T | Tuple[EXPERIMENT_T, EXPERIMENT_T]` |
| `data` | The dataset to evaluate on. Can be a dataset name, a list of<br>examples, an async generator of examples, or an async iterable of examples.<br>**TYPE:**`DATA_T | AsyncIterable[Example]`**DEFAULT:**`None` |
| `evaluators` | A list of evaluators to run<br>on each example. Defaults to None.<br>**TYPE:**`Sequence[EVALUATOR_T] | None`**DEFAULT:**`None` |
| `summary_evaluators` | A list of summary<br>evaluators to run on the entire dataset. Defaults to None.<br>**TYPE:**`Sequence[SUMMARY_EVALUATOR_T] | None`**DEFAULT:**`None` |
| `metadata` | Metadata to attach to the experiment.<br>Defaults to None.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |
| `experiment_prefix` | A prefix to provide for your experiment name.<br>Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `description` | A description of the experiment.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `max_concurrency` | The maximum number of concurrent<br>evaluations to run. If None then no limit is set. If 0 then no concurrency.<br>Defaults to 0.<br>**TYPE:**`int | None`**DEFAULT:**`0` |
| `num_repetitions` | The number of times to run the evaluation.<br>Each item in the dataset will be run and evaluated this many times.<br>Defaults to 1.<br>**TYPE:**`int`**DEFAULT:**`1` |
| `client` | The LangSmith client to use.<br>Defaults to None.<br>**TYPE:**`Client | None`**DEFAULT:**`None` |
| `blocking` | Whether to block until the evaluation is complete.<br>Defaults to True.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `experiment` | An existing experiment to<br>extend. If provided, experiment\_prefix is ignored. For advanced<br>usage only.<br>**TYPE:**`TracerSession | None`**DEFAULT:**`None` |
| `load_nested` | Whether to load all child runs for the experiment.<br>Default is to only load the top-level root runs. Should only be specified<br>when evaluating an existing experiment. |
| `error_handling` | How to handle individual run errors. 'log'<br>will trace the runs with the error message as part of the experiment,<br>'ignore' will not count the run as part of the experiment at all.<br>**TYPE:**`str, default="log"`**DEFAULT:**`'log'` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AsyncExperimentResults` | AsyncIterator\[ExperimentResultRow\]: An async iterator over the experiment results. |

Environment

- LANGSMITH\_TEST\_CACHE: If set, API calls will be cached to disk to save time and
cost during testing. Recommended to commit the cache files to your repository
for faster CI/CD runs.
Requires the 'langsmith\[vcr\]' package to be installed.

Examples:

```
>>> from typing import Sequence
>>> from langsmith import Client, aevaluate
>>> from langsmith.schemas import Example, Run
>>> client = Client()
>>> dataset = client.clone_public_dataset(
...     "https://smith.langchain.com/public/419dcab2-1d66-4b94-8901-0357ead390df/d"
... )
>>> dataset_name = "Evaluate Examples"
```

Basic usage:

```
>>> def accuracy(run: Run, example: Example):
...     # Row-level evaluator for accuracy.
...     pred = run.outputs["output"]
...     expected = example.outputs["answer"]
...     return {"score": expected.lower() == pred.lower()}
```

```
>>> def precision(runs: Sequence[Run], examples: Sequence[Example]):
...     # Experiment-level evaluator for precision.
...     # TP / (TP + FP)
...     predictions = [run.outputs["output"].lower() for run in runs]
...     expected = [example.outputs["answer"].lower() for example in examples]
...     # yes and no are the only possible answers
...     tp = sum([p == e for p, e in zip(predictions, expected) if p == "yes"])
...     fp = sum([p == "yes" and e == "no" for p, e in zip(predictions, expected)])
...     return {"score": tp / (tp + fp)}
```

```
>>> import asyncio
>>> async def apredict(inputs: dict) -> dict:
...     # This can be any async function or just an API call to your app.
...     await asyncio.sleep(0.1)
...     return {"output": "Yes"}
>>> results = asyncio.run(
...     aevaluate(
...         apredict,
...         data=dataset_name,
...         evaluators=[accuracy],
...         summary_evaluators=[precision],
...         experiment_prefix="My Experiment",
...         description="Evaluate the accuracy of the model asynchronously.",
...         metadata={
...             "my-prompt-version": "abcd-1234",
...         },
...     )
... )
View the evaluation results for experiment:...
```

Evaluating over only a subset of the examples using an async generator:

```
>>> async def example_generator():
...     examples = client.list_examples(dataset_name=dataset_name, limit=5)
...     for example in examples:
...         yield example
>>> results = asyncio.run(
...     aevaluate(
...         apredict,
...         data=example_generator(),
...         evaluators=[accuracy],
...         summary_evaluators=[precision],
...         experiment_prefix="My Subset Experiment",
...         description="Evaluate a subset of examples asynchronously.",
...     )
... )
View the evaluation results for experiment:...
```

Streaming each prediction to more easily + eagerly debug.

```
>>> results = asyncio.run(
...     aevaluate(
...         apredict,
...         data=dataset_name,
...         evaluators=[accuracy],
...         summary_evaluators=[precision],
...         experiment_prefix="My Streaming Experiment",
...         description="Streaming predictions for debugging.",
...         blocking=False,
...     )
... )
View the evaluation results for experiment:...
```

```
>>> async def aenumerate(iterable):
...     async for elem in iterable:
...         print(elem)
>>> asyncio.run(aenumerate(results))
```

Running without concurrency:

```
>>> results = asyncio.run(
...     aevaluate(
...         apredict,
...         data=dataset_name,
...         evaluators=[accuracy],
...         summary_evaluators=[precision],
...         experiment_prefix="My Experiment Without Concurrency",
...         description="This was run without concurrency.",
...         max_concurrency=0,
...     )
... )
View the evaluation results for experiment:...
```

Using Async evaluators:

```
>>> async def helpfulness(run: Run, example: Example):
...     # Row-level evaluator for helpfulness.
...     await asyncio.sleep(5)  # Replace with your LLM API call
...     return {"score": run.outputs["output"] == "Yes"}
```

```
>>> results = asyncio.run(
...     aevaluate(
...         apredict,
...         data=dataset_name,
...         evaluators=[helpfulness],
...         summary_evaluators=[precision],
...         experiment_prefix="My Helpful Experiment",
...         description="Applying async evaluators example.",
...     )
... )
View the evaluation results for experiment:...
```

.. versionchanged:: 0.2.0

```
'max_concurrency' default updated from None (no limit on concurrency)
to 0 (no concurrency at all).
```

### ``aevaluate\_existing`async`¶

```
aevaluate_existing(
    experiment: str | UUID | TracerSession,
    /,
    evaluators: Sequence[EVALUATOR_T | AEVALUATOR_T] | None = None,
    summary_evaluators: Sequence[SUMMARY_EVALUATOR_T] | None = None,
    metadata: dict | None = None,
    max_concurrency: int | None = 0,
    client: Client | None = None,
    load_nested: bool = False,
    blocking: bool = True,
) -> AsyncExperimentResults
```

Evaluate existing experiment runs asynchronously.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `experiment` | The identifier of the experiment to evaluate.<br>**TYPE:**`str | UUID` |
| `evaluators` | Optional sequence of evaluators to use for individual run evaluation.<br>**TYPE:**`Sequence[EVALUATOR_T] | None`**DEFAULT:**`None` |
| `summary_evaluators` | Optional sequence of evaluators<br>to apply over the entire dataset.<br>**TYPE:**`Sequence[SUMMARY_EVALUATOR_T] | None`**DEFAULT:**`None` |
| `metadata` | Optional metadata to include in the evaluation results.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |
| `max_concurrency` | The maximum number of concurrent<br>evaluations to run. If None then no limit is set. If 0 then no concurrency.<br>Defaults to 0.<br>**TYPE:**`int | None`**DEFAULT:**`0` |
| `client` | Optional Langsmith client to use for evaluation.<br>**TYPE:**`Client | None`**DEFAULT:**`None` |
| `load_nested` | Whether to load all child runs for the experiment.<br>Default is to only load the top-level root runs.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `blocking` | Whether to block until evaluation is complete.<br>**TYPE:**`bool`**DEFAULT:**`True` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `AsyncExperimentResults` | AsyncIterator\[ExperimentResultRow\]: An async iterator over the experiment results. |

Examples:

Define your evaluators

```
>>> from typing import Sequence
>>> from langsmith.schemas import Example, Run
>>> def accuracy(run: Run, example: Example):
...     # Row-level evaluator for accuracy.
...     pred = run.outputs["output"]
...     expected = example.outputs["answer"]
...     return {"score": expected.lower() == pred.lower()}
>>> def precision(runs: Sequence[Run], examples: Sequence[Example]):
...     # Experiment-level evaluator for precision.
...     # TP / (TP + FP)
...     predictions = [run.outputs["output"].lower() for run in runs]
...     expected = [example.outputs["answer"].lower() for example in examples]
...     # yes and no are the only possible answers
...     tp = sum([p == e for p, e in zip(predictions, expected) if p == "yes"])
...     fp = sum([p == "yes" and e == "no" for p, e in zip(predictions, expected)])
...     return {"score": tp / (tp + fp)}
```

Load the experiment and run the evaluation.

```
>>> import asyncio
>>> import uuid
>>> from langsmith import Client, aevaluate, aevaluate_existing
>>> client = Client()
>>> dataset_name = "__doctest_aevaluate_existing_" + uuid.uuid4().hex[:8]
>>> dataset = client.create_dataset(dataset_name)
>>> example = client.create_example(
...     inputs={"question": "What is 2+2?"},
...     outputs={"answer": "4"},
...     dataset_id=dataset.id,
... )
>>> async def apredict(inputs: dict) -> dict:
...     await asyncio.sleep(0.001)
...     return {"output": "4"}
>>> results = asyncio.run(
...     aevaluate(
...         apredict, data=dataset_name, experiment_prefix="doctest_experiment"
...     )
... )
View the evaluation results for experiment:...
>>> experiment_id = results.experiment_name
>>> # Consume all results to ensure evaluation is complete
>>> async def consume_results():
...     result_list = [r async for r in results]
...     return len(result_list) > 0
>>> asyncio.run(consume_results())
True
>>> import time
>>> time.sleep(3)
>>> results = asyncio.run(
...     aevaluate_existing(
...         experiment_id,
...         evaluators=[accuracy],
...         summary_evaluators=[precision],
...     )
... )
View the evaluation results for experiment:...
>>> client.delete_dataset(dataset_id=dataset.id)
```

### ``evaluate ¶

```
evaluate(
    target: TARGET_T | Runnable | EXPERIMENT_T | tuple[EXPERIMENT_T, EXPERIMENT_T],
    /,
    data: DATA_T | None = None,
    evaluators: Sequence[EVALUATOR_T] | Sequence[COMPARATIVE_EVALUATOR_T] | None = None,
    summary_evaluators: Sequence[SUMMARY_EVALUATOR_T] | None = None,
    metadata: dict | None = None,
    experiment_prefix: str | None = None,
    description: str | None = None,
    max_concurrency: int | None = 0,
    num_repetitions: int = 1,
    client: Client | None = None,
    blocking: bool = True,
    experiment: EXPERIMENT_T | None = None,
    upload_results: bool = True,
    error_handling: Literal["log", "ignore"] = "log",
    **kwargs: Any,
) -> ExperimentResults | ComparativeExperimentResults
```

Evaluate a target system on a given dataset.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `target` | The target system or experiment(s) to evaluate. Can be a function<br>that takes a dict and returns a dict, a langchain Runnable, an<br>existing experiment ID, or a two-tuple of experiment IDs.<br>**TYPE:**`TARGET_T | Runnable | EXPERIMENT_T | Tuple[EXPERIMENT_T, EXPERIMENT_T]` |
| `data` | The dataset to evaluate on. Can be a dataset name, a list of<br>examples, or a generator of examples.<br>**TYPE:**`DATA_T`**DEFAULT:**`None` |
| `evaluators` | A list of evaluators to run on each example. The evaluator signature<br>depends on the target type. Default to None.<br>**TYPE:**`Sequence[EVALUATOR_T] | Sequence[COMPARATIVE_EVALUATOR_T] | None`**DEFAULT:**`None` |
| `summary_evaluators` | A list of summary<br>evaluators to run on the entire dataset. Should not be specified if<br>comparing two existing experiments. Defaults to None.<br>**TYPE:**`Sequence[SUMMARY_EVALUATOR_T] | None`**DEFAULT:**`None` |
| `metadata` | Metadata to attach to the experiment.<br>Defaults to None.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |
| `experiment_prefix` | A prefix to provide for your experiment name.<br>Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `description` | A free-form text description for the experiment.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `max_concurrency` | The maximum number of concurrent<br>evaluations to run. If None then no limit is set. If 0 then no concurrency.<br>Defaults to 0.<br>**TYPE:**`int | None`**DEFAULT:**`0` |
| `client` | The LangSmith client to use.<br>Defaults to None.<br>**TYPE:**`Client | None`**DEFAULT:**`None` |
| `blocking` | Whether to block until the evaluation is complete.<br>Defaults to True.<br>**TYPE:**`bool`**DEFAULT:**`True` |
| `num_repetitions` | The number of times to run the evaluation.<br>Each item in the dataset will be run and evaluated this many times.<br>Defaults to 1.<br>**TYPE:**`int`**DEFAULT:**`1` |
| `experiment` | An existing experiment to<br>extend. If provided, experiment\_prefix is ignored. For advanced<br>usage only. Should not be specified if target is an existing experiment or<br>two-tuple fo experiments.<br>**TYPE:**`TracerSession | None`**DEFAULT:**`None` |
| `load_nested` | Whether to load all child runs for the experiment.<br>Default is to only load the top-level root runs. Should only be specified<br>when target is an existing experiment or two-tuple of experiments.<br>**TYPE:**`bool` |
| `randomize_order` | Whether to randomize the order of the outputs for each<br>evaluation. Default is False. Should only be specified when target is a<br>two-tuple of existing experiments.<br>**TYPE:**`bool` |
| `error_handling` | How to handle individual run errors. 'log'<br>will trace the runs with the error message as part of the experiment,<br>'ignore' will not count the run as part of the experiment at all.<br>**TYPE:**`str, default="log"`**DEFAULT:**`'log'` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ExperimentResults` | If target is a function, Runnable, or existing experiment.<br>**TYPE:**`ExperimentResults | ComparativeExperimentResults` |
| `ComparativeExperimentResults` | If target is a two-tuple of existing experiments.<br>**TYPE:**`ExperimentResults | ComparativeExperimentResults` |

Examples:

Prepare the dataset:

```
>>> from typing import Sequence
>>> from langsmith import Client
>>> from langsmith.evaluation import evaluate
>>> from langsmith.schemas import Example, Run
>>> client = Client()
>>> dataset = client.clone_public_dataset(
...     "https://smith.langchain.com/public/419dcab2-1d66-4b94-8901-0357ead390df/d"
... )
>>> dataset_name = "Evaluate Examples"
```

Basic usage:

```
>>> def accuracy(run: Run, example: Example):
...     # Row-level evaluator for accuracy.
...     pred = run.outputs["output"]
...     expected = example.outputs["answer"]
...     return {"score": expected.lower() == pred.lower()}
>>> def precision(runs: Sequence[Run], examples: Sequence[Example]):
...     # Experiment-level evaluator for precision.
...     # TP / (TP + FP)
...     predictions = [run.outputs["output"].lower() for run in runs]
...     expected = [example.outputs["answer"].lower() for example in examples]
...     # yes and no are the only possible answers
...     tp = sum([p == e for p, e in zip(predictions, expected) if p == "yes"])
...     fp = sum([p == "yes" and e == "no" for p, e in zip(predictions, expected)])
...     return {"score": tp / (tp + fp)}
>>> def predict(inputs: dict) -> dict:
...     # This can be any function or just an API call to your app.
...     return {"output": "Yes"}
>>> results = evaluate(
...     predict,
...     data=dataset_name,
...     evaluators=[accuracy],
...     summary_evaluators=[precision],
...     experiment_prefix="My Experiment",
...     description="Evaluating the accuracy of a simple prediction model.",
...     metadata={
...         "my-prompt-version": "abcd-1234",
...     },
... )
View the evaluation results for experiment:...
```

Evaluating over only a subset of the examples

```
>>> experiment_name = results.experiment_name
>>> examples = client.list_examples(dataset_name=dataset_name, limit=5)
>>> results = evaluate(
...     predict,
...     data=examples,
...     evaluators=[accuracy],
...     summary_evaluators=[precision],
...     experiment_prefix="My Experiment",
...     description="Just testing a subset synchronously.",
... )
View the evaluation results for experiment:...
```

Streaming each prediction to more easily + eagerly debug.

```
>>> results = evaluate(
...     predict,
...     data=dataset_name,
...     evaluators=[accuracy],
...     summary_evaluators=[precision],
...     description="I don't even have to block!",
...     blocking=False,
... )
View the evaluation results for experiment:...
>>> for i, result in enumerate(results):
...     pass
```

Using the `evaluate` API with an off-the-shelf LangChain evaluator:

```
>>> from langsmith.evaluation import LangChainStringEvaluator
>>> from langchain_openai import ChatOpenAI
>>> def prepare_criteria_data(run: Run, example: Example):
...     return {
...         "prediction": run.outputs["output"],
...         "reference": example.outputs["answer"],
...         "input": str(example.inputs),
...     }
>>> results = evaluate(
...     predict,
...     data=dataset_name,
...     evaluators=[\
...         accuracy,\
...         LangChainStringEvaluator("embedding_distance"),\
...         LangChainStringEvaluator(\
...             "labeled_criteria",\
...             config={\
...                 "criteria": {\
...                     "usefulness": "The prediction is useful if it is correct"\
...                     " and/or asks a useful followup question."\
...                 },\
...                 "llm": ChatOpenAI(model="gpt-4o"),\
...             },\
...             prepare_data=prepare_criteria_data,\
...         ),\
...     ],
...     description="Evaluating with off-the-shelf LangChain evaluators.",
...     summary_evaluators=[precision],
... )
View the evaluation results for experiment:...
```

Evaluating a LangChain object:

```
>>> from langchain_core.runnables import chain as as_runnable
>>> @as_runnable
... def nested_predict(inputs):
...     return {"output": "Yes"}
>>> @as_runnable
... def lc_predict(inputs):
...     return nested_predict.invoke(inputs)
>>> results = evaluate(
...     lc_predict.invoke,
...     data=dataset_name,
...     evaluators=[accuracy],
...     description="This time we're evaluating a LangChain object.",
...     summary_evaluators=[precision],
... )
View the evaluation results for experiment:...
```

.. versionchanged:: 0.2.0

```
'max_concurrency' default updated from None (no limit on concurrency)
to 0 (no concurrency at all).
```

### ``evaluate\_comparative ¶

```
evaluate_comparative(
    experiments: tuple[EXPERIMENT_T, EXPERIMENT_T],
    /,
    evaluators: Sequence[COMPARATIVE_EVALUATOR_T],
    experiment_prefix: str | None = None,
    description: str | None = None,
    max_concurrency: int = 5,
    client: Client | None = None,
    metadata: dict | None = None,
    load_nested: bool = False,
    randomize_order: bool = False,
) -> ComparativeExperimentResults
```

Evaluate existing experiment runs against each other.

This lets you use pairwise preference scoring to generate more
reliable feedback in your experiments.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `experiments` | The identifiers of the experiments to compare.<br>**TYPE:**`Tuple[str | UUID, str | UUID]` |
| `evaluators` | A list of evaluators to run on each example.<br>**TYPE:**`Sequence[COMPARATIVE_EVALUATOR_T]` |
| `experiment_prefix` | A prefix to provide for your experiment name.<br>Defaults to None.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `description` | A free-form text description for the experiment.<br>**TYPE:**`str | None`**DEFAULT:**`None` |
| `max_concurrency` | The maximum number of concurrent evaluations to run.<br>Defaults to 5.<br>**TYPE:**`int`**DEFAULT:**`5` |
| `client` | The LangSmith client to use.<br>Defaults to None.<br>**TYPE:**`Client | None`**DEFAULT:**`None` |
| `metadata` | Metadata to attach to the experiment.<br>Defaults to None.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |
| `load_nested` | Whether to load all child runs for the experiment.<br>Default is to only load the top-level root runs.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `randomize_order` | Whether to randomize the order of the outputs for each evaluation.<br>Default is False.<br>**TYPE:**`bool`**DEFAULT:**`False` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ComparativeExperimentResults` | The results of the comparative evaluation.<br>**TYPE:**`ComparativeExperimentResults` |

Examples:

Suppose you want to compare two prompts to see which one is more effective.
You would first prepare your dataset:

```
>>> from typing import Sequence
>>> from langsmith import Client
>>> from langsmith.evaluation import evaluate
>>> from langsmith.schemas import Example, Run
>>> client = Client()
>>> dataset = client.clone_public_dataset(
...     "https://smith.langchain.com/public/419dcab2-1d66-4b94-8901-0357ead390df/d"
... )
>>> dataset_name = "Evaluate Examples"
```

Then you would run your different prompts:

```
>>> import functools
>>> import openai
>>> from langsmith.evaluation import evaluate
>>> from langsmith.wrappers import wrap_openai
>>> oai_client = openai.Client()
>>> wrapped_client = wrap_openai(oai_client)
>>> prompt_1 = "You are a helpful assistant."
>>> prompt_2 = "You are an exceedingly helpful assistant."
>>> def predict(inputs: dict, prompt: str) -> dict:
...     completion = wrapped_client.chat.completions.create(
...         model="gpt-4o-mini",
...         messages=[\
...             {"role": "system", "content": prompt},\
...             {\
...                 "role": "user",\
...                 "content": f"Context: {inputs['context']}"\
...                 f"\n\ninputs['question']",\
...             },\
...         ],
...     )
...     return {"output": completion.choices[0].message.content}
>>> results_1 = evaluate(
...     functools.partial(predict, prompt=prompt_1),
...     data=dataset_name,
...     description="Evaluating our basic system prompt.",
...     blocking=False,  # Run these experiments in parallel
... )
View the evaluation results for experiment:...
>>> results_2 = evaluate(
...     functools.partial(predict, prompt=prompt_2),
...     data=dataset_name,
...     description="Evaluating our advanced system prompt.",
...     blocking=False,
... )
View the evaluation results for experiment:...
>>> results_1.wait()
>>> results_2.wait()
```

```
Finally, you would compare the two prompts directly:
```

```
>>> import json
>>> from langsmith.evaluation import evaluate_comparative
>>> from langsmith import schemas
>>> def score_preferences(runs: list, example: schemas.Example):
...     assert len(runs) == 2  # Comparing 2 systems
...     assert isinstance(example, schemas.Example)
...     assert all(run.reference_example_id == example.id for run in runs)
...     pred_a = runs[0].outputs["output"] if runs[0].outputs else ""
...     pred_b = runs[1].outputs["output"] if runs[1].outputs else ""
...     ground_truth = example.outputs["answer"] if example.outputs else ""
...     tools = [\
...         {\
...             "type": "function",\
...             "function": {\
...                 "name": "rank_preferences",\
...                 "description": "Saves the prefered response ('A' or 'B')",\
...                 "parameters": {\
...                     "type": "object",\
...                     "properties": {\
...                         "reasoning": {\
...                             "type": "string",\
...                             "description": "The reasoning behind the choice.",\
...                         },\
...                         "preferred_option": {\
...                             "type": "string",\
...                             "enum": ["A", "B"],\
...                             "description": "The preferred option, either 'A' or 'B'",\
...                         },\
...                     },\
...                     "required": ["preferred_option"],\
...                 },\
...             },\
...         }\
...     ]
...     completion = openai.Client().chat.completions.create(
...         model="gpt-4o-mini",
...         messages=[\
...             {"role": "system", "content": "Select the better response."},\
...             {\
...                 "role": "user",\
...                 "content": f"Option A: {pred_a}"\
...                 f"\n\nOption B: {pred_b}"\
...                 f"\n\nGround Truth: {ground_truth}",\
...             },\
...         ],
...         tools=tools,
...         tool_choice={
...             "type": "function",
...             "function": {"name": "rank_preferences"},
...         },
...     )
...     tool_args = completion.choices[0].message.tool_calls[0].function.arguments
...     loaded_args = json.loads(tool_args)
...     preference = loaded_args["preferred_option"]
...     comment = loaded_args["reasoning"]
...     if preference == "A":
...         return {
...             "key": "ranked_preference",
...             "scores": {runs[0].id: 1, runs[1].id: 0},
...             "comment": comment,
...         }
...     else:
...         return {
...             "key": "ranked_preference",
...             "scores": {runs[0].id: 0, runs[1].id: 1},
...             "comment": comment,
...         }
>>> def score_length_difference(runs: list, example: schemas.Example):
...     # Just return whichever response is longer.
...     # Just an example, not actually useful in real life.
...     assert len(runs) == 2  # Comparing 2 systems
...     assert isinstance(example, schemas.Example)
...     assert all(run.reference_example_id == example.id for run in runs)
...     pred_a = runs[0].outputs["output"] if runs[0].outputs else ""
...     pred_b = runs[1].outputs["output"] if runs[1].outputs else ""
...     if len(pred_a) > len(pred_b):
...         return {
...             "key": "length_difference",
...             "scores": {runs[0].id: 1, runs[1].id: 0},
...         }
...     else:
...         return {
...             "key": "length_difference",
...             "scores": {runs[0].id: 0, runs[1].id: 1},
...         }
>>> results = evaluate_comparative(
...     [results_1.experiment_name, results_2.experiment_name],
...     evaluators=[score_preferences, score_length_difference],
...     client=client,
... )
View the pairwise evaluation results at:...
>>> eval_results = list(results)
>>> assert len(eval_results) >= 10
>>> assert all(
...     "feedback.ranked_preference" in r["evaluation_results"]
...     for r in eval_results
... )
>>> assert all(
...     "feedback.length_difference" in r["evaluation_results"]
...     for r in eval_results
... )
```

### ``evaluate\_existing ¶

```
evaluate_existing(
    experiment: str | UUID | TracerSession,
    /,
    evaluators: Sequence[EVALUATOR_T] | None = None,
    summary_evaluators: Sequence[SUMMARY_EVALUATOR_T] | None = None,
    metadata: dict | None = None,
    max_concurrency: int | None = 0,
    client: Client | None = None,
    load_nested: bool = False,
    blocking: bool = True,
) -> ExperimentResults
```

Evaluate existing experiment runs.

| PARAMETER | DESCRIPTION |
| --- | --- |
| `experiment` | The identifier of the experiment to evaluate.<br>**TYPE:**`str | UUID` |
| `data` | The data to use for evaluation.<br>**TYPE:**`DATA_T` |
| `evaluators` | Optional sequence of evaluators to use for individual run evaluation.<br>**TYPE:**`Sequence[EVALUATOR_T] | None`**DEFAULT:**`None` |
| `summary_evaluators` | Optional sequence of evaluators<br>to apply over the entire dataset.<br>**TYPE:**`Sequence[SUMMARY_EVALUATOR_T] | None`**DEFAULT:**`None` |
| `metadata` | Optional metadata to include in the evaluation results.<br>**TYPE:**`dict | None`**DEFAULT:**`None` |
| `max_concurrency` | The maximum number of concurrent<br>evaluations to run. If None then no limit is set. If 0 then no concurrency.<br>Defaults to 0.<br>**TYPE:**`int | None`**DEFAULT:**`0` |
| `client` | Optional Langsmith client to use for evaluation.<br>**TYPE:**`Client | None`**DEFAULT:**`None` |
| `load_nested` | Whether to load all child runs for the experiment.<br>Default is to only load the top-level root runs.<br>**TYPE:**`bool`**DEFAULT:**`False` |
| `blocking` | Whether to block until evaluation is complete.<br>**TYPE:**`bool`**DEFAULT:**`True` |

| RETURNS | DESCRIPTION |
| --- | --- |
| `ExperimentResults` | The evaluation results.<br>**TYPE:**`ExperimentResults` |

Environment

- LANGSMITH\_TEST\_CACHE: If set, API calls will be cached to disk to save time and
cost during testing. Recommended to commit the cache files to your repository
for faster CI/CD runs.
Requires the 'langsmith\[vcr\]' package to be installed.

Examples:

Define your evaluators

```
>>> from typing import Sequence
>>> from langsmith.schemas import Example, Run
>>> def accuracy(run: Run, example: Example):
...     # Row-level evaluator for accuracy.
...     pred = run.outputs["output"]
...     expected = example.outputs["answer"]
...     return {"score": expected.lower() == pred.lower()}
>>> def precision(runs: Sequence[Run], examples: Sequence[Example]):
...     # Experiment-level evaluator for precision.
...     # TP / (TP + FP)
...     predictions = [run.outputs["output"].lower() for run in runs]
...     expected = [example.outputs["answer"].lower() for example in examples]
...     # yes and no are the only possible answers
...     tp = sum([p == e for p, e in zip(predictions, expected) if p == "yes"])
...     fp = sum([p == "yes" and e == "no" for p, e in zip(predictions, expected)])
...     return {"score": tp / (tp + fp)}
```

Load the experiment and run the evaluation.

```
>>> import uuid
>>> from langsmith import Client
>>> from langsmith.evaluation import evaluate, evaluate_existing
>>> client = Client()
>>> dataset_name = "__doctest_evaluate_existing_" + uuid.uuid4().hex[:8]
>>> dataset = client.create_dataset(dataset_name)
>>> example = client.create_example(
...     inputs={"question": "What is 2+2?"},
...     outputs={"answer": "4"},
...     dataset_id=dataset.id,
... )
>>> def predict(inputs: dict) -> dict:
...     return {"output": "4"}
>>> # First run inference on the dataset
... results = evaluate(
...     predict, data=dataset_name, experiment_prefix="doctest_experiment"
... )
View the evaluation results for experiment:...
>>> experiment_id = results.experiment_name
>>> # Wait for the experiment to be fully processed and check if we have results
>>> len(results) > 0
True
>>> import time
>>> time.sleep(2)
>>> results = evaluate_existing(
...     experiment_id,
...     evaluators=[accuracy],
...     summary_evaluators=[precision],
... )
View the evaluation results for experiment:...
>>> client.delete_dataset(dataset_id=dataset.id)
```

### ``run\_evaluator ¶

```
run_evaluator(
    func: Callable[\
        [Run, Optional[Example]], _RUNNABLE_OUTPUT | Awaitable[_RUNNABLE_OUTPUT]\
    ],
)
```

Create a run evaluator from a function.

Decorator that transforms a function into a `RunEvaluator`.

Back to top