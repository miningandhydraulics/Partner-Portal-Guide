REST API Integration
====================

Overview
--------

The MHS Partner Portal REST API provides programmatic access to all portal functionality, enabling seamless integration with external systems.

**What You Can Do:**

* Search for products and check availability
* Get real-time pricing information
* Create and submit orders programmatically
* Track order status and fulfillment
* Automate routine ordering tasks
* Build custom integrations with your systems

**Integration Scenarios:**

* ERP system connectivity for automated ordering
* Inventory management synchronization
* Real-time stock checking dashboards
* Automated quote generation
* Order status monitoring systems
* Custom reporting and analytics

Prerequisites
-------------

Before using the API, you need:

1. **Active Partner Portal account**
2. **Generated API key** (see :doc:`developer-portal`)
3. **Basic knowledge** of REST APIs and HTTP requests
4. **Development environment** with HTTP client library

Getting Started
---------------

Base URL
~~~~~~~~

All API endpoints use the following base URL:

.. code-block:: text

   https://partner.mhsonline.au/api/v1

Authentication
~~~~~~~~~~~~~~

All requests require Bearer token authentication using your API key:

.. code-block:: http

   Authorization: Bearer mhs_your_32_character_key

**Example Request:**

.. code-block:: bash

   curl -X GET "https://partner.mhsonline.au/api/v1/products/search?q=ABC123" \
     -H "Authorization: Bearer mhs_your_api_key"

Interactive Documentation
~~~~~~~~~~~~~~~~~~~~~~~~~~

Access the full interactive API documentation with live testing:

.. code-block:: text

   https://partner.mhsonline.au/api/v1/docs

This Swagger UI interface allows you to:

* Browse all available endpoints
* See detailed request/response examples
* Test API calls directly from your browser
* View parameter descriptions and validation rules
* Explore response schemas

Common API Operations
---------------------

Search for Products
~~~~~~~~~~~~~~~~~~~

**Endpoint:** ``GET /api/v1/products/search``

**Parameters:**

* ``q`` (required) - Search query (part number or description)
* ``search_type`` (optional) - Search method: ``exact``, ``fuzzy``, or ``description``
* ``limit`` (optional) - Maximum results (default: 10, max: 100)
* ``page`` (optional) - Page number for pagination (default: 1)

**Example Request:**

.. code-block:: bash

   curl -X GET "https://partner.mhsonline.au/api/v1/products/search?q=ABC123&search_type=exact&limit=10" \
     -H "Authorization: Bearer mhs_your_api_key"

**Example Response:**

.. code-block:: json

   {
     "success": true,
     "total": 1,
     "page": 1,
     "limit": 10,
     "results": [
       {
         "part_number": "ABC123",
         "description": "Hydraulic Fitting",
         "qty_available": 50,
         "unit_price": 15.50,
         "image_url": "https://..."
       }
     ]
   }

Get Product Pricing
~~~~~~~~~~~~~~~~~~~

**Endpoint:** ``POST /api/v1/products/pricing``

**Request Body:**

.. code-block:: json

   {
     "part_number": "ABC123",
     "quantity": 10
   }

**Example Request:**

.. code-block:: bash

   curl -X POST "https://partner.mhsonline.au/api/v1/products/pricing" \
     -H "Authorization: Bearer mhs_your_api_key" \
     -H "Content-Type: application/json" \
     -d '{"part_number":"ABC123","quantity":10}'

**Example Response:**

.. code-block:: json

   {
     "success": true,
     "part_number": "ABC123",
     "quantity": 10,
     "unit_price": 15.50,
     "extended_price": 155.00,
     "available_stock": 50,
     "status": "in_stock"
   }

Bulk Pricing Query
~~~~~~~~~~~~~~~~~~

**Endpoint:** ``POST /api/v1/products/pricing/bulk``

**Request Body:**

.. code-block:: json

   {
     "items": [
       {"part_number": "ABC123", "quantity": 10},
       {"part_number": "XYZ789", "quantity": 5},
       {"part_number": "DEF456", "quantity": 15}
     ]
   }

**Example Response:**

.. code-block:: json

   {
     "success": true,
     "total_items": 3,
     "results": [
       {
         "part_number": "ABC123",
         "quantity": 10,
         "unit_price": 15.50,
         "extended_price": 155.00,
         "status": "in_stock"
       }
     ]
   }

