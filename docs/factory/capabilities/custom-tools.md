---
title: "Custom Tools"
---

Following the Function Calling schema, you can develop your own tools and functions and give your Unit access to them - as you'd do with a regular model - making the Unit forms usage memories with them.

The only actual limitation is that those tools are only in your code: outside of it, the Unit will still have memories of them but it will be impossible for it to recall the tools, leading to possible weird behavior.

Solution: Once you plug an Unit in your code, try to use it only there, especially if there it has access to specific functions not available in the Factory.

We're currently testing a couple of solutions to let users define Custom Tools directly in the factory and an update will soon come together with a Vault System where you can safely store your data and keys without compromising opsec.

### Custom Tools Quickstart

#### JavaScript

```javascript
const ReiCoreSdk = require("reicore-sdk");

// Initialize the SDK
const apiKey = "your_unit_secret_token";
const reiAgent = new ReiCoreSdk({ agentSecretKey: apiKey });

// Define your custom functions
const getWeather = (location) => {
  // Implement your weather API call here
  return `Weather in ${location}: Sunny, 22°C`;
};

const searchDatabase = (query) => {
  // Implement your database search here
  return `Found 3 results for: ${query}`;
};

// Define function schemas
const functions = [
  {
    type: "function",
    function: {
      name: "get_weather",
      description: "Get the current weather for a location",
      parameters: {
        type: "object",
        properties: {
          location: {
            type: "string",
            description: "The city and state, e.g. San Francisco, CA",
          },
        },
        required: ["location"],
      },
    },
  },
  {
    type: "function",
    function: {
      name: "search_database",
      description: "Search the database for specific information",
      parameters: {
        type: "object",
        properties: {
          query: {
            type: "string",
            description: "The search query",
          },
        },
        required: ["query"],
      },
    },
  },
];

// Function mapping
const functionMap = {
  get_weather: getWeather,
  search_database: searchDatabase,
};

async function processWithFunctions(query) {
  try {
    // First call to get function details
    const response = await reiAgent.chatCompletion({
      messages: [{ role: "user", content: query }],
      tools: functions,
    });
    const message = response.choices[0].message;

    // Check if the agent wants to call a function
    if (message.tool_calls && message.tool_calls.length) {
      const tool = message.tool_calls[0];
      const functionName = tool.function.name;
      const functionArgs = JSON.parse(tool.function.arguments);

      // Call the function
      const functionResponse = functionMap[functionName](
        ...Object.values(functionArgs),
      );

      // Send the function response back to the agent
      const secondResponse = await reiAgent.chatCompletion({
        messages: [
          { role: "user", content: query },
          { role: "tool", tool_call_id: tool.id, content: functionResponse },
        ],
      });

      return secondResponse.choices[0].message.content;
    }

    return message.content;
  } catch (error) {
    console.error("Error:", error);
    return null;
  }
}

// Example usage
processWithFunctions("What's the weather in Tokyo?")
  .then((response) => console.log(response))
  .catch((error) => console.error(error));
```

#### Python

```python
from client import Client
import json

# Initialize the client
client = Client(
    api_key="your_unit_secret_token",
    base_url="https://api.reilabs.org"
)

# Define your custom functions
def get_weather(location: str) -> str:
    # Implement your weather API call here
    return f"Weather in {location}: Sunny, 22°C"

def search_database(query: str) -> str:
    # Implement your database search here
    return f"Found 3 results for: {query}"

# Define function schemas
functions = [
    {
        "name": "get_weather",
        "description": "Get the current weather for a location",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "The city and state, e.g. San Francisco, CA"
                }
            },
            "required": ["location"]
        }
    },
    {
        "name": "search_database",
        "description": "Search the database for specific information",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {
                    "type": "string",
                    "description": "The search query"
                }
            },
            "required": ["query"]
        }
    }
]

# Function mapping
function_map = {
    "get_weather": get_weather,
    "search_database": search_database
}

def process_with_functions(query: str):
    try:
        # First call to get function details
        response = client.chat.completions.create(
            model="Unit01",
            messages=[{"role": "user", "content": query}],
            functions=functions
        )

        message = response.choices[0].message

        # Check if the agent wants to call a function
        if message.function_call:
            function_name = message.function_call.name
            function_args = json.loads(message.function_call.arguments)

            # Call the function
            function_response = function_map[function_name](**function_args)

            # Send the function response back to the agent
            second_response = client.chat.completions.create(
                model="Unit01",
                messages=[
                    {"role": "user", "content": query},
                    {"role": "function", "name": function_name, "content": function_response}
                ],
                functions=functions
            )

            return second_response.choices[0].message.content

        return message.content

    except Exception as e:
        print(f"Error: {e}")
        return None

# Example usage
response = process_with_functions("What's the weather in Tokyo?")
print(response)
```

### Advanced Custom Tools

#### Multiple Function Calls

```python
def process_with_multiple_functions(query: str):
    try:
        messages = [{"role": "user", "content": query}]

        while True:
            response = client.chat.completions.create(
                model="Unit01",
                messages=messages,
                functions=functions
            )

            message = response.choices[0].message

            if not message.function_call:
                return message.content

            # Add the function call to messages
            messages.append({
                "role": "assistant",
                "content": None,
                "function_call": {
                    "name": message.function_call.name,
                    "arguments": message.function_call.arguments
                }
            })

            # Call the function
            function_name = message.function_call.name
            function_args = json.loads(message.function_call.arguments)
            function_response = function_map[function_name](**function_args)

            # Add the function response to messages
            messages.append({
                "role": "function",
                "name": function_name,
                "content": function_response
            })

    except Exception as e:
        print(f"Error: {e}")
        return None
```

#### Function Calling with Context

```python
def process_with_context(query: str, context: dict):
    try:
        # Add context to the system message
        system_message = {
            "role": "system",
            "content": json.dumps(context)
        }

        response = client.chat.completions.create(
            model="Unit01",
            messages=[
                system_message,
                {"role": "user", "content": query}
            ],
            functions=functions
        )

        # Process function calls as before
        message = response.choices[0].message
        if message.function_call:
            function_name = message.function_call.name
            function_args = json.loads(message.function_call.arguments)
            function_response = function_map[function_name](**function_args)

            second_response = client.chat.completions.create(
                model="Unit01",
                messages=[
                    system_message,
                    {"role": "user", "content": query},
                    {"role": "function", "name": function_name, "content": function_response}
                ],
                functions=functions
            )

            return second_response.choices[0].message.content

        return message.content

    except Exception as e:
        print(f"Error: {e}")
        return None
```
