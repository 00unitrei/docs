---
title: "Integration with Existing Services"
---

You can easily integrate your Unit into your existing services using the SDK or the python client.

### 1. Direct API Integration

#### JavaScript

```javascript
const ReiCoreSdk = require("reicore-sdk");

// Initialize the SDK
const secretKey = "your_unit_secret_token";
const reiAgent = new ReiCoreSdk({ agentSecretKey: secretKey });

// Simple integration example
async function getAgentResponse(query) {
  try {
    const payload = {
      messages: [
        {
          role: "user",
          content: query,
        },
      ],
    };
    const response = await reiAgent.chatCompletion(payload);
    return response.choices[0].message.content;
  } catch (error) {
    console.error("Error:", error);
    return null;
  }
}

// Example usage in an Express service
const express = require("express");
const app = express();

app.use(express.json());

app.post("/query", async (req, res) => {
  try {
    const response = await getAgentResponse(req.body.text);
    if (!response) {
      return res.status(500).json({ error: "Failed to get response" });
    }
    res.json({ response });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

#### Python

```python
from reicore_sdk import ReiCoreSdk

# Initialize the client
rei_agent = ReiCoreSdk("your_unit_secret_token")

# Simple integration example
def get_agent_response(query):
    try:
        payload = {
            "messages": [
                {
                    "role": "user",
                    "content": query
                }
            ]
        }

        response = rei_agent.chat.completion(payload)
        return response["choices"][0]["message"]["content"]
    except Exception as e:
        print(f"Error: {e}")
        return None

# Example usage in a web service
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

class Query(BaseModel):
    text: str

@app.post("/query")
async def process_query(query: Query):
    response = get_agent_response(query.text)
    if response is None:
        raise HTTPException(status_code=500, detail="Failed to get response")
    return {"response": response}
```

### 2. Webhook Integration

#### Setting up a Webhook Endpoint

```python
from flask import Flask, request, jsonify
from reicore_sdk import ReiCoreSdk

app = Flask(__name__)
rei_agent = ReiCoreSdk("your_unit_secret_token")

@app.route('/webhook', methods=['POST'])
async def webhook_handler():
    data = request.json
    query = data.get('query')

    if not query:
        return jsonify({"error": "No query provided"}), 400

    try:
        response = rei_agent.chat.completion(
            {
                "messages": [
                    {
                        "role": "user",
                        "content": query
                    }
                ]
            }
        )
        return jsonify({
            "response": response["choices"][0]["message"]["content"]
        })
    except Exception as e:
        return jsonify({"error": str(e)}), 500

```

### 3. Message Queue Integration

#### Using RabbitMQ

```python
import pika
from reicore_sdk import ReiCoreSdk
import json

# Initialize the client
rei_agent = ReiCoreSdk("your_unit_secret_token")

# Set up RabbitMQ connection
connection = pika.BlockingConnection(
    pika.ConnectionParameters('localhost')
)
channel = connection.channel()

# Declare queue
channel.queue_declare(queue='rei_agent_queue')

def process_message(ch, method, properties, body):
    try:
        data = json.loads(body)
        query = data.get('query')

        # Get response from Rei Agent
        response = rei_agent.chat.completion({
            "messages": [
                {
                    "role": "user",
                    "content": query
                }
            ]
        })

        # Send response back
        ch.basic_publish(
            exchange='',
            routing_key=properties.reply_to,
            body=json.dumps({
                "response": response["choices"][0]["message"]["content"]
            })
        )
    except Exception as e:
        ch.basic_publish(
            exchange='',
            routing_key=properties.reply_to,
            body=json.dumps({"error": str(e)})
        )

# Start consuming
channel.basic_consume(
    queue='rei_agent_queue',
    on_message_callback=process_message,
    auto_ack=True
)

channel.start_consuming()
```

### 4. Database Integration

#### Using PostgreSQL

```python
import psycopg2
from reicore_sdk import ReiCoreSdk
import json

# Initialize the client
rei_agent = ReiCoreSdk("your_unit_secret_token")

def store_and_process_query(query, user_id):
    try:
        # Connect to database
        conn = psycopg2.connect("dbname=your_db user=your_user password=your_password")
        cur = conn.cursor()

        # Get response from Rei Agent
        response = rei_agent.chat.completion({
            "messages": [
                {
                    "role": "user",
                    "content": query
                }
            ]
        })

        # Store query and response
        cur.execute("""
            INSERT INTO queries (user_id, query, response, created_at)
            VALUES (%s, %s, %s, NOW())
        """, (user_id, query, response.choices[0].message.content))

        conn.commit()
        return response["choices"][0]["message"]["content"]

    except Exception as e:
        print(f"Error: {e}")
        return None
    finally:
        if 'cur' in locals():
            cur.close()
        if 'conn' in locals():
            conn.close()
```

### 5. Microservice Integration

#### Using Docker and FastAPI

```dockerfile
# Dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```python
# main.py
from fastapi import FastAPI, HTTPException
from reicore_sdk import ReiCoreSdk
from pydantic import BaseModel
import uvicorn
import json

app = FastAPI()
rei_agent = ReiCoreSdk("your_unit_secret_token")

class Query(BaseModel):
    text: str
    context: dict = {}

@app.post("/process")
async def process_query(query: Query):
    try:
        response = rei_agent.chat.completion({
            "messages": [
                { "role": "system", "content": json.dumps(query.context) },
                { "role": "user", "content": query.text }
            ]
        })

        return {"response": response["choices"][0]["message"]["content"]}
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)
```