Create an Order
~~~~~~~~~~~~~~~

**Endpoint:** ``POST /api/v1/orders``

**Request Body:**

.. code-block:: json

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

**Example Request:**

.. code-block:: bash

   curl -X POST "https://partner.mhsonline.au/api/v1/orders" \
     -H "Authorization: Bearer mhs_your_api_key" \
     -H "Content-Type: application/json" \
     -d @order.json

**Example Response:**

.. code-block:: json

   {
     "success": true,
     "order_id": "uuid-here",
     "order_number": "PO-2025-001",
     "status": "pending_approval",
     "message": "Order created successfully and pending approval"
   }

.. note::
   API-created orders go through the same approval workflow as web orders. Staff will review before processing.

Check Order Status
~~~~~~~~~~~~~~~~~~

**Endpoint:** ``GET /api/v1/orders/{order_number}/status``

**Example Request:**

.. code-block:: bash

   curl -X GET "https://partner.mhsonline.au/api/v1/orders/PO-2025-001/status" \
     -H "Authorization: Bearer mhs_your_api_key"

**Example Response:**

.. code-block:: json

   {
     "success": true,
     "order_number": "PO-2025-001",
     "status": "2",
     "internal_status": "approved",
     "finalised": false,
     "order_date": "2025-11-30T00:00:00Z",
     "has_backorders": false,
     "fulfillment_percentage": 75.0,
     "shipment_status": "Partially Shipped"
   }

Get Order Fulfillment Details
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Endpoint:** ``GET /api/v1/orders/{order_number}/fulfillment``

**Example Response:**

.. code-block:: json

   {
     "success": true,
     "order_number": "PO-2025-001",
     "line_items": [
       {
         "part_number": "ABC123",
         "quantity_ordered": 10,
         "quantity_shipped": 8,
         "quantity_backordered": 2,
         "shipment_details": [
           {
             "tracking_number": "TRACK123",
             "quantity": 8,
             "ship_date": "2025-12-01"
           }
         ]
       }
     ]
   }

Code Examples
-------------

Python Example
~~~~~~~~~~~~~~

**Complete Python Integration:**

.. code-block:: python

   import requests
   import os
   from typing import Dict, List

   API_BASE_URL = "https://partner.mhsonline.au/api/v1"
   API_KEY = os.getenv("MHS_API_KEY")  # Load from environment

   headers = {
       "Authorization": f"Bearer {API_KEY}",
       "Content-Type": "application/json"
   }

   def search_product(part_number: str, search_type: str = "exact") -> Dict:
       """Search for a product by part number."""
       response = requests.get(
           f"{API_BASE_URL}/products/search",
           headers=headers,
           params={"q": part_number, "search_type": search_type}
       )
       response.raise_for_status()
       return response.json()

   def get_pricing(part_number: str, quantity: int) -> Dict:
       """Get pricing for a specific part and quantity."""
       response = requests.post(
           f"{API_BASE_URL}/products/pricing",
           headers=headers,
           json={"part_number": part_number, "quantity": quantity}
       )
       response.raise_for_status()
       return response.json()

   def create_order(order_data: Dict) -> Dict:
       """Create a new order."""
       response = requests.post(
           f"{API_BASE_URL}/orders",
           headers=headers,
           json=order_data
       )
       response.raise_for_status()
       return response.json()

   def get_order_status(order_number: str) -> Dict:
       """Get current status of an order."""
       response = requests.get(
           f"{API_BASE_URL}/orders/{order_number}/status",
           headers=headers
       )
       response.raise_for_status()
       return response.json()

   # Example usage
   if __name__ == "__main__":
       try:
           # Search for a product
           products = search_product("ABC123")
           print(f"Found {products['total']} products")
           
           # Get pricing
           pricing = get_pricing("ABC123", 10)
           print(f"Price: ${pricing['unit_price']}")
           
           # Check order status
           status = get_order_status("PO-2025-001")
           print(f"Order status: {status['internal_status']}")
           
       except requests.exceptions.HTTPError as e:
           print(f"API Error: {e.response.status_code}")
           print(f"Details: {e.response.text}")

JavaScript/Node.js Example
~~~~~~~~~~~~~~~~~~~~~~~~~~

**Complete Node.js Integration:**

