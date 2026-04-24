---
title: "Quickstart"
---

The Reigent API is a powerful interface designed to interact with the Reigent, enabling seamless integration for authentication, agent retrieval, and chat completions. This API simplifies the process of connecting to the Reigent and utilizing its features.

## Authentication

### Rei Agent API Key

To authenticate the REI Agent API requests

<AccordionGroup>
  <Accordion title="How to get a Rei Agent API Key" icon="circle-question">
    #### Steps

    1. Navigate to [Reigent Portal](https://app.reilabs.org/)

    2. Create an agent as stated in the Reigent Portal.

    3. Find the agent from the side bar at the left.

    4. Click `...`˝

    5. Click `Agent Details`

    6. Locate your Agent `Secret Key` from the pop-out dialog.
       ![Agent Secret Key pop-out dialog](../intro-rei-secret-view.png)

    7. Copy.

    - Treat this key as highly sensitive—it grants full API access.
    - Never expose it in client-side code or version control (e.g., Git).

    8. Key rotation

    - You may regenerate the Secret Key if is needed.
    - Only the latest generated Secret Key is valid.
  </Accordion>

  <Accordion title="How to use Rei Agent API Key" icon="wrench">
    **Example:**

    ```http
    GET /v1/{...} HTTP/1.1
    Authorization: Bearer YOUR_REIGENT_UNIT_SECRET_KEY
    ```
  </Accordion>
</AccordionGroup>

### User API Key

To manage resources

<AccordionGroup>
  <Accordion title="How to get a User API Key" icon="circle-question">
    #### Steps

    1. Navigate to [Reigent Portal](https://app.reilabs.org/)

    2. Create an agent as stated in the Reigent Portal.

    3. Click on three dots next to the `Create`.

       ![Top right bar of Reigent Portal](../intro-user-secret-view.png)

    4. Click `View API`.

       ![User API Key pop-out dialog](../intro-user-secret-view-2.png)

    5. Locate your `User Secret Key` from the pop-out dialog.

       ![User API Key pop-out dialog](../intro-user-secret-view-3.png)

    6. Turn the key to `Active`

    7. Copy.

    - Treat this key as highly sensitive—it grants full API access.
    - Never expose it in client-side code or version control (e.g., Git).

    8. Key rotation

    - You may regenerate the User Secret Key if is needed.
    - Only the latest generated User Secret Key is valid.
  </Accordion>

  <Accordion title="How to use User API Key" icon="wrench">
    **Example:**

    ```http
    GET /v1/{...} HTTP/1.1
    Authorization: Bearer YOUR_USER_SECRET_KEY
    ```
  </Accordion>
</AccordionGroup>

## Base URL

```
https://api.reilabs.org
```
