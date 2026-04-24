---
title: "Agent Clone"
---

Clone Rei Agent by User's Secret Token.

- **URL**: `/v1/accounts/units/clone`
- **Method**: `POST`
- **Headers**:

  | Key           | Value                                                                           |
  | ------------- | ------------------------------------------------------------------------------- |
  | Authorization | Bearer [**user-secret-token**](/docs/api-and-sdk/quickstart/#user-secret-token) |

---

- **Request**:

  **agentId** `string` Required

  _rei_agent_id_ from [**Get Agent API**](/docs/api-and-sdk/api-reference/get-reigent.md)

---

- **Response**:

```json
{
  "isCloneSuccess": true,
  "secretToken": "{{rei agent secret token}}"
}
```

---

- **Error**

| Response Code | Reason               |
| ------------- | -------------------- |
| 401           | Unauthorized         |
| 404           | User Agent not found |
