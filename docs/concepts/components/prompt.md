# Prompt Object


Prompts in Ragas are used inside various metrics and synthetic data generation tasks. In each of these tasks, Ragas also provides a way for the user to modify or replace the default prompt with a custom prompt. This guide provides an overview of the Prompt Object in Ragas. 


## Components of a Prompt Object

In Ragas, a prompt object is composed of the following key components:

1. **Instruction**: A fundamental element of any prompt, the instruction is a natural language directive that clearly describes the task the Language Model (LLM) should perform. This is specified using the `instruction` variable within the prompt object.

2. **Few-Shot Examples**: LLMs are known to perform better when provided with few-shot examples, as they help the model understand the task context and generate more accurate responses. These examples are specified using the `examples` variable in the prompt object. Each example consists of an input and its corresponding output, which the LLM uses to learn the task.

3. **Input Model**: Every prompt expects an input to produce an output. In Ragas, the expected format of this input is defined using the `input_model` variable. This is a Pydantic model that outlines the structure of the input, enabling validation and parsing of the data provided to the prompt.

4. **Output Model**: Upon execution, a prompt generates an output. The format of this output is specified using the `output_model` variable in the prompt object. Like the input model, the output model is a Pydantic model that defines the structure of the output, facilitating validation and parsing of the data produced by the LLM.


## Example

Here's an example of a prompt object that defines a prompt for a text generation task:

```python
from ragas.experimental.prompt import PydanticPrompt
from pydantic import BaseModel, Field

class MyInput(BaseModel):
    question: str = Field(description="The question to answer")

class MyOutput(BaseModel):
    answer: str = Field(description="The answer to the question")

class MyPrompt(PydanticPrompt[MyInput,MyInput]):
    instruction = "Answer the given question"
    input_model = MyInput
    output_model = MyOutput
    examples = [
        (
            MyInput(question="Who's building the opensource standard for LLM app evals?"),
            MyOutput(answer="Ragas")
        )
    ]
    
```

## Guidelines for Creating Effective Prompts

When creating prompts in Ragas, consider the following guidelines to ensure that your prompts are effective and aligned with the task requirements:

1. **Clear and Concise Instructions**: Provide clear and concise instructions that clearly define the task the LLM should perform. Ambiguity in instructions can lead to inaccurate responses.
2. **Relevant Few-Shot Examples**: Include relevant few-shot examples that cover a diverse range of scenarios related to the task (ideally 3-5). These examples help the LLM understand the context and generate accurate responses.
3. **Simple Input and Output Models**: Define simple and intuitive input and output models that accurately represent the data format expected by the LLM and the output generated by the LLM. If the models are complex, try to break the task into smaller sub-tasks with separate prompts.