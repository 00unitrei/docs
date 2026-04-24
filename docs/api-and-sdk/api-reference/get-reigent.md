---
title: "Get Reigent"
---

**Retrieve a Reigent Unit.**

- **URL**: `/v1/agents`
- **Method**: `GET`
- **Headers**:

  | Key           | Value                                                                                     |
  | ------------- | ----------------------------------------------------------------------------------------- |
  | Authorization | Bearer [**rei-agent-secret-token**](/docs/api-and-sdk/quickstart/#rei-agent-secret-token) |

- **Response:**

```json
{
  "id": 00,
  "name": "Agent XX",
  "agent_functionalities": "",
  "agent_model": {
    "id": 1,
    "name": "XX",
    "model_name": "XX"
  },
  "response_format": "text",
  "temperature": 0.7,
  "max_tokens": 32000
}
```

- **Error**

| Response Code | Reason          |
| ------------- | --------------- |
| 401           | Unauthorized    |
| 404           | Agent not found |
