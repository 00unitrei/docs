---
title: "Agent Creation"
---

Create Rei Agent by User's Secret Token.

- **URL**: `/v1/accounts/units`
- **Method**: `POST`
- **Headers**:

  | Key           | Value                                                                           |
  | ------------- | ------------------------------------------------------------------------------- |
  | Authorization | Bearer [**user-secret-token**](/docs/api-and-sdk/quickstart/#user-secret-token) |

---

- **Request**:

  **agentModel** `string` Required

  _Allowed values_:
  - `google/gemini-2.5-flash`

  ***

  **tag** `string` Required

  Tag to identify your unit

  ***

  **behaviourPrompt** `string` Required

  ***

  **temperature** `int` Optional

  _Range_: 0 to 1

  ***

  **maxTokens** `int` Optional

  _Range_: 1 to 2000000

  ***

  **responseFormat** `string` Optional

  _Allowed values_: `text`, `json`, `markdown`, `html`

  ***

  **color** `string` Optional

  Color code in Hex format

  _Format_: `#FFFFFF`

---

- **Response**:

```json
{
  "secretToken": "{{rei agent secret token}}"
}
```

---

- **Error**

| Response Code | Reason           |
| ------------- | ---------------- |
| 400           | Validation Error |
| 401           | Unauthorized     |
