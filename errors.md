# ❌ Error Codes & Handling

The API returns structured JSON responses for validation results and errors. Most API error responses include an internal `errorCode`, a `status`, and a `message`.

## HTTP Status & Error Mappings

| HTTP Code | status  | errorCode | message                                             |
|:----------|:--------|:----------|:----------------------------------------------------|
| **_General Errors (1xxx)_** | | | |
| **500**   | `error` | 1001      | Internal server error                               |
| **404**   | `error` | 1002      | Invalid path                                        |
| **400**   | `error` | 1003      | Invalid argument                                    |
| **502**   | `error` | 1004      | Gateway error                                       |
| **405**   | `error` | 1005      | The HTTP method used is not allowed                 |
| **415**   | `error` | 1006      | Unsupported Media Type. Check Content-Type header   |
| **_Auth Errors (2xxx)_** | | | |
| **401**   | `error` | 2001      | Authentication required. Missing credentials        |
| **403**   | `error` | 2002      | Invalid API key or the user is disabled             |
| **403**   | `error` | 2003      | User not found in the system                        |
| **403**   | `error` | 2004      | Access denied                                       |
| **_Validation Errors (3xxx)_** | | | |
| **400**   | `error` | 3001      | Validation failed                                   |
| **429**   | `error` | 3003      | Rate limit exceeded                                 |

---

## 🛠 Example Responses

### ✅ Successful Upload (Sandbox Goal)
Your primary goal during testing is to successfully upload a valid XML file to the Sandbox environment. Once you receive the `ok` status as shown below, you can contact your manager to request production access.

```json
{
  "supplierCode": "FAKE_USER",
  "status": "ok",
  "message": "File is valid"
}
```

### ❌ Validation Error
When a validation error or limit is triggered, you will receive a JSON response indicating exactly what went wrong. 

For example, this validation error occurred because the `price` field requires a positive integer (in cents), but a decimal value was provided instead (e.g. `500.00` instead of `50000`).

```json
{
  "supplierCode": "FAKE_USER",
  "status": "error",
  "message": "Validation failed",
  "errorCode": 3001,
  "errors": [
    "Validation error : XSD Error: '500.00' is not a valid value for 'integer'."
  ]
}
```

### ⚠️ 413 Payload Too Large (File size limit)
Unlike other validation errors, the `413 Request Entity Too Large` error does **not** return a JSON body. The response body will be completely empty.

Instead, the specific reason and limits are provided directly in the HTTP headers. Look for the `errorMessage` header in your HTTP client's response:

```http
HTTP/1.1 413 Request Entity Too Large
errorMessage: Request size is larger than permissible limit. Request size is 110 MB where permissible limit is 100 MB
content-length: 0
```

---
**Navigation:** [🏠 Back to Home](README.md) | [📄 Data Schema](schema.md) | [💬 Support](FEEDBACK.md)