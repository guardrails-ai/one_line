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

`__init__`

- `on_fail`: The policy to enact when a validator fails.
