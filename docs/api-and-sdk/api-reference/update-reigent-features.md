---
title: "Update Reigent Features"
---

** Update a Reigent Unit's Tools.**

- **URL**: `/v1/agents/tools`
- **Method**: `PUT`
- **Headers**:

  | Key           | Value                                                                                     |
  | ------------- | ----------------------------------------------------------------------------------------- |
  | Authorization | Bearer [**rei-agent-secret-token**](/docs/api-and-sdk/quickstart/#rei-agent-secret-token) |

- **Request**:

  **key** `string` Required

  _Allowed values_:
  - crypto
  - crypto:laevitas
  - apex_chart
  - image_gen
  - skip_web_search

  _For Latest values_:
  - Retrieve from [**Get Reigent Unit's Tools**](./get-reigent-tools.md)

  ***

  **isActive** `boolean` Required

---

- **Response**:

```json
{
  "isUpdated": true
}
```

---

- **Error**

| Response Code | Reason           |
| ------------- | ---------------- |
| 400           | Validation Error |
| 401           | Unauthorized     |
