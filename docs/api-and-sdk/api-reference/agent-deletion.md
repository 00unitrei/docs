---
title: "Agent Deletion"
---

Delete Rei Agent by User's Secret Token.

- **URL**: `/v1/accounts/units`
- **Method**: `DELETE`
- **Headers**:

  | Key           | Value                                                                           |
  | ------------- | ------------------------------------------------------------------------------- |
  | Authorization | Bearer [**user-secret-token**](/docs/api-and-sdk/quickstart/#user-secret-token) |

---

- **Request**:

  **agentKey** `string` Required

  _rei-agent-secret-token_ from Rei Portal

---

- **Response**:

| Response Code |         |
| ------------- | ------- |
| 204           | Success |

---

- **Error**

| Response Code | Reason               |
| ------------- | -------------------- |
| 401           | Unauthorized         |
| 404           | User Agent not found |
