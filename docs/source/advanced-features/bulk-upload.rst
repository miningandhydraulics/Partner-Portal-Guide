Bulk Upload and Ordering
========================

Upload large orders from spreadsheets and process multiple items efficiently with the bulk upload feature.

What is Bulk Upload?
--------------------

Bulk upload allows you to:
- **Upload entire orders** from spreadsheets
- **Process hundreds of parts** at once
- **Validate stock and pricing** in bulk
- **Handle complex ordering** scenarios efficiently

**When to Use Bulk Upload:**
- **Large maintenance orders** with many parts
- **Scheduled maintenance** requiring multiple components
- **Equipment commissioning** with extensive part lists
- **Emergency ordering** requiring rapid processing

**Benefits:**
- **Save time** on large orders
- **Reduce data entry** errors
- **Validate entire orders** before submission
- **Maintain ordering** records and templates

Preparing Your Upload File
--------------------------

**Supported File Formats:**
- **Excel files** (.xlsx, .xls)
- **CSV files** (.csv)
- **Maximum 1000 rows** per upload
- **UTF-8 encoding** recommended for CSV

**Required Columns:**
Your spreadsheet must include these columns:

1. **Part Number** (required)
   - Your part numbers or MHS part numbers
   - Custom mapped part numbers supported
   - One part number per row

2. **Quantity** (required)
   - Numeric quantities only
   - Positive numbers required
   - Decimal quantities supported where applicable

3. **Description** (optional but recommended)
   - Part description for verification
   - Helps identify parts during review
   - Used for validation against catalog

**Example Spreadsheet Format:**

.. code-block:: text

   Part Number    | Quantity | Description
   --------------|----------|------------------
   HYD-123-456   | 5        | Hydraulic Pump Seal
   PUMP-001      | 2        | Main Pump Assembly
   FILTER-789    | 10       | Oil Filter Element
   GASKET-ABC    | 20       | Pump Gasket Set

**Spreadsheet Preparation Tips:**
- **Use consistent** column headers
- **No merged cells** in the data area
- **Remove empty rows** between data
- **Ensure part numbers** are formatted as text
- **Double-check quantities** for accuracy

Upload Process
--------------

**Step-by-Step Upload:**

1. **Access Bulk Upload:**
   - Navigate to "Bulk Upload" in the main menu
   - Or access from dashboard quick actions

2. **Select Your File:**
   - Click "Choose File" or drag and drop
   - Select your prepared spreadsheet
   - Wait for file validation

3. **Map Columns:**
   - System attempts automatic column mapping
   - Verify column assignments are correct
   - Adjust mappings if needed

4. **Review Preview:**
   - Check the first few rows of data
   - Verify part numbers and quantities
   - Look for any formatting issues

5. **Start Processing:**
   - Click "Process Upload"
   - System validates each part
   - Progress indicator shows completion

**Upload Validation:**
During processing, the system:
- **Validates part numbers** against catalog
- **Checks stock availability** for each item
- **Converts custom part** numbers if mapped
- **Calculates pricing** based on your account
- **Identifies any errors** for correction

Understanding Upload Results
----------------------------

**Validation Results Display:**

**Successful Items:**
- ✅ **Valid part numbers** found in catalog
- ✅ **Stock available** or backorder possible
- ✅ **Pricing calculated** correctly
- ✅ **Ready to add** to cart

**Warning Items:**
- ⚠️ **Low stock** or partial availability
- ⚠️ **Alternative parts** suggested
- ⚠️ **Price changes** since last order
- ⚠️ **Special order** items requiring approval

**Error Items:**
- ❌ **Invalid part numbers** not found
- ❌ **Quantity issues** (negative, non-numeric)
- ❌ **Format problems** in data
- ❌ **Access restrictions** for certain parts

**Results Summary:**
- **Total items processed**
- **Successful validations**
- **Items with warnings**
- **Items with errors**
- **Estimated order value**

Handling Upload Errors
----------------------

**Common Error Types:**

**Invalid Part Numbers:**
- **Part not in catalog** - check spelling and format
- **Discontinued parts** - look for alternatives
- **Restricted access** - contact MHS for permissions
- **Custom mapping** needed - set up part number mapping

**Quantity Issues:**
- **Non-numeric values** - ensure quantities are numbers
- **Negative quantities** - use positive numbers only
- **Zero quantities** - remove or correct these rows
- **Decimal precision** - check unit requirements

