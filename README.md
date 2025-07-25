# 📦 Product Upload API Documentation

API này cho phép đăng sản phẩm từ Google Sheet hoặc hệ thống khác lên WooCommerce thông qua Webhook trên n8n.

---

## 🔗 Webhook Endpoint

**URL:**  
`POST https://n8n.bloommedia.space/webhook/3ac4b2d7-7e93-4580-bc9b-89c1428df90c`

**Method:** `POST`  
**Content-Type:** `application/json`

---

## 📌 Body Parameters

### 🧾 Thông tin sản phẩm

| Field                     | Type    | Required | Description                                  |
| ------------------------- | ------- | -------- | -------------------------------------------- |
| `Type`                    | string  | ✅       | Loại sản phẩm: `simple`, `variable`, v.v.    |
| `SKU`                     | string  | ✅       | Mã sản phẩm duy nhất                         |
| `GTIN, UPC, EAN, or ISBN` | string  | ❌       | Mã định danh sản phẩm (GTIN, UPC, EAN, ISBN) |
| `Name`                    | string  | ✅       | Tên sản phẩm                                 |
| `Published`               | boolean | ✅       | `1` nếu hiển thị, `0` nếu ẩn                 |
| `Is featured?`            | boolean | ✅       | `1` nếu nổi bật                              |
| `Visibility in catalog`   | string  | ✅       | `visible`, `catalog`, `search`, `hidden`     |
| `Short description`       | string  | ✅       | Mô tả ngắn                                   |
| `Description`             | string  | ✅       | Mô tả chi tiết                               |
| `Date sale price starts`  | string  | ❌       | Ngày bắt đầu giảm giá (ISO format)           |
| `Date sale price ends`    | string  | ❌       | Ngày kết thúc giảm giá (ISO format)          |
| `Tax status`              | string  | ✅       | `taxable`, `shipping`, `none`                |
| `Tax class`               | string  | ❌       | Loại thuế cụ thể nếu có                      |
| `In stock`                | boolean | ✅       | `1` nếu còn hàng                             |
| `Stock`                   | number  | ✅       | Số lượng còn lại                             |
| `Low stock amount`        | number  | ❌       | Cảnh báo khi gần hết hàng                    |
| `Backorders allowed?`     | boolean | ✅       | `1` cho phép đặt hàng trước                  |
| `Sold individually?`      | boolean | ✅       | `1` chỉ mua 1 sản phẩm mỗi đơn               |
| `Weight (kg)`             | number  | ✅       | Trọng lượng sản phẩm (kg)                    |
| `Length (cm)`             | number  | ✅       | Chiều dài (cm)                               |
| `Width (cm)`              | number  | ✅       | Chiều rộng (cm)                              |
| `Height (cm)`             | number  | ✅       | Chiều cao (cm)                               |
| `Allow customer reviews?` | boolean | ✅       | `1` nếu cho phép đánh giá                    |
| `Purchase note`           | string  | ❌       | Ghi chú khi mua hàng                         |
| `Sale price`              | string  | ❌       | Giá khuyến mãi                               |
| `Regular price`           | string  | ✅       | Giá niêm yết                                 |
| `Shipping class`          | string  | ❌       | Lớp vận chuyển nếu có                        |

---

### 🏷 Categories

**Type:** `string`  
**Format:** Dùng `>` để thể hiện phân cấp, `,` để phân tách nhiều nhóm.

**Ví dụ:**

```json
"Categories": "Hawaiian Shirts > NFL Hawaiian Shirts > Kansas City Chiefs Hawaiian Shirts, Hawaiian Shirts"
```

---

### 🖼 Images

**Type:** `string`  
**Format:** Danh sách URL ảnh, phân cách bằng dấu phẩy `,`

**Ví dụ:**

```json
"Images": "https://yourcdn.com/image1.jpg, https://yourcdn.com/image2.jpg"
```

---

### 🔠 Attributes

Tối đa 3 thuộc tính: `Attribute 1 name`, `Attribute 1 value(s)`, `Attribute 1 visible`, `Attribute 1 global`, … đến `Attribute 3 .

Mỗi Attribute c cấu trúc:

```json
"Attribute 1 name": "Color",
"Attribute 1 value(s)": "Red, White",
"Attribute 1 visible": 1,
"Attribute 1 global": 1
```

| Field                  | Type    | Description                                      |
| ---------------------- | ------- | ------------------------------------------------ |
| `Attribute n name`     | string  | Tên thuộc tính (Color, Size, ...)                |
| `Attribute n value(s)` | string  | Các giá trị, phân cách bằng `,`                  |
| `Attribute n visible`  | boolean | `1` nếu hiển thị ở frontend                      |
| `Attribute n global`   | boolean | `1` nếu dùng lại trong toàn hệ thống WooCommerce |

---

### 🌐 Web Info

| Field    | Type   | Required | Description                         |
| -------- | ------ | -------- | ----------------------------------- |
| `web`    | string | ✅       | Tên hệ thống (ví dụ: YesThatHoodie) |
| `domain` | string | ✅       | Tên miền cần đăng sản phẩm          |

---

## 📤 Example Request Body

```json
{
  "Type": "variable",
  "SKU": "KC-ALOHA-001",
  "Name": "Kansas City Chiefs Hawaiian Shirt",
  "Published": 1,
  "Is featured?": 0,
  "Visibility in catalog": "visible",
  "Short description": "Short intro...",
  "Description": "Long description...",
  "Tax status": "taxable",
  "In stock?": 1,
  "Stock": 100,
  "Backorders allowed?": 0,
  "Sold individually?": 0,
  "Weight (kg)": 0.3,
  "Length (cm)": 30,
  "Width (cm)": 25,
  "Height (cm)": 2,
  "Allow customer reviews?": 1,
  "Regular price": "39.95",
  "Categories": "Hawaiian Shirts > NFL Hawaiian Shirts > Kansas City Chiefs Hawaiian Shirts, Hawaiian Shirts",
  "Images": "https://cdn.site.com/img1.jpg, https://cdn.site.com/img2.jpg",
  "Attribute 1 name": "Color",
  "Attribute 1 value(s)": "Red, White",
  "Attribute 1 visible": 1,
  "Attribute 1 global": 1,
  "Attribute 2 name": "Size",
  "Attribute 2 value(s)": "S, M, L, XL, 2XL",
  "Attribute 2 visible": 1,
  "Attribute 2 global": 1,
  "web": "YesThatHoodie",
  "domain": "yes.thathoodie.com"
}
```

---
