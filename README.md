# Suppliers API

This API allows you to automate the upload of your product data (prices, stock, basic information) directly into our system.

## 🛠 Onboarding Process

Follow these steps to integrate with our system:
1. **Request Access**: Contact your assigned manager to provide your **Static IP address** and **Email**. This is required to add your system to our IP Whitelist.
2. **Sandbox Testing**: Once your IP is whitelisted, you can test uploading files to the Sandbox environment.
3. **Go Live**: After successful tests, notify your manager. We will provide you with an **API_KEY** and **USERNAME** for production access.

---

## 🚀 Quickstart

Below is an example of uploading a product XML file to the Sandbox environment using `cURL`. 

```bash
curl -X 'POST' \
  'https://suppliers.rde.lv/sandbox/api/v1/goods' \
  -H 'X-User-Name: FAKE_USER' \
  -H 'Authorization: Bearer FAKE_PASSWORD' \
  -H 'Content-Type: application/xml' \
  --data-binary @goods-example.xml
```

---

## 🌍 Connection Details & Limits

| Environment    | Base URL                   | Endpoint                     |
|:---------------|:---------------------------|:-----------------------------|
| **Sandbox**    | `https://suppliers.rde.lv` | `POST /sandbox/api/v1/goods` |
| **Production** | `https://suppliers.rde.lv` | `POST /api/v1/goods`         |

### Authentication
All production requests must include authentication headers. Sandbox environment does not require real credentials, but you must pass dummy values.
* `X-User-Name`: `<USERNAME>`
* `Authorization`: `Bearer <YOUR_API_KEY>`

### Limits
* **Max File Size**: 100 MB per request.
* **Rate Limits**:
  * **Sandbox**: 1 request per minute.
  * **Production**: 1 request per hour.

---

## 📚 Documentation Reference

For detailed technical specifications, please refer to the following documents:

* 📄 **[Data Schema & Field Descriptions](schema.md)** - Detailed requirements for the XML structure.
* ❌ **[Error Codes & Handling](errors.md)** - Explanations for all HTTP statuses and system errors.
* 💾 **[XSD Schema](goods-1.0.xsd)** - Raw XSD schema file for validation.
* 📝 **[Example XML](goods-example.xml)** - A sample valid XML file.

---

If you experience any difficulties or have questions, please reach out via our [Support Feedback Form](FEEDBACK.md).