.. code-block:: javascript

   const axios = require('axios');

   const API_BASE_URL = 'https://partner.mhsonline.au/api/v1';
   const API_KEY = process.env.MHS_API_KEY;

   const headers = {
     'Authorization': `Bearer ${API_KEY}`,
     'Content-Type': 'application/json'
   };

   // Search for a product
   async function searchProduct(partNumber, searchType = 'exact') {
     try {
       const response = await axios.get(
         `${API_BASE_URL}/products/search`,
         {
           headers,
           params: { q: partNumber, search_type: searchType }
         }
       );
       return response.data;
     } catch (error) {
       console.error('Search failed:', error.response?.data || error.message);
       throw error;
     }
   }

   // Get pricing
   async function getPricing(partNumber, quantity) {
     try {
       const response = await axios.post(
         `${API_BASE_URL}/products/pricing`,
         { part_number: partNumber, quantity },
         { headers }
       );
       return response.data;
     } catch (error) {
       console.error('Pricing failed:', error.response?.data || error.message);
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
       
       const pricing = await getPricing('ABC123', 10);
       console.log(`Price: $${pricing.unit_price}`);
       
       const status = await getOrderStatus('PO-2025-001');
       console.log(`Order status: ${status.internal_status}`);
     } catch (error) {
       process.exit(1);
     }
   })();

Error Handling
--------------

HTTP Status Codes
~~~~~~~~~~~~~~~~~

The API uses standard HTTP status codes:

.. list-table::
   :header-rows: 1
   :widths: 10 20 70

   * - Code
     - Meaning
     - Action
   * - 200
     - Success
     - Process response data
   * - 201
     - Created
     - Resource created successfully
   * - 400
     - Bad Request
     - Check request data format and parameters
   * - 401
     - Unauthorized
     - Verify API key is correct and active
   * - 403
     - Forbidden
     - Check API key permissions
   * - 404
     - Not Found
     - Resource doesn't exist
   * - 422
     - Validation Error
     - Fix validation issues in request
   * - 429
     - Rate Limited
     - Wait and retry (check Retry-After header)
   * - 500
     - Server Error
     - Contact support if persistent

Error Response Format
~~~~~~~~~~~~~~~~~~~~~

All errors return a consistent format:

.. code-block:: json

   {
     "success": false,
     "error": "Error message",
     "detail": "Detailed error description",
     "status_code": 400
   }

Implementing Retry Logic
~~~~~~~~~~~~~~~~~~~~~~~~~

**Python Example with Exponential Backoff:**

.. code-block:: python

   import time
   import requests
   from typing import Optional

   def make_api_request_with_retry(
       url: str,
       headers: dict,
       max_retries: int = 3,
       method: str = "GET",
       **kwargs
   ) -> requests.Response:
       """Make API request with exponential backoff retry logic."""
       
       for attempt in range(max_retries):
           try:
               if method == "GET":
                   response = requests.get(url, headers=headers, **kwargs)
               elif method == "POST":
                   response = requests.post(url, headers=headers, **kwargs)
               
               # Check for rate limiting
               if response.status_code == 429:
                   retry_after = int(response.headers.get('Retry-After', 60))
                   print(f"Rate limited. Retrying after {retry_after}s...")
                   time.sleep(retry_after)
                   continue
               
               # Raise for other HTTP errors
               response.raise_for_status()
               return response
               
           except requests.exceptions.RequestException as e:
               if attempt == max_retries - 1:
                   raise
               
               # Exponential backoff
               wait_time = 2 ** attempt
               print(f"Request failed. Retrying in {wait_time}s...")
               time.sleep(wait_time)
       
       raise Exception("Max retries exceeded")

**JavaScript Example:**

.. code-block:: javascript

   async function makeRequestWithRetry(url, options, maxRetries = 3) {
     for (let attempt = 0; attempt < maxRetries; attempt++) {
       try {
         const response = await axios(url, options);
         return response;
       } catch (error) {
         if (error.response?.status === 429) {
           const retryAfter = error.response.headers['retry-after'] || 60;
           console.log(`Rate limited. Retrying after ${retryAfter}s...`);
           await new Promise(resolve => setTimeout(resolve, retryAfter * 1000));
           continue;
         }
         
         if (attempt === maxRetries - 1) {
           throw error;
         }
         
         const waitTime = Math.pow(2, attempt) * 1000;
         console.log(`Request failed. Retrying in ${waitTime}ms...`);
         await new Promise(resolve => setTimeout(resolve, waitTime));
       }
     }
   }

