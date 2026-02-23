# n8n-product-automation
# 🛒 Product Data Automation — n8n Workflow

An automated workflow built with **n8n** that fetches product data from a public API, stores it in Google Sheets, flags expensive products, sends email alerts, and generates a summary report — all automatically.

---

## 📌 What This Workflow Does

1. **Fetches** product data from [FakeStore API](https://fakestoreapi.com/products)
2. **Stores** all 20 products in a Google Sheet with their details
3. **Classifies** each product using an IF node:
   - Price > $100 → marked as `Expensive`
   - Price ≤ $100 → marked as `Normal`
4. **Sends a Gmail alert** for every expensive product
5. **Generates a summary** in a separate sheet with:
   - Total number of products
   - Average price of all products

---

## 🔧 Nodes Used

| Node | Purpose |
|---|---|
| Manual Trigger | Starts the workflow on demand |
| HTTP Request | Fetches product data from FakeStore API |
| IF | Checks if price > 100 |
| Edit Fields / Set (×2) | Tags each product as Expensive or Normal |
| Merge | Combines both branches back into one stream |
| Google Sheets (Products) | Appends all products with status to the sheet |
| IF | Filters only expensive products for email |
| Gmail | Sends an alert email per expensive product |
| Code (JavaScript) | Calculates total products and average price |
| Google Sheets (Summary) | Writes the summary row to a separate tab |

---

## 🗂️ Google Sheet Structure

**Tab 1 — Products**

| ID | Title | Price | Category | Status |
|---|---|---|---|---|
| 1 | Product Name | 109.95 | electronics | Expensive |
| 2 | Product Name | 22.30 | clothing | Normal |

**Tab 2 — Summary**

| Total Products | Average Price |
|---|---|
| 20 | 55.34 |

---

## ⚙️ Setup Instructions

### Prerequisites
- [n8n](https://n8n.io/) installed locally or via Docker
- Google Cloud account with **Google Sheets API**, **Google Drive API**, and **Gmail API** enabled
- A Gmail account for sending alerts

### Step 1 — Google Cloud Setup
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Enable these APIs:
   - Google Sheets API
   - Google Drive API
   - Gmail API
3. Create OAuth 2.0 credentials (Web Application type)
4. Add redirect URI: `http://localhost:5678/rest/oauth2-credential/callback`

### Step 2 — n8n Credentials
1. In n8n go to **Settings → Credentials**
2. Add **Google Sheets OAuth2** credential
3. Add **Gmail OAuth2** credential

### Step 3 — Google Sheet
1. Create a new Google Sheet
2. Add a tab named **Products** with headers: `ID, Title, Price, Category, Status`
3. Add a tab named **Summary** with headers: `Total Products, Average Price`

### Step 4 — Import Workflow
1. In n8n click **Add Workflow → Import from File**
2. Upload `product_automation_workflow.json`
3. Update the Google Sheets nodes with your spreadsheet
4. Update the Gmail node with your email address

### Step 5 — Run
Click **Execute Workflow** and watch it go! ✅

---

## 📁 Files in This Repo

```
├── product_automation_workflow.json   # Importable n8n workflow
├── README.md                          # This file
```

---

## 🛠️ Built With

- [n8n](https://n8n.io/) — Workflow automation
- [FakeStore API](https://fakestoreapi.com/) — Mock product data
- Google Sheets — Data storage
- Gmail — Email alerts

---

## 👩‍💻 Author

**Fatima** — Built as part of a product data automation practice assignment.

