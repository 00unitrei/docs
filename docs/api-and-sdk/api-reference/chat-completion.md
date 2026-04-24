---
title: "Chat Completion"
---

Chat completion by Reigent.

- **URL**: `/v1/chat/completions`
- **Method**: `POST`
- **Headers**:

  | Key           | Value                                                                                     |
  | ------------- | ----------------------------------------------------------------------------------------- |
  | Authorization | Bearer [**rei-agent-secret-token**](/docs/api-and-sdk/quickstart/#rei-agent-secret-token) |

- **Request**:

  ***

  **model** `string` Optional

  _Default_: Agent's Configured Model

  _Allowed values_:
  - `google/gemini-2.5-flash`

  ***

  **messages** `array` (min length: 1) Required

  <Accordion title="Show message structure">
    Each message object contains:

    **role** `string` Required  
     The role of the message author.  
     _Allowed values: `"system"`, `"user"`, `"assistant"`, `"tool"`_

    **content** `string` or `array` Required  
     The contents of the message. Can be:
    - Simple text string
    - Array of content parts (for multimodal inputs)

    <Accordion title="Show content parts structure">
      Each content part object contains:

      **type** `string` Required  
       The type of content part.  
       _Allowed values: `"text"`, `"image_url"`, `"file"`, `"file_url"`_

      **text** `string` Conditional  
       Text content (required when type is `"text"`)

      **image_url** `object` Conditional  
       Image URL details (required when type is `"image_url"`)  
       _Contains:_
      - **url** `string` Required  
        The URL of the image

      <br/>
      **file** `object` Conditional  
       File details (required when type is `"file"`)  
       _Contains:_
      - **filename** `string` Required  
        The name of the file
      - **file_data** `string` Required  
        The buffer content of the file

      <br/>
      **file_url** `string` Conditional  
       The URL of the file (required when type is `"file_url"`)
    </Accordion>

    **name** `string` Optional  
     An optional name for the participant

    **tool_call_id** `string` Optional  
     Required when role is `"tool"`

    **tool_calls** `array` Optional  
     Tool calls made by the assistant
  </Accordion>

  ***

  **temperature** `number` Optional

  _Range_: 0 to 2
  _Default_: 1

  ***

  **max_tokens** `integer` Optional

  _Minimum_: 1

  ***

  **top_p** `number` Optional

  _Range_: 0 to 1

  ***

  **n** `integer` Optional

  _Minimum_: 1

  ***

  **seed** `number` Optional

  _Range_: 0 to 2^53-1

  ***

  **stream** `boolean` Optional

  ***

  **stop** `string` or `array[string]` Optional

  ***

  **presence_penalty** `number` Optional

  _Range_: -2.0 to 2.0

  ***

  **frequency_penalty** `number` Optional

  _Range_: -2.0 to 2.0

  ***

  **logit_bias** `object` Optional

  _Key format_: Token IDs as string numbers
  _Value range_: -100 to 100

  ***

  **logprobs** `boolean` Optional

  ***

  **_top_logprobs_** `number` Optional

  ***

  **user** `string` Optional

  ***

  **response_format** `object` Optional

  Specifies the format of the model's output. Use this to request structured responses.

  <Accordion title="Show response_format structure">
    **type** `string` Required
    The type of response format.
    _Allowed values: `"text"`, `"json_object"`, `"json_schema"`_
    - `"text"` - Standard text response (default)
    - `"json_object"` - Response will be valid JSON
    - `"json_schema"` - Response will conform to a specified JSON schema

    **json_schema** `object` Conditional
    Schema definition (required when type is `"json_schema"`)
    _Contains:_
    - **name** `string` Required
      The name of the schema

    - **strict** `boolean` Optional
      Whether to enforce strict schema adherence. Default: `false`

    - **schema** `object` Required
      JSON Schema definition with:
      - **type** `string` - Schema type (e.g., `"object"`)
      - **properties** `object` - Property definitions
      - **required** `array` - Required property names
      - **additionalProperties** `boolean` - Allow extra properties
  </Accordion>

  ***

  **tools** `array` Optional  
   A list of tools the model may call

  <Accordion title="Show tool structure">
    Each tool object contains:

    **type** `string` Required  
     _Must be: `"function"`_

    **function** `object` Required  
     The function definition

    **function.name** `string` Required  
     The name of the function

    **function.parameters** `object` Required  
     The parameters the function accepts
  </Accordion>

  ***

  **tool_choice** `string` or `object` Optional  
   Controls which tool is called  
   _Allowed string values: `"none"`, `"auto"`_  
   _Or specify a tool with:_

  ```json
  {
    "type": "function",
    "function": {
      "name": "tool_name"
    }
  }
  ```

---

- **Sample Request**

1. **Type: Text**

<Accordion title="Show sample">
  ```json
  {
      "messages": [
          {
              "role": "user",
              "content": "Hello, can you help me with my research?"
          }
      ],
      "tools": [
          {
              "type": "function",
              "function": {
                  "name": "get_weather",
                  "description": "Get current temperature of given location",
                  "parameters": {
                      "type": "object",
                      "properties": {
                          "location": {
                              "type": "string",
                              "description": "City and country (e.g. Paris, France)"
                          }
                      },
                      "required": ["location"],
                      "additionalProperties": false
                  },
                  "strict": true
              }
          }
      ]
  };
  ```
</Accordion>

2. **Type: Image (in URL)**

<Accordion title="Show sample">
  ```json
  {
      "messages": [
          {
              "role": "user",
              "content": [
                {
                  "type": "text",
                  "text": "Hello, can you help me with my research?"
                },
                {
                  "type": "image_url",
                  "image_url": {
                    "url": "https://test.png"
                  }
                },
              ]
          }
      ],
      "tools": []
  };
  ```
</Accordion>

3. **Type: Image (in Base64)**

<Accordion title="Show sample">
  ```json
  {
      "messages": [
          {
              "role": "user",
              "content": [
                {
                  "type": "text",
                  "text": "Hello, can you help me with my research?"
                },
                {
                  "type": "image_url",
                  "image_url": {
                    "url": "data:image/png;base64,iV..."
                  }
                },
              ]
          }
      ],
      "tools": []
  };
  ```
</Accordion>

4. **Type: Docs (PDF)**

<Accordion title="Show sample">
  ```json
  {
      "messages": [
          {
              "role": "user",
              "content": [
                {
                  "type": "text",
                  "text": "Hello, what's inside the PDF?"
                },
                {
                  "type": "file",
                  "file": {
                    "filename": "Sample File Name",
                    "file_data": "data:application/pdf;base64,JVBERi0xLjMNCiXi48/....",
                  }
                }
              ]
          }
      ],
      "tools": []
  };
  ```
</Accordion>

5. **Type: Docs**

- Supported File Types: - `json`, `xlsx`, `xlsm`, `csv`, `md`, `pptx`, `docx`, `txt`

<Accordion title="Show sample">
  ```json
  {
      "messages": [
          {
              "role": "user",
              "content": [
                {
                  "type": "text",
                  "text": "Hello, what's inside the PDF?"
                },
                {
                  "type": "input_file",
                  "file_url": "https://file_url",
                }
              ]
          }
      ],
      "tools": []
  };
  ```
</Accordion>

6. **Type: JSON Schema Response**

<Accordion title="Show sample">
  ```json
  {
    "model": "google/gemini-2.5-flash",
    "messages": [
      {
        "role": "user",
        "content": "Tell me about a cat named Whiskers. Return the response as a JSON object with the following fields: 'name' (string), 'color' (string - the cat's fur color), 'age' (integer - in years), and 'personality' (string - brief description). Make sure to include all required fields."
      }
    ],
    "response_format": {
      "type": "json_schema",
      "json_schema": {
        "name": "cat_info",
        "strict": true,
        "schema": {
          "type": "object",
          "properties": {
            "name": {
              "type": "string",
              "description": "The cat's name"
            },
            "color": {
              "type": "string",
              "description": "The cat's fur color"
            },
            "age": {
              "type": "integer",
              "description": "The cat's age in years"
            },
            "personality": {
              "type": "string",
              "description": "Brief description of the cat's personality"
            }
          },
          "required": ["name", "color", "age", "personality"],
          "additionalProperties": false
        }
      }
    }
  }
  ```
</Accordion>

---

- **Response**:

1. **Without Tools**

<Accordion title="Show sample">
  ```json
  {
    "choices": [
      {
        "index": 0,
        "message": {
          "content": "Hello! How can I assist you today?",
          "role": "assistant"
        }
      }
    ]
  }
  ```
</Accordion>

2. **With Tools**

<Accordion title="Show sample">
  ```json
  {
    "choices": [
      {
        "index": 0,
        "message": {
          "content": "",
          "role": "assistant",
          "tool_calls": [
            {
              "id": "call_zSIBPi4QKxjkpAewfi5YbTnI",
              "type": "function",
              "function": {
                "name": "get_weather",
                "arguments": "{\"location\":\"Paris, France\"}"
              }
            }
          ]
        }
      }
    ]
  }
  ```
</Accordion>

3. **With JSON Schema**

<Accordion title="Show sample">
  ```json
  {
    "choices": [
      {
        "index": 0,
        "message": {
          "content": "{\"name\":\"Whiskers\",\"color\":\"orange tabby\",\"age\":3,\"personality\":\"Playful and curious, loves to explore and cuddle\"}",
          "role": "assistant"
        }
      }
    ]
  }
  ```
</Accordion>

---

- **Error**

| Response Code | Reason           |
| ------------- | ---------------- |
| 400           | Validation Error |
| 401           | Unauthorized     |
| 404           | Agent not found  |
