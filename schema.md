# 📄 Data Schema & Field Descriptions

The uploaded XML file must strictly adhere to the [goods-1.0.xsd](goods-1.0.xsd) schema. The file must contain a list of `products`, and may contain an optional `metadata` block.

## General Rules & Data Formats

> Each uploaded file must contain a **full catalog snapshot** of all products involved in the exchange. This allows our system to automatically mark missing products as unavailable for order. Partial updates (for individual products or groups) are not supported.

* **Encoding:** All files and the data within them must be in **UTF-8** encoding.
* **Strict XML Structure:** Additional tags and parameters inside the fields described below are not permitted (even those otherwise allowed by the XML standard).

### Rules for String and Numeric Fields
* **String Fields:** The use of XML reserved characters (`&`, `<`, `>`, `"`, `'`) is prohibited in raw text. If necessary, please place the text inside a `<![CDATA[ ... ]]>` block.
* **Numeric Fields:** All numeric values should be transmitted only as integers (0-9) without decimal or thousands separators, spaces, or other characters:
  * **Price:** in euro cents (original price multiplied by 100).
  * **Weight:** in grams (kilograms multiplied by 1000).

### File Verification Before Submission
Please perform the following actions before submitting data to the API:
1. Open the file in the Google Chrome browser: it should open without errors and display correctly (this is a basic well-formed XML check).
2. Configure your system to perform automatic validation against our XSD schema (see Download Schema below) before every submission.

---

## Root Element: `<data>`
Must include the namespace `xmlns="http://suppliers.rde.lv/api/v1/goods"`.

### Metadata Elements (`<metadata>`)
Optional but recommended block describing the file payload.

| Field | Required | Clarification & Requirements |
|:---|:---|:---|
| `version` | No | Schema version. If provided, the value must be fixed to 1.0 |
| `created_at` | No | File creation timestamp in strict UTC (Zulu Time) format per ISO 8601. Must end with the "Z" designator to explicitly exclude local time zone offsets (e.g., 2026-05-05T14:35:40Z). |
| `total_count` | No | Total number of product records (`<product>` elements) contained within the file (integer ≥ 0). Used for data integrity verification. |

### Product Elements (`<product>`)
Inside the `<products>` block, provide one or more `<product>` entries.

| Field | Required | Clarification & Requirements |
|:---|:---|:---|
| `id` | **Yes** | Unique product identifier in the system. Must be unique across the entire catalog, cannot be empty, and cannot change over time. |
| `category` | **Yes** | Product category (group). String, maximum 65 characters. Cannot be empty. |
| `brand` | **Yes** | Product brand (manufacturer). String, maximum 65 characters. Cannot be empty. |
| `country` | No | Manufacturer's country code per ISO 3166-1 alpha-2 (exactly 2 characters, e.g., LV, DE, PL). |
| `sku` | **Yes** | Manufacturer SKU. Unique in the catalog, cannot be changed. Cannot be empty. |
| `ean` | **Yes** | GTIN (EAN-8/13/128 and similar). 8-25 characters, A-Z0-9 only. Unique, cannot be changed, no spaces or special characters. Cannot be empty. |
| `name` | **Yes** | Product name (LV or EN only). Maximum 65 characters. Cannot be empty. |
| `quantity` | **Yes** | Stock balance (integer >= 0). Text values like "greater than" are not permitted. |
| `price` | **Yes** | Price excluding VAT in euro cents (integer > 0). |
| `currency` | See Note | Currency code per ISO 4217 (3 characters). If the price is in EUR, this field may be omitted. |
| `cost` | No | Recommended retail price in euro cents (integer > 0). |
| `weight` | No | Net weight in grams (0-9 only). |
| `weight_brutto` | No | Gross weight in grams (0-9 only). |
| `size` | No | Packaging dimensions: Length x Width x Height in cm. Example: 10x20x30 (numbers and "x" only, no spaces). |
| `images` | No | Array of product images. |
| `img` | No | Full direct link (JPG, BMP, PNG, GIF, TIFF, WebP). |
| `delivery` | No | Delivery time to our warehouse in hours (integer). |
| `desc` | No | Extended description. HTML tags are allowed. Must be wrapped in `<![CDATA[ ... ]]>`. |
| `intrastat` | No | INTRASTAT code (0-9 only, strictly 8 characters). |

---
**Navigation:** [🏠 Back to Home](README.md) | [❌ Error Codes](errors.md) | [💬 Support](FEEDBACK.md)
