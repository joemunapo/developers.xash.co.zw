# Authentication

The Xash API uses token-based authentication to secure API requests. This page explains how to authenticate your requests to the API.

## Registration and Login Flow

The authentication flow for Xash API consists of the following steps:

1. **Register a user account**
2. **Set a password for the account**
3. **Login to obtain an access token**
4. **Use the access token for API requests**

## API Endpoints

### Register

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/auth/register</div>

Creates a new user account in the system.

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| phone | string | Yes | User's phone number in international format (e.g., 263712345678) |
| first_name | string | Yes | User's first name |
| last_name | string | Yes | User's last name |
| email | string | No | User's email address |

**Example Request:**
```json
{
  "phone": "263712345678",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john.doe@example.com"
}
```

**Example Response:**
```json
{
  "success": true,
  "data": {
    "user_number": "USR12345",
    "phone": "263712345678",
    "message": "Registration successful. Please set your password."
  }
}
```

### Resend User Number

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/auth/resend-user-number/{phone}</div>

Resends the user number to the specified phone number.

**Path Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| phone | string | Yes | User's phone number in international format (e.g., 263712345678) |

**Example Response:**
```json
{
  "success": true,
  "data": {
    "message": "User number sent successfully."
  }
}
```

### Set Password

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/auth/set-password</div>

Sets a password for a newly registered user account.

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| user_number | string | Yes | User number received during registration |
| password | string | Yes | New password (minimum 8 characters) |
| password_confirmation | string | Yes | Confirmation of the new password |

**Example Request:**
```json
{
  "user_number": "USR12345",
  "password": "securePassword123",
  "password_confirmation": "securePassword123"
}
```

**Example Response:**
```json
{
  "success": true,
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "message": "Password set successfully."
}
```

### Login

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/auth/login</div>

Authenticates a user and returns an access token.

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| user_number | string | Yes | User's unique identifier |
| password | string | Yes | User's password |

**Example Request:**
```json
{
  "user_number": "USR12345",
  "password": "securePassword123"
}
```

**Example Response:**
```json
{
  "success": true,
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "message": "Login successful."
}
```

### Logout

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/auth/logout</div>

Invalidates the current user's access token.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "message": "Logout successful."
}
```

### Get Profile

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/profile</div>

Retrieves the authenticated user's profile information.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "phone": "26377712345",
  "first_name": "John",
  "last_name": "Doe",
  "id_number": "63-2345164-V-20",
  "dob": "1990-12-12",
  "email": "email@example.com",
  "is_sub_account": false,
  "user_number": 377323641,
  "business": {
    "id": 107,
    "profile_phone": "263777234567",
    "business_name": "Your Business",
    "business_category": "Technology",
    "bp_number": "000",
    "business_address": {
      "address_line_1": "Your Business Address",
      "address_line_2": null,
      "city": "Your City",
      "country": "Your Country"
    },
    "home_address": {
      "address_line_1": "your address",
      "address_line_2": null,
      "city": "Your City",
      "country": "Your Country"
    }
  },
  "services": [
    {
      "name": "Airtime Voucher",
      "slug": "airtime_voucher",
      "order_by": 1
    },
    {
      "name": "Direct Airtime",
      "slug": "direct_airtime",
      "order_by": 2
    },
    {
      "name": "Data",
      "slug": "data",
      "order_by": 3
    },
    {
      "name": "Electricity",
      "slug": "electricity",
      "order_by": 4
    },
    ....
  ]
}

```

### Create Business

<div class="api-method post">POST</div>
<div class="endpoint">/api/v1/auth/create-business</div>

Creates a business profile for the authenticated user.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Request Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| business_name | string | Yes | Name of the business |
| business_category_id | integer | Yes | ID of the business category |
| business_address | string | Yes | Physical address of the business |
| business_phone | string | Yes | Contact phone number for the business |
| business_email | string | No | Contact email for the business |

**Example Request:**
```json

{
  "address_line_1" : "Acme Corporation",
  "city": "Your City",
  "business_name": "Your Business Name",
  "business_category":"Information Technology",
  "business_address_line_1": "123 Main Street,",
  "business_city":"Your Business City"
}
```

**Example Response:**
```json
{
  "success": true,
  "data": {
    "message": "Business profile created successfully."
  }
}
```

### Get Business Categories

<div class="api-method get">GET</div>
<div class="endpoint">/api/v1/auth/business-categories</div>

Retrieves a list of available business categories.

**Headers:**

| Name | Value | Required |
|------|-------|----------|
| Authorization | Bearer YOUR_ACCESS_TOKEN | Yes |

**Example Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Retail"
    },
    {
      "id": 2,
      "name": "Technology"
    },
    {
      "id": 3,
      "name": "Food & Beverage"
    }
  ]
}
```

## Using the Access Token

After successful login or password setup, you'll receive an access token. Include this token in the Authorization header of all API requests:

```bash
curl -X GET \
  https://api.xash.co.zw/v1/wallet \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...' \
  -H 'Content-Type: application/json'
```

## Token Expiration

Access tokens do not expire automatically but can be invalidated by:
- Logging out using the logout endpoint
- Changing the user's password
- Administrator action

For security reasons, it's recommended to:
1. Store tokens securely
2. Use HTTPS for all API requests
3. Implement proper logout when the user session ends

## Error Handling

Common authentication errors include:

| Error | Description | Resolution |
|-------|-------------|------------|
| Invalid credentials | The user_number or password is incorrect | Verify credentials and try again |
| User not found | The specified user_number doesn't exist | Check the user_number or register a new account |
| Unauthenticated | The access token is invalid or expired | Re-authenticate to obtain a new token |
| Validation error | The request parameters don't meet validation rules | Check the error details and correct the parameters |

For more information on error handling, see the [Error Handling](error-handling.md) section.
