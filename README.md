## Overview

| Developed by | Guardrails AI |
| --- | --- |
| Date of development | Feb 15, 2024 |
| Validator type | Format |
| Blog |  |
| License | Apache 2 |
| Input/Output | Output |

## Description

The validator ensures that a generated output is a single line (i.e. sentence only). It is a posthoc validator, i.e. it is applied on the output of the LLM response.

## Installation

```bash
$ guardrails hub install hub://guardrails/one_line
```

## Usage Examples

### Validating string output via Python

In this example, we’ll test that a generated LLM sentence is a single line.

```python
# Import Guard and Validator
from guardrails.hub import ValidChoices
from guardrails import Guard

# Initialize Validator
val = OneLine(on_fail="fix")

# Setup Guard
guard = Guard.from_string(
    validators=[val, ...],
)

guard.parse("Guardrails are essential for AI dev.")  # Validator passes
guard.parse(
    "Guardrails are essential for AI dev. "
    "You can initialize guadrails for strings, JSON objects and via python and javascript."
)  # Validator fails
```

### Validating JSON output via Python

In this example, we verify that a summary of a product contains a single line.

```python
# Import Guard and Validator
from pydantic import BaseModel
from guardrails.hub import ValidChoices
from guardrails import Guard

# Initialize Validator
val = OneLine(on_fail="fix")

# Create Pydantic BaseModel
class ProductInfo(BaseModel):
    product_name: str = Field(description="Name of the product")
    product_summary: str = Field(
        description="A one line summary of the product", validators=[val]
    )

# Create a Guard to check for valid Pydantic output
guard = Guard.from_pydantic(output_class=ProductInfo)

# Run LLM output generating JSON through guard
guard.parse("""
{
    "product_name": "Hairspray",
    "product_summary": "This product helps your styled hair stay in place."
}
""")
```


## API Reference

**`__init__(self, on_fail="noop")`**
<ul>

Initializes a new instance of the Validator class.

**Parameters:**

- **`on_fail`** *(str, Callable):* The policy to enact when a validator fails. If `str`, must be one of `reask`, `fix`, `filter`, `refrain`, `noop`, `exception` or `fix_reask`. Otherwise, must be a function that is called when the validator fails.

</ul>

<br>

**`__call__(self, value, metadata={}) → ValidationOutcome`**

<ul>

Validates the given `value` using the rules defined in this validator, relying on the `metadata` provided to customize the validation process. This method is automatically invoked by `guard.parse(...)`, ensuring the validation logic is applied to the input data.

Note:

1. This method should not be called directly by the user. Instead, invoke `guard.parse(...)` where this method will be called internally for each associated Validator.
2. When invoking `guard.parse(...)`, ensure to pass the appropriate `metadata` dictionary that includes keys and values required by this validator. If `guard` is associated with multiple validators, combine all necessary metadata into a single dictionary.

**Parameters:**

- **`value`** *(Any):* The input value to validate.
- **`metadata`** *(dict):* A dictionary containing metadata required for validation. No additional metadata keys are needed for this validator.

</ul>
