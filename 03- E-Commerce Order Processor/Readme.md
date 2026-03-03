# 🛒 E-commerce Order Processor — Built with n8n

An automated order fulfillment workflow that checks inventory, creates packing lists, and notifies the warehouse team — all triggered the moment a new order comes in.

---

## 📌 What It Does

When a customer places an order, this workflow:
1. **Receives** the order via Webhook (triggered from Hoppscotch / any e-commerce platform)
2. **Splits** the order into individual items using a Code node
3. **Checks stock** for each item against a Google Sheets inventory database
4. **Merges** order data with inventory data
5. **Decides** — if stock is sufficient, creates a fulfillment record and notifies the warehouse. If not, alerts the inventory manager immediately.

---

## 🔁 Workflow Architecture

```
Webhook (receives order)
    ↓
Code Node (splits items array into individual items)
    ↓                          ↓
Google Sheets Lookup     →  Merge Node (joins by SKU)
                                ↓
                            IF Node (stock_available >= quantity_ordered?)
                           ↙                        ↘
                    TRUE ✅                       FALSE ❌
              Write to Fulfillment Queue       Email Inventory Manager
              Send Warehouse Notification      (stock alert)
```

---

## 🧰 Tech Stack

| Tool | Role |
|------|------|
| n8n | Workflow automation engine |
| Webhook | Order trigger |
| Google Sheets | Inventory database + Fulfillment queue |
| Code Node (JS) | Parse and split order items |
| Merge Node | Join order data with inventory data |
| IF Node | Stock availability logic |
| Gmail / SMTP | Notifications and alerts |
| Hoppscotch | Send dummy order data for testing |

---

## 📊 Google Sheets Setup

### Inventory Database (read by n8n)

| sku | product_name | stock_available | reorder_level | warehouse_location |
|-----|-------------|----------------|---------------|-------------------|
| TSHIRT-RED-M | Red T-Shirt (Medium) | 10 | 5 | Shelf A1 |
| JEANS-BLUE-32 | Blue Jeans (32) | 0 | 3 | Shelf B2 |
| SNEAKERS-WHT-42 | White Sneakers (Size 42) | 5 | 2 | Shelf C3 |

### Fulfillment Queue (written by n8n on successful orders)

| order_id | customer_name | customer_address | sku | quantity_ordered | warehouse_location | packing_list | fulfillment_date |
|----------|-------------|-----------------|-----|-----------------|-------------------|-------------|-----------------|

---

## 🚀 How to Run

1. Clone this repo and import the workflow JSON into your n8n instance
2. Set up a **Google Service Account** and share both sheets with it
3. Add your Google Sheets credential in n8n
4. Activate the Webhook node and copy the URL
5. Send a test order via Hoppscotch using the sample payload below
6. Watch the workflow execute end to end!

---

## 📦 Sample Order Payload (Hoppscotch)

```json
{
  "order_id": "ORD-1023",
  "customer": {
    "name": "Ahmed Khan",
    "email": "ahmed.khan@email.com",
    "address": "123 Main Street, Karachi, Pakistan"
  },
  "order_date": "2026-02-27",
  "items": [
    { "sku": "TSHIRT-RED-M", "product_name": "Red T-Shirt (Medium)", "quantity": 2, "price": 19.99 },
    { "sku": "JEANS-BLUE-32", "product_name": "Blue Jeans (32)", "quantity": 1, "price": 49.99 },
    { "sku": "SNEAKERS-WHT-42", "product_name": "White Sneakers (Size 42)", "quantity": 1, "price": 89.99 }
  ],
  "total_amount": 179.96,
  "payment_status": "paid",
  "shipping_method": "express"
}


Built by **Ahmed Khan** as part of an automation engineering assignment.
Feel free to connect on LinkedIn or fork this project!
