# API Security Testing Checklist

A practical checklist we use when testing REST and GraphQL APIs, mapped to the **OWASP API Security Top 10 (2023)**.

**68 checks across 9 areas.** Tick each box as you test.

## Contents

1. [Planning & Recon](#1-planning--recon)
2. [Authentication](#2-authentication)
3. [Authorization (BOLA / BFLA)](#3-authorization-bola--bfla)
4. [Data Exposure & Mass Assignment](#4-data-exposure--mass-assignment)
5. [Input Validation & Injection](#5-input-validation--injection)
6. [Business Logic & Resource Limits](#6-business-logic--resource-limits)
7. [SSRF & Third-party APIs](#7-ssrf--third-party-apis)
8. [Configuration & Transport](#8-configuration--transport)
9. [GraphQL (if applicable)](#9-graphql-if-applicable)

## 1. Planning & Recon

### Scope and documentation

- [ ] **REC-01** Collect API documentation (OpenAPI/Swagger, Postman collections, GraphQL schema)
- [ ] **REC-02** Confirm in-scope hosts, API versions and environments (prefer staging)
- [ ] **REC-03** Obtain test accounts for every role, two per role for authorization testing
- [ ] **REC-04** Agree rate limits, testing windows and an emergency contact

### Discovery

- [ ] **REC-05** Map endpoints from client traffic (web, mobile, desktop) through Burp
- [ ] **REC-06** Discover hidden and undocumented endpoints and parameters
- [ ] **REC-07** Look for old or unused API versions (/v1, /v2, /beta, /internal)
- [ ] **REC-08** Check for exposed docs, debug, health, actuator and metrics endpoints
- [ ] **REC-09** Identify API type and technology (REST, GraphQL, SOAP, gRPC, framework, gateway)

**Tools:** Burp Suite · Postman · ffuf · Kiterunner · Swagger UI

## 2. Authentication

### Login and credentials

- [ ] **AUTH-01** Test login for user enumeration through messages, status codes or timing
- [ ] **AUTH-02** Check rate limiting and lockout on login, OTP and password reset
- [ ] **AUTH-03** Test password reset flow (token strength, expiry, reuse, host header)
- [ ] **AUTH-04** Check that every API version enforces the same authentication controls
- [ ] **AUTH-05** Test for default, weak or hard-coded API keys

### Tokens and sessions

- [ ] **AUTH-06** Decode JWTs and review claims (role, user ID, expiry)
- [ ] **AUTH-07** Test JWT 'alg: none' and algorithm confusion (RS256 → HS256)
- [ ] **AUTH-08** Test JWT signature validation and weak signing secrets
- [ ] **AUTH-09** Check token expiry, refresh token rotation and logout invalidation
- [ ] **AUTH-10** Check tokens are not sent in URLs or logs
- [ ] **AUTH-11** Test OAuth flows (redirect_uri validation, state parameter, scopes)

**Tools:** Burp Suite · JWT Editor · jwt_tool · Postman

## 3. Authorization (BOLA / BFLA)

### Object level (API1)

- [ ] **AUTHZ-01** Replace object IDs with another user's IDs in every endpoint (GET, PUT, PATCH, DELETE)
- [ ] **AUTHZ-02** Test IDs in path, query, body, headers and nested JSON
- [ ] **AUTHZ-03** Test predictable / sequential IDs and UUID leakage in other responses
- [ ] **AUTHZ-04** Test bulk and list endpoints for other users' objects

### Function level (API5)

- [ ] **AUTHZ-05** Call admin or privileged endpoints with a normal user token
- [ ] **AUTHZ-06** Switch HTTP methods (GET → PUT/DELETE) on protected resources
- [ ] **AUTHZ-07** Test role changes between tenants or organizations (multi-tenant isolation)
- [ ] **AUTHZ-08** Run Autorize across all roles and review differences

**Tools:** Burp Suite · Autorize · AuthMatrix · Postman

## 4. Data Exposure & Mass Assignment

### Object property level (API3)

- [ ] **DATA-01** Compare response fields with what the UI shows; flag extra sensitive fields
- [ ] **DATA-02** Look for PII, tokens, password hashes or internal IDs in responses
- [ ] **DATA-03** Add unexpected fields to create/update requests (role, isAdmin, balance, status)
- [ ] **DATA-04** Test updating read-only properties (owner, price, createdAt)

### Errors and logs

- [ ] **DATA-05** Trigger errors and check for stack traces, SQL errors or internal paths
- [ ] **DATA-06** Check that sensitive data is not written to logs or analytics

**Tools:** Burp Suite · Postman · Param Miner

## 5. Input Validation & Injection

### Injection

- [ ] **INJ-01** Test SQL injection in all parameters, including JSON values and headers
- [ ] **INJ-02** Test NoSQL injection (operators like $ne, $gt in JSON)
- [ ] **INJ-03** Test OS command injection in parameters passed to system tools
- [ ] **INJ-04** Test XML/XXE in endpoints that accept XML or SOAP
- [ ] **INJ-05** Test server-side template injection where input is rendered

### Content and files

- [ ] **INJ-06** Test content-type confusion (JSON vs XML vs form)
- [ ] **INJ-07** Test file upload endpoints (type, size, path traversal, malicious content)
- [ ] **INJ-08** Test stored XSS through API data later shown in web apps

**Tools:** Burp Suite · sqlmap · NoSQLMap · Postman

## 6. Business Logic & Resource Limits

### Business flows (API6)

- [ ] **BIZ-01** Test sensitive flows for automation abuse (sign-up, purchase, booking, coupons)
- [ ] **BIZ-02** Test price, quantity, discount and refund manipulation
- [ ] **BIZ-03** Skip or repeat steps in multi-step workflows
- [ ] **BIZ-04** Test race conditions on balance, coupon or stock updates

### Resource consumption (API4)

- [ ] **BIZ-05** Check rate limiting per user, IP and API key
- [ ] **BIZ-06** Test large page sizes, limits and payloads
- [ ] **BIZ-07** Test expensive operations (exports, reports, search) for abuse
- [ ] **BIZ-08** Test SMS / email sending endpoints for cost abuse

**Tools:** Burp Intruder · Turbo Intruder · Postman Runner

## 7. SSRF & Third-party APIs

### Server-side request forgery (API7)

- [ ] **SSRF-01** Find parameters that accept URLs, hosts or webhooks
- [ ] **SSRF-02** Test requests to internal IPs, localhost and cloud metadata endpoints
- [ ] **SSRF-03** Test URL parser bypasses and redirects

### Unsafe consumption (API10)

- [ ] **SSRF-04** Review how data from third-party APIs is validated before use
- [ ] **SSRF-05** Check that third-party calls use TLS and timeouts

**Tools:** Burp Collaborator · interactsh

## 8. Configuration & Transport

### Security misconfiguration (API8)

- [ ] **CFG-01** Check TLS versions and ciphers; HTTP should redirect or be blocked
- [ ] **CFG-02** Review CORS policy (wildcards, reflected origins, credentials)
- [ ] **CFG-03** Check security headers (HSTS, Content-Type, cache-control for sensitive data)
- [ ] **CFG-04** Check unnecessary HTTP methods (TRACE, OPTIONS exposure)
- [ ] **CFG-05** Check verbose server banners and version disclosure

### Inventory (API9)

- [ ] **CFG-06** Confirm deprecated versions are retired or protected like current ones
- [ ] **CFG-07** Check non-production APIs are not reachable from the internet

**Tools:** sslscan · Burp Suite · curl

## 9. GraphQL (if applicable)

### GraphQL-specific tests

- [ ] **GQL-01** Check whether introspection is enabled in production
- [ ] **GQL-02** Check field suggestions ('Did you mean') leak schema details
- [ ] **GQL-03** Test query depth, complexity and alias limits
- [ ] **GQL-04** Test batching (arrays of queries) and rate-limit bypass
- [ ] **GQL-05** Test authorization in every resolver, including nested objects
- [ ] **GQL-06** Check GraphiQL / Playground is not publicly exposed

**Tools:** InQL · GraphQL Voyager · graphw00f · Burp Suite

---

*Bugfynd · Cyber Security Testing & Training · contact@bugfynd.com*
