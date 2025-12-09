Developer Portal
================

Overview
--------

The Developer Portal allows you to create API keys for programmatic access to the MHS Partner Portal, enabling integration with external systems like ERP, WMS, or custom applications.

**What You Can Do:**

* Generate API keys for authentication
* Manage multiple keys for different systems
* Set granular permissions per key
* Monitor key usage and activity
* Revoke compromised or unused keys

**Integration Possibilities:**

* ERP system connectivity
* Inventory management synchronization
* Automated order placement
* Real-time stock checking
* Custom reporting dashboards

Accessing the Developer Portal
-------------------------------

1. Log into the Partner Portal
2. Navigate to **User Area → Developer Portal**
3. You'll see two main sections:

   * **API Documentation** - Link to interactive API docs
   * **API Keys** - Manage your authentication keys

Creating an API Key
-------------------

Step 1: Click "Create API Key"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This opens a modal where you configure your new key.

Step 2: Configure Key Settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Key Name (Required):**

Provide a descriptive name to help identify the key's purpose later.

Examples:

* "Production ERP System"
* "Test Environment"
* "Inventory Dashboard"
* "Order Automation Script"

**Permissions:**

Choose the appropriate access level:

* **Read Only** - Search products, view pricing, check order status
  
  * Recommended for: Dashboards, reports, monitoring systems
  * Cannot: Create or modify orders

* **Read & Write** - All read operations + create orders
  
  * Recommended for: Order automation, ERP integration
  * Can: Search, view, and create orders

* **Admin** - All operations + administrative functions
  
  * Recommended for: Full system integration (use sparingly)
  * Can: All read/write operations + advanced management

.. warning::
   Admin permissions should be used sparingly and only when necessary. Most integrations work well with Read & Write permissions.

**Expires In (Optional):**

Set an expiration time for enhanced security:

* Leave empty for no expiration
* Set days until expiration (e.g., 365 for annual rotation)
* Recommended: 180-365 days for production keys

.. tip::
   Regular key rotation enhances security. Set expiration dates to remind you to rotate keys periodically.

Step 3: Generate and Save Your Key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Click **Generate Key**
2. A modal displays your new API key
3. **⚠️ IMPORTANT:** Copy the key immediately - it's only shown once!
4. Store the key securely (password manager, environment variable, secrets vault)
5. Click **I've Saved My Key** to close the modal

.. danger::
   **CRITICAL SECURITY WARNING**
   
   Your API key will only be displayed ONCE. If you lose it, you must revoke the key and create a new one. Store it securely immediately after generation.

Managing API Keys
-----------------

Viewing Existing Keys
~~~~~~~~~~~~~~~~~~~~~

All your API keys are listed with the following information:

* **Key Name** - The descriptive name you assigned
* **Key Prefix** - First 12 characters (for identification)
* **Permission Level** - Read Only, Read & Write, or Admin
* **Active Status** - Whether the key is currently active
* **Rate Limit** - Requests per minute/day allowed
* **Creation Date** - When the key was generated
* **Last Used** - Timestamp of most recent API call (if applicable)

Revoking a Key
~~~~~~~~~~~~~~

To revoke an API key:

1. Click the trash icon next to the key
2. Confirm the action in the dialog
3. The key is immediately deactivated

.. warning::
   Revoked keys cannot be recovered. Any systems using the revoked key will lose access immediately. Ensure you have updated all applications before revoking active keys.

**When to Revoke:**

* Key has been compromised or exposed
* System using the key is decommissioned
* Key hasn't been used in 90+ days
* Migrating to a new key
* Permission levels need to change (create new key instead)

Rate Limits
-----------

All API keys have rate limits to ensure fair usage and system stability:

**Default Limits:**

* **60 requests per minute**
* **10,000 requests per day**

**Rate Limit Headers:**

All API responses include rate limit information:

.. code-block:: text

   X-RateLimit-Limit-Minute: 60
   X-RateLimit-Limit-Day: 10000
   X-RateLimit-Remaining-Minute: 59
   X-RateLimit-Remaining-Day: 9999

**Handling Rate Limits:**

When you exceed rate limits, you'll receive a ``429 Too Many Requests`` response with a ``Retry-After`` header indicating when you can retry.

.. tip::
   Monitor rate limit headers in your application to avoid hitting limits. Implement exponential backoff for retry logic.

Security Best Practices
-----------------------

Storage
~~~~~~~

**DO:**

