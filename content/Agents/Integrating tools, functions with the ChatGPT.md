---
tags:
  - developing
date: 2024-12-24
backlinks: "[[Memory]]"
---
### Define Functions
Ensure each function has a precise signature and a comprehensive docstring describing its purpose and parameters.

```python
def add(x: float, y: float) -> float:
    """Add two numbers."""
    return x + y
```

### Convert Functions to JSON Schemas
Utilize Python’s inspect module to extract function signatures and convert them into JSON schemas compatible with the ChatGPT API.

```python
import inspect

def function_to_schema(func):
    """Convert a Python function to an OpenAI function schema."""
    signature = inspect.signature(func)
    parameters = {
        name: {"type": "number" if param.annotation in [int, float] else "string"}
        for name, param in signature.parameters.items()
    }
    return {
        "name": func.__name__,
        "description": func.__doc__,
        "parameters": {
            "type": "object",
            "properties": parameters,
            "required": list(signature.parameters.keys()),
        }
    }
# **Register Functions with the ChatGPT API**:
tools = [add, convert_temperature]
tool_schemas = [function_to_schema(tool) for tool in tools]
```

### Convert tool defined in class into schema
To convert a tool defined in a class into a schema compatible with the ChatGPT API, you can extract the methods of the class (treated as tools) and map their details (like name, description, and parameters) into the schema format.

```python
class MathTools:
    def add(self, x: float, y: float) -> float:
        """Add two numbers."""
        return x + y

    def subtract(self, x: float, y: float) -> float:
        """Subtract two numbers."""
        return x - y
```

Class to schema

```python
import inspect

def class_method_to_schema(cls):
    """Convert all methods of a class into OpenAI tool schemas."""
    schemas = []
    for name, method in inspect.getmembers(cls, predicate=inspect.isfunction):
        signature = inspect.signature(method)
        parameters = {
            param_name: {"type": "number" if param.annotation in [int, float] else "string"}
            for param_name, param in signature.parameters.items()
        }
        schema = {
            "name": name,
            "description": method.__doc__,
            "parameters": {
                "type": "object",
                "properties": parameters,
                "required": list(signature.parameters.keys()),
            }
        }
        schemas.append(schema)
    return schemas
```

How to use it?
**Instantiate the Class and Convert to Schemas**
```python
math_tools = MathTools()
tool_schemas = class_method_to_schema(MathTools)

# `tool_schemas` now contains JSON schemas for all class methods
print(tool_schemas)
```
Example output of tool_schemas
```json
[
    {
        "name": "add",
        "description": "Add two numbers.",
        "parameters": {
            "type": "object",
            "properties": {
                "x": {"type": "number"},
                "y": {"type": "number"}
            },
            "required": ["x", "y"]
        }
    },
    {
        "name": "subtract",
        "description": "Subtract two numbers.",
        "parameters": {
            "type": "object",
            "properties": {
                "x": {"type": "number"},
                "y": {"type": "number"}
            },
            "required": ["x", "y"]
        }
    }
]
```
**Register the Tools with the ChatGPT API**
```python
# Pass the schemas when interacting with the API
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Add 3 and 4"}],
    functions=tool_schemas,
)
```

### Implement the Chat Interaction Loop:
A function to handle user input, manage conversation history, use tools and interact with the ChatGPT API.

```python
import json

def chat_with_tools_and_memory(user_input):
    memory.add_message("user", user_input)

    messages = [
        {"role": "system", "content": "You are a helpful assistant capable of using tools."},
        {"role": "user", "content": user_input},
    ] + memory.get_conversation()

    response = client.chat.completions.create(
        model="gpt-4-0613",  # Ensure you're using a model that supports function calling
        messages=messages,
        functions=tool_schemas,
    )

    message = response.choices[0].message

    if message.get("function_call"):
        function_call = message["function_call"]
        func_name = function_call["name"]
        arguments = json.loads(function_call["arguments"])

        # Call the corresponding Python function
        tool_map = {tool.__name__: tool for tool in tools}
        result = tool_map[func_name](**arguments)

        # Return the result to the assistant
        memory.add_message("assistant", str(result))
        return result

    memory.add_message("assistant", message["content"])
    return message["content"]
```

## Call the Methods Dynamically (Future work)
**Calling methods dynamically** means to invoking a method at runtime without hardcoding its name. Instead of directly calling math_tools.add(3, 4), you use the method’s name (as a string) and dynamically look up and execute it.

This is useful when:
• The method to be called isn’t known until runtime.
• You want to map string names (e.g., from a user query or an API) to actual method calls.
• You need flexibility to handle multiple methods without writing individual if-else or switch statements.

**Example in Real Use Case**
Suppose you integrate this with an API like ChatGPT:
1. The user asks the assistant: "Add 3 and 4".
2. The assistant recognizes the operation "add".
3. The corresponding method (MathTools.add) is dynamically identified and invoked using tool_map.
4. The result (7) is returned to the user.

> This dynamic lookup ensures the assistant is extensible and doesn’t require hardcoded mappings for every possible operation.

**Code Example**

```python
tool_map = {method.__name__: method for _, method in inspect.getmembers(math_tools, predicate=inspect.ismethod)}

# Dynamically call a method based on the tool name
tool_name = "add"
args = {"x": 3, "y": 4}
result = tool_map[tool_name](**args)
print(result)  # Output: 7
```

**Handle Tool Invocation Dynamically**
- After receiving the response, check if the assistant has identified a function call.
- Dynamically map the tool name to its corresponding method and invoke it.
Full example

```python
# Initialize
math_tools = MathTools()
tool_schemas = class_method_to_schema(MathTools)
tool_map = {method.__name__: method for _, method in inspect.getmembers(math_tools, predicate=inspect.ismethod)}
client = OpenAI()

# ChatGPT Interaction
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "You are a helpful assistant capable of using tools."},
        {"role": "user", "content": "Add 3 and 4"}
    ],
    functions=tool_schemas,
)

# Handle the tool call dynamically
if response.choices[0].message.get("function_call"):
    function_call = response.choices[0].message["function_call"]
    func_name = function_call["name"]
    args = json.loads(function_call["arguments"])
    result = tool_map[func_name](**args)  # Dynamically invoke the method

    print(f"Result: {result}")  # Return the result
```

**Send the Result Back to ChatGPT**:
• Use the result obtained from the dynamically invoked method to respond back to the user.
```python
assistant_response = client.chat.completions.create(
    model="gpt-4-0613",
    messages=[
        {"role": "system", "content": "You are a helpful assistant capable of using tools."},
        {"role": "user", "content": "Add 3 and 4"},
        {"role": "assistant", "content": str(result)}  # Pass the result back
    ]
)
```

**Explanation of Tools mapping**

```python
tool_map = {method.__name__: method for _, method in inspect.getmembers(math_tools, predicate=inspect.ismethod)}
```
1. inspect.getmembers(math_tools, predicate=inspect.ismethod):
- This retrieves all the methods of the object math_tools that are part of the MathTools class.
- inspect.ismethod ensures only methods (not attributes or properties) are retrieved.

2. Dictionary Comprehension:
- The comprehension iterates over the methods returned by getmembers.
- method.__name__: The name of the method (e.g., "add").
- method: The actual method object, which can be invoked with arguments.
- The result is a dictionary where keys are method names and values are the corresponding method objects.
Example output:
```python
{
    "add": <bound method MathTools.add of <MathTools object>>,
    "subtract": <bound method MathTools.subtract of <MathTools object>>,
}
```