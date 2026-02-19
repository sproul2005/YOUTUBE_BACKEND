# YouTube Backend – Access Token & Refresh Token System

## 📌 Overview

This backend implements a secure authentication system for a YouTube-like application using **JWT Access Tokens** and **Refresh Tokens**.

The goal is to provide:

* Secure user sessions
* Stateless authentication
* Seamless login experience
* Protection against token theft

---

## 🔐 Token Strategy

### ✅ Access Token

Used for authenticating API requests.

**Properties:**

* Short-lived (e.g., 15 minutes)
* Contains user identification data
* Sent with protected requests

**Usage:**

* Access protected routes
* Verify user identity

---

### ✅ Refresh Token

Used only to generate new access tokens.

**Properties:**

* Long-lived (e.g., 7 days)
* Stored securely (HTTP-only cookie recommended)
* Can be revoked

**Usage:**

* Token renewal
* Session continuation

---

## ⚙️ Authentication Flow

### 🔑 Login Flow

1. User submits credentials
2. Server validates user
3. Server generates:

   * Access Token
   * Refresh Token
4. Tokens sent to client

---

### 🔓 Accessing Protected Routes

Client sends Access Token:

Authorization: Bearer <access_token>

Server:

* Verifies token
* Extracts user data
* Grants/denies access

---

### 🔄 Token Refresh Flow

When Access Token expires:

1. Client calls refresh endpoint
2. Server verifies Refresh Token
3. Server issues new Access Token
4. (Optional) Rotate Refresh Token

---

## 🛣️ Authentication Endpoints

### ✅ Login

`POST /api/v1/users/login`

**Response:**

* accessToken
* refreshToken (cookie or body)

---

### ✅ Refresh Access Token

`POST /api/v1/users/refresh-token`

**Purpose:**

* Generate new Access Token

---

### ✅ Logout

`POST /api/v1/users/logout`

**Purpose:**

* Invalidate Refresh Token
* Clear cookies

---

## 🧠 Why This System is Important in a YouTube Backend

A YouTube-style backend typically has many protected operations:

✔ Upload videos
✔ Like / Comment
✔ Subscribe
✔ Watch history
✔ User profile management

All these actions require **authenticated users**.

Using tokens ensures:

* No server-side sessions needed
* Scalable architecture
* Improved security

---

## 🔒 Protected Route Example

Example: Upload Video

`POST /api/v1/videos/upload`

Requires:
✔ Valid Access Token

Middleware Flow:

1. Extract token from header/cookie
2. Verify JWT
3. Attach user to request
4. Continue controller logic

---

## 🛡️ Security Best Practices

### ✅ Short Access Token Expiry

Example:

* Access Token → 15 minutes
* Refresh Token → 7 days

---

### ✅ Store Tokens Securely

Recommended:

* Access Token → Memory / HTTP-only cookie
* Refresh Token → HTTP-only cookie

Avoid:
🚫 localStorage for Refresh Tokens

---

### ✅ Refresh Token Rotation (Recommended)

Issue a new refresh token whenever used.

Benefits:
✔ Prevent replay attacks
✔ Better session control

---

### ✅ Token Revocation / Blacklisting

Invalidate refresh tokens when:

* Logout
* Password change
* Suspicious activity

---

### ✅ HTTP-only & Secure Cookies

Protect against:
✔ XSS
✔ Token theft

---

## ❌ Common Mistakes in Backend Auth Systems

🚫 Long-lived Access Tokens
🚫 Not validating Refresh Tokens
🚫 Sending tokens in insecure storage
🚫 Not handling token expiry
🚫 No logout invalidation logic

---

## 🧩 Example JWT Payload

Access Token typically contains:

{
"_id": "userId",
"email": "[user@email.com](mailto:user@email.com)",
"username": "creator"
}

---

## ⚙️ Environment Variables (Typical Setup)

JWT_ACCESS_SECRET=
JWT_REFRESH_SECRET=
ACCESS_TOKEN_EXPIRY=15m
REFRESH_TOKEN_EXPIRY=7d

---

## ✅ Benefits for Your YouTube Backend

✔ Stateless authentication
✔ Highly scalable
✔ Industry-standard security model
✔ Smooth user experience
✔ Easy integration with frontend/mobile apps

---

## 🚀 Ideal For

* MERN Stack YouTube Clone
* Video Streaming Platforms
* Scalable REST APIs
* SPA / Mobile Applications

---

**End of Documentation**
