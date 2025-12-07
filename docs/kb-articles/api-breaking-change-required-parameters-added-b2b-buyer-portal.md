# API Breaking Change: Required Parameters Added - b2b-buyer-portal

# API Breaking Change: Required Parameters Added

**Quick Summary:** This article provides solutions for users facing validation errors due to newly introduced required parameters in API requests.

**Type:** Troubleshooting Guide | **Difficulty:** Intermediate | **Estimated Time:** 10-15 minutes

## Applies To

- **Product/Repository:** b2b-buyer-portal
- **Language/Framework:** TypeScript
- **Versions:** All versions affected
- **Environment:** Both Development and Production

## Symptoms

Users experiencing this issue may observe:

- **Error Messages:**
  ```
  422 Unprocessable Entity: Missing required parameters.
  ```

- **Behavior:**
  - API requests that once succeeded are now failing.
  - Users see validation errors without clear guidance on missing fields.

- **Impact:**
  - Failed interactions with the API, hindering user operations and integrations that depend on successful requests.

## Root Cause

### Technical Explanation

The introduction of new required parameters in API requests has led to validation errors when these parameters are not included. This is likely due to updates in the API definition that enforce stricter validation rules.

**Key factors:**
- New mandatory fields added to existing API endpoints.
- The API's validation logic now rejects requests that fail to provide these fields.
- Potentially outdated client implementations that do not account for recent API changes.

## Resolution

### Migration Guide (For Code-Level Changes)

**IMPORTANT:** If your issue stems from code changes, follow the migration guide below:

#### Before (Old Code/Config):
```typescript
const response = await apiClient.post('/orders', {
  // Missing new required fields
  customerId: 123,
  products: [/* product data */]
});
```

#### After (New Code/Config):
```typescript
const response = await apiClient.post('/orders', {
  customerId: 123,
  products: [/* product data */],
  shippingAddress: {/* new required field */}, // New required field
  paymentMethod: {/* new required field */} // Another new required field
});
```

#### What Changed:
- `shippingAddress` field is now required.
- `paymentMethod` field is now required.
- Other additional fields may also be required as per updated API specifications.

### Step-by-Step Fix

**Method 1: Update API Requests**

Follow these steps to resolve the issue:

1. **Identify Required Fields**
   - Check the API documentation for the latest requirements for requests to relevant endpoints.

2. **Modify Your API Calls**
   ```typescript
   const orderPayload = {
     customerId: 123,
     products: [/* product data */],
     shippingAddress: {/* appropriate address data */}, // Include the new required shipping address
     paymentMethod: {/* appropriate payment method data */} // Include the new required payment method
   };

   const response = await apiClient.post('/orders', orderPayload);
   ```

3. **Test the Changes**
   ```bash
   curl -X POST 'https://api.yourdomain.com/orders' \
   -H 'Content-Type: application/json' \
   -d '{ "customerId": 123, "products": [...], "shippingAddress": {...}, "paymentMethod": {...}}'
   ```

**Expected Result:**
The API should successfully process the request without returning a validation error.

**Verification:**
```bash
curl -i -H 'Authorization: Bearer YOUR_TOKEN' https://api.yourdomain.com/orders
# Check that the status code is 200 and includes the order detail.
```

**Method 2: Use Default Values for New Parameters** *(If immediate update is not feasible)*

If you must retain old functionality for a short period, you can implement default handling:

1. **Fallback Defaults**
   - Set sensible defaults within your application for new fields until you can make the necessary updates.

```typescript
const defaultShippingAddress = { /* default address values */ };
const defaultPaymentMethod = { /* default payment values */ };

const orderPayload = {
  customerId: 123,
  products: [/* product data */],
  shippingAddress: userProvidedAddress || defaultShippingAddress,
  paymentMethod: userProvidedPayment || defaultPaymentMethod
};
```

## Workarounds

If the above fix cannot be applied immediately:

**Temporary Solution:**
- Inform users about the new required fields through API documentation.
- Implement conditional logic to handle missing parameters and return a user-friendly error message.

**Note:** This is a temporary measure. Implement the full resolution when possible.

## Prevention & Best Practices

To avoid this issue in the future:

1. **Stay Updated with API Documentation**
   - Regularly review API documentation for changes to required parameters.

2. **Implement Client-side Validation**
   - Add validation checks in your client applications that guard against sending incomplete data to the API.

3. **Version Control Your API Calls**
   - Use specific API version endpoints to maintain control over breaking changes when upgrading.

## References

- **Related GitHub Issues:** [GitHub issue links to relevant discussions]
- **Pull Requests:** [Direct links to PRs that implemented the changes]
- **Documentation:** [Link to relevant documentation about the API]
- **Related Articles:** [Other KB articles that explain similar issues]

## Related Articles

- [API Rate Limiting Changes]
- [Updating API Clients for Breaking Changes]

## Support Escalation

If this article does not resolve your issue:

1. **Gather the following information:**
   - Full error message and stack trace
   - Environment details (OS, versions, configurations)
   - Steps to reproduce the issue
   - Logs from API consumption points

2. **Contact Support:**
   - Submit via [email support, ticket system, etc.].
   - Include all information from step 1.
   - Reference this KB article for faster assistance.

*Last Updated: October 2023*  
*Article ID: B2B-12345*