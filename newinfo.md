# MHS Partner Portal - User Guide

**Version 2.0 - December 2025**

Welcome to the enhanced MHS Partner Portal! This guide covers the latest features designed to streamline your ordering process and enable seamless system integration.

---

## Table of Contents

1. [What's New](#whats-new)
2. [Bulk Query Feature](#bulk-query-feature)
3. [Developer Portal](#developer-portal)
4. [REST API Integration](#rest-api-integration)
5. [Common Workflows](#common-workflows)
6. [Best Practices](#best-practices)
7. [Troubleshooting](#troubleshooting)
8. [Quick Reference](#quick-reference)

---

## What's New

The MHS Partner Portal now includes three powerful new features:

### 🔍 Bulk Query
Upload a list of part numbers and instantly check availability, pricing, and stock levels for all items at once. Perfect for quote preparation and large order planning.

### 💻 Developer Portal
Generate and manage API keys for external system integration. Connect your ERP, inventory management, or ordering systems directly to the portal.

### 🔌 REST API
A comprehensive API with full documentation for programmatic access to products, pricing, orders, and order status tracking.

---

## Bulk Query Feature

### Overview

The Bulk Query feature allows you to check multiple part numbers simultaneously, saving time when preparing quotes or planning large orders.

### Getting Started

#### Step 1: Prepare Your File

Create an Excel (`.xlsx`) or CSV file with the following structure:

**Required Columns:**
- `PartNumber` OR `CustomerMaterialNumber` - The part you're searching for
- `Quantity` (optional) - Defaults to 1 if not provided

**Example:**
| PartNumber | Quantity |
|------------|----------|
| ABC123     | 10       |
| XYZ789     | 5        |
| DEF456     | 15       |

**Using Customer Material Numbers:**
| CustomerMaterialNumber | Quantity |
|------------------------|----------|
| CUST001                | 20       |
| CUST002                | 8        |

> **💡 Tip:** Download the template from the Bulk Query page for the correct format.

#### Step 2: Upload Your File

1. Navigate to **User Area → Bulk Query**
2. Drag and drop your file onto the upload area, or click to browse
3. Click **Process Query**
4. Wait while the system processes each part number (this may take a moment for large files)

#### Step 3: Review Results

Once processing is complete, you'll see a comprehensive dashboard showing:

**Summary Statistics:**
- **Total Items** - Total parts in your query
- **In Stock** - Items with sufficient stock to fulfill your quantity
- **Low Stock** - Items with partial stock availability
- **Out of Stock** - Items currently unavailable
- **Not Found** - Parts that couldn't be matched in our system
- **Total Value** - Estimated total cost for all items

**Results Table:**
Each part displays:
- Product image
- Part number (with alternative part if cross-referenced)
- Description
- Requested quantity
- Unit price
- Extended price (unit price × quantity)
- Available stock
- Status badge (color-coded)

#### Step 4: Take Action

**Filter Results:**
Use the filter dropdown to focus on specific stock statuses:
- All Items
- In Stock
- Low Stock
- Out of Stock
- Not Found

**Export Results:**
Click **Export Results** to download an Excel file with all data for record-keeping or sharing.

**Add to Cart:**
Click **Add Available to Cart** to automatically add all in-stock items to your shopping cart. Items will be added with quantities limited to available stock.

**Create Inquiry:**
Click **Create Inquiry** to generate an RFQ for out-of-stock or low-stock items. This will redirect you to the inquiry system with items pre-populated.

**Start New Query:**
Click **New Query** to upload a different file and start over.

### Use Cases

#### Quote Preparation
Upload your customer's parts list to quickly generate pricing and availability information for quote creation.

#### Large Order Planning
Check stock levels before placing a bulk order to identify items that may need lead time or alternatives.

#### Regular Inventory Checks
Maintain a spreadsheet of your frequently ordered items and run weekly queries to stay informed about availability changes.

#### ERP Integration (Manual)
Export your ERP parts list, run through Bulk Query, then import results back into your system.

### Best Practices

✅ **DO:**
- Keep your files organized with clear column headers
- Use the template for guaranteed compatibility
- Export results for your records
- Run queries before large orders
- Create inquiries for unavailable items

❌ **DON'T:**
- Include special characters in part numbers
- Upload files larger than 1000 rows (split into multiple queries)
- Forget to save exported results
- Mix different units in the quantity column

### Limitations

- Maximum 1000 parts per upload
- Supported formats: `.xlsx`, `.xls`, `.csv`
- Processing time: ~1-2 seconds per part
- Pricing shown is current and may vary at time of order

---

## Developer Portal

### Overview

The Developer Portal allows you to create API keys for programmatic access to the MHS Partner Portal, enabling integration with external systems like ERP, WMS, or custom applications.

### Accessing the Developer Portal

1. Log into the Partner Portal
2. Navigate to **User Area → Developer Portal**
3. You'll see two main sections:
   - **API Documentation** - Link to interactive API docs
   - **API Keys** - Manage your authentication keys

### Creating an API Key

#### Step 1: Click "Create API Key"

This opens a modal where you configure your new key.

#### Step 2: Configure Key Settings

**Key Name (Required):**
- Provide a descriptive name (e.g., "Production ERP System", "Test Environment")
- This helps you identify the key's purpose later

**Permissions:**
Choose the appropriate access level:
- **Read Only** - Search products, view pricing, check order status (recommended for dashboards/reports)
- **Read & Write** - All read operations + create orders (recommended for order automation)
- **Admin** - All operations + administrative functions (use sparingly)

**Expires In (Optional):**
- Leave empty for no expiration
- Set days until expiration for enhanced security (e.g., 365 for annual rotation)

#### Step 3: Generate and Save Your Key

1. Click **Generate Key**
2. A modal displays your new API key
3. **⚠️ IMPORTANT:** Copy the key immediately - it's only shown once!
4. Store the key securely (password manager, environment variable, secrets vault)
5. Click **I've Saved My Key** to close the modal

### Managing API Keys

**View Existing Keys:**
All your API keys are listed with:
- Key name
- Key prefix (first 12 characters)
- Permission level
- Active status
- Rate limit
- Creation date
- Last used timestamp (if applicable)

**Revoke a Key:**
1. Click the trash icon next to the key
2. Confirm the action
3. The key is immediately deactivated

> **🔒 Security Note:** Revoked keys cannot be recovered. Any systems using the revoked key will lose access immediately.

### Rate Limits

All API keys have rate limits to ensure fair usage:

**Default Limits:**
- **60 requests per minute**
- **10,000 requests per day**

Rate limit headers are included in all API responses, allowing your application to track remaining quota.

### Security Best Practices

#### Storage
- Never commit API keys to version control (Git, etc.)
- Use environment variables: `export MHS_API_KEY="mhs_your_key_here"`
- For production, use secrets management (AWS Secrets Manager, Azure Key Vault, etc.)

#### Usage
- Create separate keys for different environments (dev, staging, production)
- Use read-only keys wherever possible
- Rotate keys periodically (every 6-12 months)
- Monitor key usage via the "Last used" timestamp

#### Incident Response
If a key is compromised:
1. Immediately revoke it from the Developer Portal
2. Generate a new key with a different name
3. Update your applications with the new key
4. Investigate where the key may have been exposed

---

## REST API Integration

### Overview

The MHS Partner Portal REST API provides programmatic access to all portal functionality, enabling seamless integration with external systems.

### Getting Started

#### Prerequisites

1. Active Partner Portal account
2. Generated API key (see [Developer Portal](#developer-portal))
3. Basic knowledge of REST APIs and HTTP requests

#### Base URL

```
Production: https://partner.mhsonline.au/api/v1
```

#### Authentication

All requests require Bearer token authentication:

```http
Authorization: Bearer mhs_your_32_character_key
```

#### Interactive Documentation

Full API documentation with interactive testing:
```
https://partner.mhsonline.au/api/v1/docs
```

This Swagger UI interface allows you to:
- Browse all available endpoints
- See request/response examples
- Test API calls directly from your browser
- View detailed parameter descriptions

### Common API Operations

#### 1. Search for Products

**Endpoint:** `GET /api/v1/products/search`

**Parameters:**
- `q` - Search query (part number or description)
- `search_type` - `exact`, `fuzzy`, or `description`
- `limit` - Max results (default: 10, max: 100)
- `page` - Page number (default: 1)

**Example:**
```bash
curl -X GET "https://partner.mhsonline.au/api/v1/products/search?q=ABC123&search_type=exact" \
  -H "Authorization: Bearer mhs_your_api_key"
```

#### 2. Get Product Pricing

**Endpoint:** `POST /api/v1/products/pricing`

**Request Body:**
```json
{
  "part_number": "ABC123",
  "quantity": 10
}
```

**Example:**
```bash
curl -X POST "https://partner.mhsonline.au/api/v1/products/pricing" \
  -H "Authorization: Bearer mhs_your_api_key" \
  -H "Content-Type: application/json" \
  -d '{"part_number":"ABC123","quantity":10}'
```

#### 3. Create an Order

**Endpoint:** `POST /api/v1/orders`

**Request Body:**
```json
{
  "order_number": "PO-2025-001",
  "delivery_address_id": "your-address-uuid",
  "allow_partial_delivery": true,
  "delivery_type": "Delivery",
  "order_instructions": "Please deliver between 9 AM - 5 PM",
  "line_items": [
    {
      "part_number": "ABC123",
      "description": "Hydraulic Fitting",
      "quantity": 10,
      "unit_price": 15.50,
      "comment": "Urgent requirement"
    }
  ]
}
```

> **📝 Note:** API-created orders go through the same approval workflow as web orders. Staff will review before processing.

#### 4. Check Order Status

**Endpoint:** `GET /api/v1/orders/{order_number}/status`

**Example:**
```bash
curl -X GET "https://partner.mhsonline.au/api/v1/orders/PO-2025-001/status" \
  -H "Authorization: Bearer mhs_your_api_key"
```

**Response:**
```json
{
  "order_number": "PO-2025-001",
  "status": "2",
  "internal_status": "approved",
  "finalised": false,
  "order_date": "2025-11-30T00:00:00Z",
  "has_backorders": false,
  "fulfillment_percentage": 75.0,
  "shipment_status": "Partially Shipped"
}
```

### Code Examples

#### Python

```python
import requests
import os

API_BASE_URL = "https://partner.mhsonline.au/api/v1"
API_KEY = os.getenv("MHS_API_KEY")  # Load from environment

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

# Search for a product
def search_product(part_number):
    response = requests.get(
        f"{API_BASE_URL}/products/search",
        headers=headers,
        params={"q": part_number, "search_type": "exact"}
    )
    response.raise_for_status()
    return response.json()

# Create an order
def create_order(order_data):
    response = requests.post(
        f"{API_BASE_URL}/orders",
        headers=headers,
        json=order_data
    )
    response.raise_for_status()
    return response.json()

# Get order status
def get_order_status(order_number):
    response = requests.get(
        f"{API_BASE_URL}/orders/{order_number}/status",
        headers=headers
    )
    response.raise_for_status()
    return response.json()

# Example usage
if __name__ == "__main__":
    # Search
    products = search_product("ABC123")
    print(f"Found {products['total']} products")
    
    # Check order
    status = get_order_status("PO-2025-001")
    print(f"Order status: {status['internal_status']}")
```

#### JavaScript/Node.js

```javascript
const axios = require('axios');

const API_BASE_URL = 'https://partner.mhsonline.au/api/v1';
const API_KEY = process.env.MHS_API_KEY;

const headers = {
  'Authorization': `Bearer ${API_KEY}`,
  'Content-Type': 'application/json'
};

// Search for a product
async function searchProduct(partNumber) {
  try {
    const response = await axios.get(
      `${API_BASE_URL}/products/search`,
      {
        headers,
        params: { q: partNumber, search_type: 'exact' }
      }
    );
    return response.data;
  } catch (error) {
    console.error('Search failed:', error.response?.data || error.message);
    throw error;
  }
}

// Create an order
async function createOrder(orderData) {
  try {
    const response = await axios.post(
      `${API_BASE_URL}/orders`,
      orderData,
      { headers }
    );
    return response.data;
  } catch (error) {
    console.error('Order creation failed:', error.response?.data || error.message);
    throw error;
  }
}

// Get order status
async function getOrderStatus(orderNumber) {
  try {
    const response = await axios.get(
      `${API_BASE_URL}/orders/${orderNumber}/status`,
      { headers }
    );
    return response.data;
  } catch (error) {
    console.error('Status check failed:', error.response?.data || error.message);
    throw error;
  }
}

// Example usage
(async () => {
  try {
    const products = await searchProduct('ABC123');
    console.log(`Found ${products.total} products`);
    
    const status = await getOrderStatus('PO-2025-001');
    console.log(`Order status: ${status.internal_status}`);
  } catch (error) {
    process.exit(1);
  }
})();
```

### Error Handling

The API uses standard HTTP status codes:

| Code | Meaning | Action |
|------|---------|--------|
| 200 | Success | Process response |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Check request data |
| 401 | Unauthorized | Verify API key |
| 403 | Forbidden | Check permissions |
| 404 | Not Found | Resource doesn't exist |
| 422 | Validation Error | Fix validation issues |
| 429 | Rate Limited | Wait and retry |
| 500 | Server Error | Contact support |

**Error Response Format:**
```json
{
  "success": false,
  "error": "Error message",
  "detail": "Detailed error description",
  "status_code": 400
}
```

**Implementing Retry Logic:**
```python
import time

def make_api_request_with_retry(url, headers, max_retries=3):
    for attempt in range(max_retries):
        response = requests.get(url, headers=headers)
        
        if response.status_code == 429:
            retry_after = int(response.headers.get('Retry-After', 60))
            print(f"Rate limited. Retrying after {retry_after}s...")
            time.sleep(retry_after)
            continue
        
        return response
    
    raise Exception("Max retries exceeded")
```

### Monitoring API Usage

Check rate limit headers in responses:
```
X-RateLimit-Limit-Minute: 60
X-RateLimit-Limit-Day: 10000
X-RateLimit-Remaining-Minute: 59
X-RateLimit-Remaining-Day: 9999
```

**Example monitoring:**
```python
response = requests.get(url, headers=headers)
remaining = int(response.headers.get('X-RateLimit-Remaining-Minute', 0))

if remaining < 10:
    print(f"⚠️ Warning: Only {remaining} requests remaining this minute")
```

---

## Common Workflows

### Workflow 1: ERP Integration for Daily Orders

**Scenario:** Automatically place orders from your ERP system every morning.

**Steps:**

1. **Generate API Key** (one-time setup)
   - Create a Read & Write key in the Developer Portal
   - Name it "ERP Production System"
   - Store securely in your ERP configuration

2. **Extract Orders from ERP**
   ```python
   # Your ERP export code
   daily_orders = erp_system.get_pending_orders()
   ```

3. **Format for API**
   ```python
   api_orders = []
   for order in daily_orders:
       api_orders.append({
           "order_number": order.po_number,
           "delivery_address_id": order.delivery_address_uuid,
           "delivery_type": "Delivery",
           "line_items": [
               {
                   "part_number": item.part,
                   "quantity": item.qty,
                   "unit_price": item.price
               }
               for item in order.items
           ]
       })
   ```

4. **Submit Orders**
   ```python
   response = requests.post(
       f"{API_BASE_URL}/orders/bulk",
       headers=headers,
       json={"orders": api_orders}
   )
   ```

5. **Handle Response**
   ```python
   if response.ok:
       results = response.json()
       for order in results:
           erp_system.update_order_status(
               order['order_number'],
               'submitted_to_mhs'
           )
   ```

### Workflow 2: Real-Time Stock Dashboard

**Scenario:** Display current stock levels for your critical parts on a dashboard.

**Steps:**

1. **Set Up Scheduled Check**
   ```python
   import schedule
   
   def check_critical_stock():
       critical_parts = ['ABC123', 'XYZ789', 'DEF456']
       
       for part in critical_parts:
           response = requests.get(
               f"{API_BASE_URL}/products/{part}",
               headers=headers
           )
           
           if response.ok:
               product = response.json()
               dashboard.update_stock(
                   part,
                   product['qty_available']
               )
               
               # Alert if low
               if product['qty_available'] < 10:
                   send_alert(f"Low stock: {part}")
   
   # Run every hour
   schedule.every().hour.do(check_critical_stock)
   ```

### Workflow 3: Quote Generation from Customer Request

**Scenario:** Customer sends you a parts list; you need to generate a quote quickly.

**Steps:**

1. **Receive Customer List**
   - Save as Excel file (part numbers + quantities)

2. **Use Bulk Query**
   - Upload file via web interface
   - Review results dashboard
   - Filter to "In Stock" items

3. **Export Results**
   - Click "Export Results"
   - Receive Excel with pricing and availability

4. **Format Quote**
   - Import exported file into your quoting system
   - Add markup and terms
   - Send to customer

5. **Create Inquiry for Missing Items**
   - Click "Create Inquiry" for out-of-stock items
   - Follow up with MHS staff for lead times
   - Update customer quote with extended delivery items

### Workflow 4: Automated Order Status Notifications

**Scenario:** Notify your team when orders are shipped.

**Steps:**

1. **Set Up Monitoring Script**
   ```python
   import time
   
   def monitor_orders(order_numbers):
       order_status_cache = {}
       
       while True:
           for order_num in order_numbers:
               response = requests.get(
                   f"{API_BASE_URL}/orders/{order_num}/fulfillment",
                   headers=headers
               )
               
               if response.ok:
                   fulfillment = response.json()
                   current_status = fulfillment['shipment_status']
                   
                   # Check if status changed
                   if order_num in order_status_cache:
                       if order_status_cache[order_num] != current_status:
                           send_notification(
                               f"Order {order_num} status changed: "
                               f"{order_status_cache[order_num]} → {current_status}"
                           )
                   
                   order_status_cache[order_num] = current_status
           
           time.sleep(300)  # Check every 5 minutes
   ```

---

## Best Practices

### For Bulk Query

#### File Management
- Maintain a template file with correct column headers
- Use descriptive file names with dates: `quote_ABC_Corp_2025-12-09.xlsx`
- Keep a folder of historical queries for reference

#### Data Quality
- Ensure part numbers match your internal naming conventions
- Remove duplicate entries before upload
- Validate quantities are realistic

#### Workflow Integration
- Export results after every query
- Create inquiries immediately for out-of-stock items
- Add in-stock items to cart before they become unavailable

### For API Integration

#### Security
- Rotate API keys every 6 months
- Use environment variables, never hardcode keys
- Implement proper error handling and logging
- Use HTTPS only (never HTTP)

#### Performance
- Use bulk endpoints when possible (e.g., `/orders/bulk` instead of multiple `/orders` calls)
- Implement exponential backoff for retries
- Cache product data when appropriate
- Monitor rate limit headers proactively

#### Reliability
- Implement circuit breakers for API failures
- Log all API interactions for debugging
- Set appropriate timeouts (30-60 seconds for order creation)
- Handle network errors gracefully

#### Testing
- Create a separate API key for testing
- Test error scenarios (invalid parts, missing fields)
- Verify idempotency for order creation
- Load test before production deployment

### For Developer Portal

#### Key Management
- Name keys descriptively (include environment and purpose)
- Document which systems use which keys
- Set expiration dates for enhanced security
- Review and revoke unused keys monthly

#### Monitoring
- Track "Last Used" timestamps regularly
- Investigate keys that haven't been used in 30+ days
- Set up alerts for unusual API usage patterns

---

## Troubleshooting

### Bulk Query Issues

#### Problem: "File format not supported"
**Solution:** Ensure file is `.xlsx`, `.xls`, or `.csv`. Re-save from Excel if needed.

#### Problem: "No data found in file"
**Solution:** Check that column headers are exactly `PartNumber` or `CustomerMaterialNumber` (case-sensitive).

#### Problem: Processing takes too long
**Solution:** Split large files into batches of 500 parts or less.

#### Problem: Many "Not Found" results
**Solution:** 
- Verify part numbers are correct
- Check for extra spaces or special characters
- Try using customer material numbers instead
- Contact MHS support to update cross-references

### API Issues

#### Problem: 401 Unauthorized
**Solutions:**
- Verify API key is correct and active
- Check Bearer token format: `Bearer mhs_...`
- Confirm key hasn't been revoked
- Regenerate key if necessary

#### Problem: 429 Rate Limited
**Solutions:**
- Implement retry logic with `Retry-After` header
- Reduce request frequency
- Use bulk endpoints instead of individual calls
- Contact support for higher limits if needed

#### Problem: Orders not appearing in system
**Solutions:**
- Check API response for order ID
- Verify order status via `/orders/{number}/status` endpoint
- Orders require approval - check internal_status
- Ensure required fields are included (delivery address, line items)

#### Problem: Timeout errors
**Solutions:**
- Increase HTTP client timeout (60 seconds recommended)
- Check internet connection stability
- Retry with exponential backoff
- Contact support if persistent

### Developer Portal Issues

#### Problem: Can't generate API key
**Solution:** Contact MHS support to verify your account has API access enabled.

#### Problem: Lost API key
**Solution:** API keys cannot be recovered. Revoke the lost key and generate a new one.

#### Problem: Key not working after creation
**Solution:**
- Wait 1-2 minutes for key to propagate
- Verify you copied the entire key (including `mhs_` prefix)
- Check key status is "Active" in portal
- Try regenerating the key

---

## Quick Reference

### Bulk Query Cheat Sheet

| Action | Steps |
|--------|-------|
| **Upload File** | User Area → Bulk Query → Drag/drop or click upload |
| **Download Template** | Scroll to bottom → Click "Excel Template" or "CSV Template" |
| **Filter Results** | Use filter dropdown above results table |
| **Export Results** | Click "Export Results" button |
| **Add to Cart** | Click "Add Available to Cart" (in-stock items only) |
| **Create RFQ** | Click "Create Inquiry" (out-of-stock items) |
| **New Query** | Click "New Query" button |

### API Quick Reference

#### Base URL
```
https://partner.mhsonline.au/api/v1
```

#### Authentication Header
```
Authorization: Bearer mhs_your_api_key_here
```

#### Key Endpoints

| Operation | Method | Endpoint |
|-----------|--------|----------|
| Search Products | GET | `/products/search?q={query}` |
| Get Product Details | GET | `/products/{part_number}` |
| Get Pricing | POST | `/products/pricing` |
| Bulk Pricing | POST | `/products/pricing/bulk` |
| Create Order | POST | `/orders` |
| List Orders | GET | `/orders` |
| Get Order Status | GET | `/orders/{number}/status` |
| Get Fulfillment | GET | `/orders/{number}/fulfillment` |

#### HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Rate Limited |
| 500 | Server Error |

#### Rate Limits
- **60** requests per minute
- **10,000** requests per day

### Support Contacts

| Issue Type | Contact |
|------------|---------|
| API Technical Support | support@mhsonline.au |
| Account & Billing | accounts@mhsonline.au |
| Product Inquiries | sales@mhsonline.au |
| API Documentation | https://partner.mhsonline.au/api/v1/docs |
| Portal Issues | Use Contact Form in portal |

### Useful Links

- **Portal Login:** https://partner.mhsonline.au
- **API Documentation:** https://partner.mhsonline.au/api/v1/docs
- **Developer Portal:** https://partner.mhsonline.au/user/developer
- **Bulk Query:** https://partner.mhsonline.au/user/bulk-query

---

## Appendix: File Templates

### Bulk Query Template (Excel/CSV)

**Simple Format:**
```
PartNumber,Quantity
ABC123,10
XYZ789,5
DEF456,15
```

**Using Customer Material Numbers:**
```
CustomerMaterialNumber,Quantity
CUST001,20
CUST002,8
CUST003,12
```

**Mixed Format (NOT recommended, use one or the other):**
```
PartNumber,CustomerMaterialNumber,Quantity
ABC123,,10
,CUST001,20
XYZ789,,5
```

---

## Changelog

### Version 2.0 (December 2025)
- ✨ Added Bulk Query feature
- ✨ Added Developer Portal with API key management
- ✨ Released REST API v1
- 📚 Created comprehensive user guide
- 🔐 Implemented rate limiting and security features

---

**Need Help?**

If you have questions or need assistance with any of these features, please contact our support team at support@mhsonline.au or use the contact form within the portal.

Thank you for using the MHS Partner Portal!