✅ Store keys in environment variables:

.. code-block:: bash

   export MHS_API_KEY="mhs_your_key_here"

✅ Use secrets management systems:

* AWS Secrets Manager
* Azure Key Vault
* HashiCorp Vault
* Docker Secrets

✅ Encrypt keys at rest in databases

✅ Use password managers for manual access

**DON'T:**

❌ Never commit API keys to version control (Git, SVN, etc.)

❌ Don't hardcode keys in source code

❌ Don't share keys via email or chat

❌ Don't store keys in plain text files

Usage
~~~~~

**DO:**

✅ Create separate keys for different environments:

* Development
* Staging
* Production

✅ Use read-only keys wherever possible

✅ Rotate keys periodically (every 6-12 months)

✅ Monitor key usage via "Last used" timestamp

✅ Set expiration dates for automatic rotation reminders

**DON'T:**

❌ Don't share keys between applications

❌ Don't use production keys in development

❌ Don't give broader permissions than needed

❌ Don't ignore unused keys (revoke them)

Incident Response
~~~~~~~~~~~~~~~~~

If a key is compromised:

1. **Immediately** revoke it from the Developer Portal
2. **Generate** a new key with a different name
3. **Update** your applications with the new key
4. **Investigate** where the key may have been exposed
5. **Review** security logs for unauthorized access
6. **Document** the incident and remediation steps

.. note::
   Contact MHS support if you suspect unauthorized API access for assistance with security review.

Monitoring and Auditing
------------------------

Key Usage Tracking
~~~~~~~~~~~~~~~~~~

Monitor your API keys regularly:

* Check "Last Used" timestamps monthly
* Investigate keys not used in 30+ days
* Verify usage patterns match expectations
* Review permissions periodically

**Audit Questions:**

* Are all keys still needed?
* Do permission levels match current requirements?
* Have keys been rotated recently?
* Are any keys shared across multiple systems?

Activity Monitoring
~~~~~~~~~~~~~~~~~~~

Track API usage through:

* Rate limit headers in responses
* Application logs showing API calls
* Error rates and patterns
* Response times and performance

Best Practices Summary
----------------------

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Category
     - Best Practice
   * - **Key Creation**
     - Use descriptive names, minimal permissions, set expiration dates
   * - **Storage**
     - Environment variables, secrets managers, never in code
   * - **Usage**
     - Separate keys per environment, monitor regularly
   * - **Rotation**
     - Every 6-12 months, more frequently for high-risk systems
   * - **Monitoring**
     - Check "Last Used" monthly, revoke unused keys
   * - **Security**
     - Revoke immediately if compromised, investigate incidents

Troubleshooting
---------------

Common Issues
~~~~~~~~~~~~~

**Problem: Can't generate API key**

**Solution:** Contact MHS support to verify your account has API access enabled.

**Problem: Lost API key**

**Solution:** API keys cannot be recovered. Revoke the lost key and generate a new one.

**Problem: Key not working after creation**

**Solutions:**

* Wait 1-2 minutes for key to propagate through the system
* Verify you copied the entire key (including ``mhs_`` prefix)
* Check key status is "Active" in portal
* Confirm permissions match required operations
* Try regenerating the key if issues persist

**Problem: 401 Unauthorized errors**

**Solutions:**

* Verify API key is correct and active
* Check Bearer token format: ``Authorization: Bearer mhs_...``
* Confirm key hasn't been revoked
* Ensure key has required permissions for the operation

**Problem: 429 Rate Limited errors**

**Solutions:**

* Implement retry logic with ``Retry-After`` header
* Reduce request frequency
* Use bulk endpoints instead of individual calls
* Contact support for higher limits if needed for legitimate use

Getting Help
------------

**API Documentation:**

Access interactive API documentation at:

.. code-block:: text

   https://partner.mhsonline.au/api/v1/docs

**Support:**

For API-related questions:

* Email: support@mhsonline.au
* Subject: "API Integration Support"
* Include: Key prefix (first 12 chars), error messages, request details

**Resources:**

* :doc:`api-integration` - Complete REST API guide
* :doc:`bulk-query` - Automate bulk queries via API
* :doc:`../troubleshooting/common-issues` - General troubleshooting

Next Steps
----------

Once you have your API key:

1. :doc:`api-integration` - Learn how to use the REST API
2. Read the interactive API documentation
3. Test API calls with your key
4. Implement authentication in your application
5. Build your integration workflow