Rate Limit Monitoring
~~~~~~~~~~~~~~~~~~~~~

**Track Rate Limits in Python:**

.. code-block:: python

   def check_rate_limits(response: requests.Response) -> None:
       """Monitor rate limit headers and warn when approaching limits."""
       
       remaining_minute = int(response.headers.get('X-RateLimit-Remaining-Minute', 60))
       remaining_day = int(response.headers.get('X-RateLimit-Remaining-Day', 10000))
       
       if remaining_minute < 10:
           print(f"⚠️ Warning: Only {remaining_minute} requests remaining this minute")
       
       if remaining_day < 1000:
           print(f"⚠️ Warning: Only {remaining_day} requests remaining today")

Common Integration Workflows
-----------------------------

Workflow 1: ERP Integration for Daily Orders
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Automate order placement from your ERP system:

.. code-block:: python

   import requests
   from datetime import datetime

   def process_daily_orders():
       """Process pending orders from ERP system."""
       
       # 1. Extract orders from your ERP
       erp_orders = get_orders_from_erp()
       
       # 2. Format for MHS API
       api_orders = []
       for order in erp_orders:
           api_orders.append({
               "order_number": order.po_number,
               "delivery_address_id": order.delivery_address_uuid,
               "delivery_type": "Delivery",
               "line_items": [
                   {
                       "part_number": item.part,
                       "quantity": item.qty,
                       "unit_price": item.price,
                       "description": item.description
                   }
                   for item in order.items
               ]
           })
       
       # 3. Submit orders to MHS
       for order_data in api_orders:
           try:
               response = requests.post(
                   f"{API_BASE_URL}/orders",
                   headers=headers,
                   json=order_data
               )
               response.raise_for_status()
               
               result = response.json()
               # 4. Update ERP with submission status
               update_erp_order_status(
                   order_data['order_number'],
                   'submitted_to_mhs',
                   result['order_id']
               )
               
           except Exception as e:
               log_error(f"Failed to submit {order_data['order_number']}: {e}")

Workflow 2: Real-Time Stock Dashboard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Display current stock levels for critical parts:

.. code-block:: python

   import schedule
   import time

   def check_critical_stock():
       """Monitor stock levels for critical parts."""
       
       critical_parts = ['ABC123', 'XYZ789', 'DEF456']
       
       for part in critical_parts:
           try:
               response = requests.get(
                   f"{API_BASE_URL}/products/{part}",
                   headers=headers
               )
               response.raise_for_status()
               
               product = response.json()
               
               # Update dashboard
               update_dashboard(
                   part,
                   product['qty_available'],
                   product['unit_price']
               )
               
               # Alert if low stock
               if product['qty_available'] < 10:
                   send_alert(f"⚠️ Low stock alert: {part} ({product['qty_available']} units)")
                   
           except Exception as e:
               log_error(f"Failed to check stock for {part}: {e}")
   
   # Run every hour
   schedule.every().hour.do(check_critical_stock)
   
   while True:
       schedule.run_pending()
       time.sleep(60)

Workflow 3: Automated Order Status Notifications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Monitor orders and notify team when status changes:

.. code-block:: python

   def monitor_order_status(order_numbers: List[str]):
       """Monitor and notify on order status changes."""
       
       status_cache = {}
       
       for order_num in order_numbers:
           try:
               response = requests.get(
                   f"{API_BASE_URL}/orders/{order_num}/status",
                   headers=headers
               )
               response.raise_for_status()
               
               status = response.json()
               current_status = status['internal_status']
               
               # Check if status changed
               if order_num in status_cache:
                   if status_cache[order_num] != current_status:
                       send_notification(
                           f"Order {order_num} status changed: "
                           f"{status_cache[order_num]} → {current_status}"
                       )
               
               status_cache[order_num] = current_status
               
           except Exception as e:
               log_error(f"Failed to check status for {order_num}: {e}")

Best Practices
--------------

Security
~~~~~~~~

✅ **DO:**

* Store API keys in environment variables
* Use HTTPS only (never HTTP)
* Implement proper error handling and logging
* Rotate API keys every 6 months
* Use read-only keys when write access isn't needed

