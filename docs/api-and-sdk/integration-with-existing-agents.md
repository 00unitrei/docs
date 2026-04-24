---
title: "Integration with Existing Agents"
---

Your Unit can be easily integrated with other AI systems through function calling. Here's how to do it:

#### JavaScript

```javascript
const ReiCoreSdk = require("reicore-sdk");

const apiKey = "your_unit_secret_token";
const reiAgent = new ReiCoreSdk({ agentSecretKey: apiKey });

// Example function to query Rei Agent
async function queryReiAgent(message) {
  try {
    const response = await reiAgent.chatCompletions(message);
    return response;
  } catch (error) {
    console.error("Error querying Rei Agent:", error);
    return null;
  }
}

// Example usage in your agent
async function yourAgentFunction() {
  // Your agent's logic here
  const query = "What are the latest developments in quantum computing?";
  const reiResponse = await queryReiAgent(query);
  // Process the response
}
```

#### Python

```python
from client import Client

client = Client(
    api_key="your_unit_secret_token",
    base_url="https://api.reilabs.org"
)

# Example function to query Rei Agent
def query_rei_agent(message):
    try:
        response = client.chat.completions.create(
            model="Unit01",
            messages=[
                {"role": "user", "content": message}
            ],
            functions=[{
                "name": "query_rei_agent",
                "description": "Query the Rei Agent for information or assistance",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "query": {
                            "type": "string",
                            "description": "The query to send to the Rei Agent"
                        }
                    },
                    "required": ["query"]
                }
            }]
        )
        return response.choices[0].message.content
    except Exception as e:
        print(f"Error querying Rei Agent: {e}")
        return None

# Example usage in your agent
def your_agent_function():
    # Your agent's logic here
    query = "What are the latest developments in quantum computing?"
    rei_response = query_rei_agent(query)
    # Process the response
```

### Example integration with OpenAI

```python
from openai import OpenAI
from client import Client as ReiClient

# Initialize both clients
openai_client = OpenAI(api_key="your_openai_key")
rei_client = ReiClient(api_key="your_unit_secret_token")

def hybrid_agent_query(query):
    # First, get context from OpenAI
    openai_response = openai_client.chat.completions.create(
        model="gpt-4o",
        messages=[{"role": "user", "content": query}]
    )

    # Then, enhance with Rei Agent's specialized knowledge
    rei_response = rei_client.chat.completions.create(
        model="Unit01",
        messages=[
            {"role": "user", "content": query},
            {"role": "assistant", "content": openai_response.choices[0].message.content}
        ]
    )

    return rei_response.choices[0].message.content
```

Integrating a Unit as a counselor for common LLMs models allows the seamless integration of memories: simply passing the query and asking for more details unlocks memory without having to code message loops.