**Format Problems:**
- **Missing required columns** - add Part Number and Quantity
- **Extra characters** in part numbers - clean data
- **Merged cells** - unmerge and separate data
- **Hidden characters** - check for spaces and special characters

**Error Resolution Process:**
1. **Download error report** with specific issues
2. **Correct problems** in your original spreadsheet
3. **Re-upload the corrected** file
4. **Review results** and repeat if necessary

Adding Validated Items to Cart
------------------------------

**Bulk Add to Cart:**
After successful validation:

1. **Review all validated** items
2. **Select items** to add to cart (or select all)
3. **Modify quantities** if needed
4. **Click "Add Selected to Cart"**
5. **Items added** to your regular cart

**Individual Item Management:**
- **Edit quantities** before adding to cart
- **Remove unwanted** items from the list
- **Add notes** or special instructions
- **Check alternative** parts if suggested

**Cart Integration:**
- **Bulk items merge** with existing cart contents
- **Duplicate parts** combine quantities automatically
- **Regular cart functions** work normally
- **Proceed to checkout** as usual

Advanced Bulk Features
----------------------

**Template Management:**

**Creating Templates:**
- **Save successful uploads** as templates
- **Reuse for recurring** orders
- **Modify templates** for variations
- **Share templates** with team members

**Template Features:**
- **Pre-filled part** numbers and descriptions
- **Default quantities** for standard orders
- **Notes and instructions** included
- **Version control** for template updates

**Batch Processing:**
- **Multiple file uploads** in sequence
- **Combine results** from different uploads
- **Process large orders** in smaller batches
- **Track progress** across multiple uploads

**Integration with Custom Parts:**
- **Use your part numbers** in uploads
- **Automatic conversion** during processing
- **Mixed numbering** schemes supported
- **Validation includes** custom mappings

Mobile Bulk Upload
------------------

**Mobile Capabilities:**
- **File selection** from cloud storage
- **Review validation** results on mobile
- **Add to cart** from mobile device
- **Progress tracking** on small screens

**Mobile Limitations:**
- **File creation** better on desktop
- **Complex editing** easier with full interface
- **Large uploads** may be slower on mobile
- **Detailed review** more effective on desktop

Best Practices
---------------

**File Preparation:**
- **Use templates** for consistent formatting
- **Validate part numbers** before upload
- **Clean data** of extra spaces and characters
- **Test with small** batches first

**Data Management:**
- **Keep backup** copies of upload files
- **Document your** templates and processes
- **Version control** for template changes
- **Regular cleanup** of old files

**Workflow Optimization:**
- **Standard templates** for common orders
- **Batch similar** orders together
- **Pre-validate** part numbers when possible
- **Combine with favorites** for efficiency

**Quality Control:**
- **Double-check quantities** before upload
- **Review validation** results carefully
- **Verify pricing** looks reasonable
- **Test orders** with small quantities first

Troubleshooting
---------------

**Upload Failures:**
- **Check file format** and size limits
- **Verify column** headers and data
- **Try smaller** batch sizes
- **Check internet** connection stability

**Validation Issues:**
- **Review error** messages carefully
- **Check part number** formatting
- **Verify custom** mappings are set up
- **Contact support** for persistent issues

**Performance Problems:**
- **Reduce file size** for faster processing
- **Process during** off-peak hours
- **Check browser** compatibility
- **Clear cache** if experiencing issues

**Data Accuracy:**
- **Verify part numbers** in small batches first
- **Cross-check quantities** with requirements
- **Review pricing** for reasonableness
- **Test templates** before large uploads

Integration with Other Features
-------------------------------

**Custom Part Numbers:**
- **Bulk uploads support** custom part mappings
- **Automatic conversion** during validation
- **Mixed numbering** schemes in single upload
- **Template creation** with your part numbers

**Favorites Integration:**
- **Add validated parts** to favorites
- **Use favorites** to build upload templates
- **Bulk favorites** management
- **Template sharing** via favorites

**Order Management:**
- **Track bulk orders** in order history
- **Reorder using** bulk upload
- **Analytics on** bulk ordering patterns
- **Performance metrics** for large orders

Next Steps
----------

Master bulk ordering workflows:

1. :doc:`part-number-management` - Set up custom part mappings for bulk uploads
2. :doc:`../account-management/favorites` - Create templates from frequently ordered items
3. :doc:`../ordering/checkout-process` - Process large orders efficiently
4. :doc:`../troubleshooting/common-issues` - Resolve bulk upload problems