❌ **DON'T:**

* Never commit API keys to version control
* Don't hardcode keys in source code
* Don't share keys between environments
* Don't log API keys in application logs

Performance
~~~~~~~~~~~

✅ **DO:**

* Use bulk endpoints when processing multiple items
* Implement exponential backoff for retries
* Cache product data when appropriate
* Monitor rate limit headers proactively
* Process large operations during off-peak hours

❌ **DON'T:**

* Don't make unnecessary API calls
* Don't ignore rate limit warnings
* Don't retry immediately after rate limit errors
* Don't fetch the same data repeatedly

Reliability
~~~~~~~~~~~

✅ **DO:**

* Implement circuit breakers for API failures
* Log all API interactions for debugging
* Set appropriate timeouts (30-60 seconds)
* Handle network errors gracefully
* Validate responses before processing

❌ **DON'T:**

* Don't assume API calls always succeed
* Don't ignore error responses
* Don't process invalid response data
* Don't leave requests without timeout

Testing
~~~~~~~

✅ **DO:**

* Create separate API keys for testing
* Test error scenarios (invalid parts, missing fields)
* Verify idempotency for order creation
* Load test before production deployment
* Test rate limiting behavior

❌ **DON'T:**

* Don't test with production API keys
* Don't test order creation with real orders (use test data)
* Don't skip error handling tests
* Don't assume successful responses mean correct behavior

API Reference Quick Links
--------------------------

**Interactive Documentation:**

.. code-block:: text

   https://partner.mhsonline.au/api/v1/docs

**Key Endpoints:**

.. list-table::
   :header-rows: 1
   :widths: 30 10 60

   * - Operation
     - Method
     - Endpoint
   * - Search Products
     - GET
     - ``/products/search?q={query}``
   * - Get Product Details
     - GET
     - ``/products/{part_number}``
   * - Get Pricing
     - POST
     - ``/products/pricing``
   * - Bulk Pricing
     - POST
     - ``/products/pricing/bulk``
   * - Create Order
     - POST
     - ``/orders``
   * - List Orders
     - GET
     - ``/orders``
   * - Get Order Status
     - GET
     - ``/orders/{number}/status``
   * - Get Fulfillment
     - GET
     - ``/orders/{number}/fulfillment``

Troubleshooting
---------------

Common Issues
~~~~~~~~~~~~~

**Problem: 401 Unauthorized errors**

**Solutions:**

* Verify API key is correct and active in :doc:`developer-portal`
* Check Bearer token format: ``Authorization: Bearer mhs_...``
* Confirm key hasn't been revoked
* Ensure key has required permissions for the operation

**Problem: 429 Rate Limited errors**

**Solutions:**

* Implement retry logic with ``Retry-After`` header
* Reduce request frequency
* Use bulk endpoints instead of individual calls
* Contact support for higher limits if needed

**Problem: Orders not appearing in system**

**Solutions:**

* Check API response for order ID
* Verify order status via ``/orders/{number}/status`` endpoint
* Remember: Orders require approval - check ``internal_status``
* Ensure all required fields are included

**Problem: Timeout errors**

**Solutions:**

* Increase HTTP client timeout (60 seconds recommended)
* Check internet connection stability
* Retry with exponential backoff
* Contact support if persistent

**Problem: Invalid response data**

**Solutions:**

* Verify request parameters match API documentation
* Check request body format (valid JSON)
* Ensure part numbers exist in catalog
* Review error messages in response

Getting Help
------------

**API Documentation:**

Interactive documentation with live testing:

.. code-block:: text

   https://partner.mhsonline.au/api/v1/docs

**Support:**

For API integration assistance:

* **Email:** support@mhsonline.au
* **Subject:** "API Integration Support"
* **Include:** API key prefix, error messages, request details

**Related Documentation:**

* :doc:`developer-portal` - Generate and manage API keys
* :doc:`bulk-query` - Bulk querying via web interface
* :doc:`../troubleshooting/common-issues` - General troubleshooting

Next Steps
----------

Build your integration:

1. :doc:`developer-portal` - Create your API key if you haven't already
2. Test API calls using the interactive documentation
3. Implement authentication in your application
4. Start with simple operations (search, pricing)
5. Build up to complex workflows (order creation, monitoring)
6. Implement proper error handling and retry logic
7. Monitor and optimize your integration
