Bulk Query Feature
==================

Overview
--------

The Bulk Query feature allows you to check multiple part numbers simultaneously, saving time when preparing quotes or planning large orders. Upload a list of part numbers and instantly check availability, pricing, and stock levels for all items at once.

**Perfect for:**

* Quote preparation and pricing requests
* Large order planning
* Regular inventory checks
* Reviewing customer parts lists

**Key Benefits:**

* Process hundreds of parts in seconds
* Get real-time pricing and availability
* Export results for record-keeping
* Add available items directly to cart
* Create inquiries for unavailable parts

Getting Started
---------------

Step 1: Prepare Your File
~~~~~~~~~~~~~~~~~~~~~~~~~~

Create an Excel (``.xlsx``) or CSV file with the following structure:

**Required Columns:**

* ``PartNumber`` OR ``CustomerMaterialNumber`` - The part you're searching for
* ``Quantity`` (optional) - Defaults to 1 if not provided

**Example Using Part Numbers:**

.. csv-table::
   :header: "PartNumber", "Quantity"
   :widths: 20, 10

   "ABC123", "10"
   "XYZ789", "5"
   "DEF456", "15"

**Example Using Customer Material Numbers:**

.. csv-table::
   :header: "CustomerMaterialNumber", "Quantity"
   :widths: 30, 10

   "CUST001", "20"
   "CUST002", "8"

.. tip::
   Download the template from the Bulk Query page for the correct format.

Step 2: Upload Your File
~~~~~~~~~~~~~~~~~~~~~~~~~

1. Navigate to **User Area → Bulk Query**
2. Drag and drop your file onto the upload area, or click to browse
3. Click **Process Query**
4. Wait while the system processes each part number (this may take a moment for large files)

Step 3: Review Results
~~~~~~~~~~~~~~~~~~~~~~~

Once processing is complete, you'll see a comprehensive dashboard showing:

**Summary Statistics:**

* **Total Items** - Total parts in your query
* **In Stock** - Items with sufficient stock to fulfill your quantity
* **Low Stock** - Items with partial stock availability
* **Out of Stock** - Items currently unavailable
* **Not Found** - Parts that couldn't be matched in our system
* **Total Value** - Estimated total cost for all items

**Results Table:**

Each part displays:

* Product image
* Part number (with alternative part if cross-referenced)
* Description
* Requested quantity
* Unit price
* Extended price (unit price × quantity)
* Available stock
* Status badge (color-coded)

Step 4: Take Action
~~~~~~~~~~~~~~~~~~~~

**Filter Results:**

Use the filter dropdown to focus on specific stock statuses:

* All Items
* In Stock
* Low Stock
* Out of Stock
* Not Found

**Export Results:**

Click **Export Results** to download an Excel file with all data for record-keeping or sharing.

**Add to Cart:**

Click **Add Available to Cart** to automatically add all in-stock items to your shopping cart. Items will be added with quantities limited to available stock.

**Create Inquiry:**

Click **Create Inquiry** to generate an RFQ for out-of-stock or low-stock items. This will redirect you to the inquiry system with items pre-populated.

**Start New Query:**

Click **New Query** to upload a different file and start over.

Use Cases
---------

Quote Preparation
~~~~~~~~~~~~~~~~~

Upload your customer's parts list to quickly generate pricing and availability information for quote creation.

Large Order Planning
~~~~~~~~~~~~~~~~~~~~

Check stock levels before placing a bulk order to identify items that may need lead time or alternatives.

Regular Inventory Checks
~~~~~~~~~~~~~~~~~~~~~~~~~

Maintain a spreadsheet of your frequently ordered items and run weekly queries to stay informed about availability changes.

ERP Integration (Manual)
~~~~~~~~~~~~~~~~~~~~~~~~~

Export your ERP parts list, run through Bulk Query, then import results back into your system.

Best Practices
--------------

**DO:**

✅ Keep your files organized with clear column headers

✅ Use the template for guaranteed compatibility

✅ Export results for your records

✅ Run queries before large orders

✅ Create inquiries for unavailable items

**DON'T:**

❌ Include special characters in part numbers

❌ Upload files larger than 1000 rows (split into multiple queries)

❌ Forget to save exported results

❌ Mix different units in the quantity column

Limitations
-----------

* Maximum 1000 parts per upload
* Supported formats: ``.xlsx``, ``.xls``, ``.csv``
* Processing time: ~1-2 seconds per part
* Pricing shown is current and may vary at time of order

Troubleshooting
---------------

Common Issues
~~~~~~~~~~~~~

**Problem: "File format not supported"**

**Solution:** Ensure file is ``.xlsx``, ``.xls``, or ``.csv``. Re-save from Excel if needed.

**Problem: "No data found in file"**

**Solution:** Check that column headers are exactly ``PartNumber`` or ``CustomerMaterialNumber`` (case-sensitive).

**Problem: Processing takes too long**

**Solution:** Split large files into batches of 500 parts or less.

**Problem: Many "Not Found" results**

**Solutions:**

* Verify part numbers are correct
* Check for extra spaces or special characters
* Try using customer material numbers instead
* Contact MHS support to update cross-references

File Template
-------------

**Simple Excel/CSV Template:**

.. code-block:: text

   PartNumber,Quantity
   ABC123,10
   XYZ789,5
   DEF456,15

**Using Customer Material Numbers:**

.. code-block:: text

   CustomerMaterialNumber,Quantity
   CUST001,20
   CUST002,8
   CUST003,12

.. warning::
   Do NOT mix part numbers and customer material numbers in the same column. Use one or the other.

Integration with Other Features
--------------------------------

**Bulk Upload vs Bulk Query:**

The Bulk Query feature is different from :doc:`bulk-upload`:

* **Bulk Query**: Check pricing and availability without ordering
* **Bulk Upload**: Create and submit orders from spreadsheets

**Custom Part Numbers:**

Bulk Query supports :doc:`part-number-management` integration:

* Use your custom part numbers in queries
* Automatic cross-reference lookup
* Mixed numbering schemes supported

**Favorites Integration:**

* Add frequently queried parts to :doc:`../account-management/favorites`
* Build templates from your favorite items
* Quick access for regular inventory checks

Next Steps
----------

Continue exploring advanced features:

1. :doc:`developer-portal` - Generate API keys for programmatic access
2. :doc:`api-integration` - Automate bulk queries through the REST API
3. :doc:`bulk-upload` - Upload complete orders after validation
4. :doc:`part-number-management` - Set up custom part number mappings
