## Details

| Developed by | Guardrails AI |
| --- | --- |
| Date of development | Feb 15, 2024 |
| Validator type | Chatbots, QA |
| Blog |  |
| License | Apache 2 |
| Input/Output | Output |

## Description

## Example Usage Guide

### Installation

```bash
$ gudardrails hub install on_topic
```

### Initialization

```python
on_topic_validator = OnTopic(
	on_fail="noop",
	valid_topics="Customer support"
)

guard = Guard.from_string(
    validators=[on_topic_validator, ...],
)
```

### Invocation

```python
guard(
    "LLM response text",
)
```

## API Ref

- `valid_topics` *List[str]* - topics that the text should be about (one or many).
- `invalid_topics` *List[str], Optional, defaults to []* - topics that the text cannot be about.
- `device` *int, Optional, defaults to -1* - Device ordinal for CPU/GPU supports for Zero-Shot classifier. Setting this to -1 will leverage CPU, a positive will run the Zero-Shot model on the associated CUDA device id.
- `model` *str, Optional, defaults to 'facebook/bart-large-mnli'* - The Zero-Shot model that will be used to classify the topic. See a list of all models here: https://huggingface.co/models?pipeline_tag=zero-shot-classification llm_callable (Union[str, Callable, None], Optional, defaults to
- `'gpt-3.5-turbo')` - Either the name of the OpenAI model, or a callable that takes a prompt and returns a response.
- `disable_classifier` *bool, Optional, defaults to False* - controls whether to use the Zero-Shot model. At least one of disable_classifier and disable_llm must be False.
- `disable_llm` *bool, Optional, defaults to False* - controls whether to use the LLM fallback. At least one of disable_classifier and disable_llm must be False.
- `model_threshold` *float, Optional, defaults to 0.5* - The threshold used to determine whether to accept a topic from the Zero-Shot model. Must be a number between 0 and 1.

## Intended use

- Primary intended uses: This validator is intended to be used when you want to constrain an LLM to only respond to certain topics.
- Out-of-scope use cases: n/a

## Expected deployment metrics

|  | CPU | GPU |
| --- | --- | --- |
| Latency |  | - |
| Memory |  | - |
| Cost |  | - |
| Expected quality |  | - |

## Resources required

- Dependencies: `transformers`
- Foundation model access keys: n/a
- Compute: Yes

## Validator Performance

### Evaluation Dataset

### Model Performance Measures

| Accuracy |  |
| --- | --- |
| F1 Score |  |

### Decision thresholds
