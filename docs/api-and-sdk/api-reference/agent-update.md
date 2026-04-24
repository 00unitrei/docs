---
title: "Agent Update"
---

Update Rei Agent by User's Secret Token.

- **URL**: `/v1/accounts/units`
- **Method**: `PUT`
- **Headers**:

  | Key           | Value                                                                           |
  | ------------- | ------------------------------------------------------------------------------- |
  | Authorization | Bearer [**user-secret-token**](/docs/api-and-sdk/quickstart/#user-secret-token) |

---

- **Request**:

  **agentKey** `string` Required

  ***

  **tag** `string` Optional

  Tag to identify your unit

  ***

  **behaviourPrompt** `string` Optional

  Agent's behaviour prompt

  ***

  **description** `string` Optional

  Agent's description

  ***

---

- **Response**:

```json
{
  "updated": "true"
}
```

---

- **Error**

| Response Code | Reason           |
| ------------- | ---------------- |
| 400           | Validation Error |
| 401           | Unauthorized     |
