---
title: "Get Reigent Tools"
---

**Retrieve a Reigent Unit's Tools.**

- **URL**: `/v1/agents/tools`
- **Method**: `GET`
- **Headers**:

  | Key           | Value                                                                                     |
  | ------------- | ----------------------------------------------------------------------------------------- |
  | Authorization | Bearer [**rei-agent-secret-token**](/docs/api-and-sdk/quickstart/#rei-agent-secret-token) |

- **Response:**

```json
{
    "features": [
        {
            "key": "crypto",
            "label": "Crypto",
            "isActive": true
        },
        {
            "key": "crypto:laevitas",
            "label": "Laevitas",
            "isActive": false
        },
        {
            "key": "",
            "label": "",
            "isActive":
        },
        ...
    ]
}
```

---

- **Error**

| Response Code | Reason       |
| ------------- | ------------ |
| 401           | Unauthorized |
