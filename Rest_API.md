# 🌐 REST API & Web Fundamentals

> **Interview-Ready | ML Engineer Focused | Model Serving Context**
> *Questions sourced from backend and MLE interviews 2024–2026*

---

## Table of Contents

1. [HTTP Fundamentals](#1-http-fundamentals)
2. [REST Principles](#2-rest-principles)
3. [HTTP Methods](#3-http-methods)
4. [HTTP Status Codes](#4-http-status-codes)
5. [Authentication & Authorization](#5-authentication--authorization)
6. [API Design Best Practices](#6-api-design-best-practices)
7. [REST vs GraphQL vs gRPC](#7-rest-vs-graphql-vs-grpc)
8. [Model Serving APIs (FastAPI)](#8-model-serving-apis-fastapi)
9. [Performance & Scaling](#9-performance--scaling)
10. [Interview Q&A](#10-interview-qa)

---

## 1. HTTP Fundamentals

### Request Structure

```
GET /api/v1/models/bert-base/predict HTTP/1.1
Host: api.mymlservice.com
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
Content-Type: application/json
Accept: application/json

{
  "text": "This movie was amazing!"
}
```

```
[Method] [Path] [HTTP Version]
[Headers]
[blank line]
[Body]  ← only for POST, PUT, PATCH
```

### Response Structure

```
HTTP/1.1 200 OK
Content-Type: application/json
X-Request-ID: abc123
X-Response-Time: 42ms

{
  "label": "positive",
  "confidence": 0.94,
  "model_version": "1.2.0"
}
```

### HTTP vs HTTPS

```
HTTP:  Plain text. Insecure. Never use for production APIs.
HTTPS: HTTP + TLS/SSL encryption. Encrypts data in transit.
       Prevents eavesdropping, man-in-the-middle attacks.
       Required for any API handling sensitive data.
       Standard port: 443.

TLS handshake process:
  1. Client → Server: "Hello, I support TLS 1.3, here are my cipher suites"
  2. Server → Client: Certificate (contains public key) + chosen cipher
  3. Client verifies certificate with CA (Certificate Authority)
  4. Session keys established → encrypted communication begins
```

---

## 2. REST Principles

> **Interview Q:** *What makes an API RESTful? What are the 6 REST constraints?*

REST = **RE**presentational **S**tate **T**ransfer. An architectural style, not a protocol.

### The 6 REST Constraints

```
1. Client-Server:
   Separation of concerns between UI (client) and data storage (server).
   Client and server evolve independently.

2. Stateless:
   Each request contains ALL information needed to process it.
   Server stores NO session state between requests.
   Client sends auth token, user ID, etc. in EVERY request.
   → Enables horizontal scaling (any server can handle any request).

3. Cacheable:
   Responses must be labelled as cacheable or non-cacheable.
   Caching reduces server load and improves performance.
   HTTP headers: Cache-Control, ETag, Last-Modified.

4. Uniform Interface:
   Standardised communication: resources identified by URLs,
   manipulation via representations (JSON/XML),
   self-descriptive messages, HATEOAS.

5. Layered System:
   Client can't tell if it's talking to the actual server or
   an intermediary (load balancer, CDN, cache, API gateway).

6. Code on Demand (Optional):
   Server can send executable code to client (JavaScript).
   Only optional constraint.
```

### Resources and URIs

```
REST is resource-centric. URLs identify resources (nouns), not actions (verbs).

✓ Good (noun-based, hierarchical):
  GET  /models                     → list all models
  GET  /models/bert-base           → get specific model
  POST /models                     → create a new model
  GET  /models/bert-base/versions  → list versions of a model
  POST /models/bert-base/predict   → predict (action on resource)

❌ Bad (verb-based):
  GET  /getModels
  POST /createModel
  GET  /runPrediction?modelId=bert
```

---

## 3. HTTP Methods

> **Interview Q:** *What is the difference between PUT and PATCH? What does idempotent mean?*

### Methods Reference

| Method | Purpose | Body | Idempotent | Safe |
|---|---|---|---|---|
| GET | Retrieve resource | No | ✓ Yes | ✓ Yes |
| POST | Create resource | Yes | ❌ No | ❌ No |
| PUT | Replace entire resource | Yes | ✓ Yes | ❌ No |
| PATCH | Partial update | Yes | Usually | ❌ No |
| DELETE | Delete resource | Optional | ✓ Yes | ❌ No |
| HEAD | Like GET but no body | No | ✓ Yes | ✓ Yes |
| OPTIONS | Get allowed methods | No | ✓ Yes | ✓ Yes |

### Idempotency Explained

```
Idempotent: Calling the same operation N times has the same effect as calling it once.

GET /models/bert         → Always returns same model. Idempotent. ✓
DELETE /models/bert      → First call deletes it. Second call: already deleted.
                           State is the same either way. Idempotent. ✓
PUT /models/bert {data}  → Sets model to {data}. Same result every time. ✓
POST /models {data}      → Each call creates a NEW model. NOT idempotent. ❌

Why it matters: Idempotent methods are safe to retry on network failure.
Your client can retry a DELETE safely; retrying POST might duplicate data.
```

### PUT vs PATCH

```json
Current resource:
{
  "name": "bert-base",
  "version": "1.0",
  "status": "active",
  "accuracy": 0.94
}

PUT /models/bert-base  → REPLACE ENTIRE RESOURCE
{
  "name": "bert-base",
  "version": "1.1",
  "status": "active",
  "accuracy": 0.94
}
// Must send ALL fields. Missing fields might be set to null.

PATCH /models/bert-base  → PARTIAL UPDATE
{
  "version": "1.1"
}
// Only update specified fields. Other fields unchanged.
```

---

## 4. HTTP Status Codes

> **Interview Q:** *What status code would you return if a user sends invalid input? If they're not authorised? If the resource doesn't exist?*

### Essential Status Codes

```
2xx — Success
  200 OK:              Request succeeded. Standard success for GET, PUT, PATCH, DELETE.
  201 Created:         Resource successfully created. Return after POST. Include Location header.
  202 Accepted:        Request accepted for async processing (not yet done).
  204 No Content:      Success but no response body. Common for DELETE.

3xx — Redirection
  301 Moved Permanently: Resource permanently moved to new URL. Update your bookmarks.
  302 Found:             Temporary redirect. Use current URL again next time.
  304 Not Modified:      Cached version is still valid. Don't re-download. (ETag matched)

4xx — Client Errors (client's fault)
  400 Bad Request:       Invalid request syntax, missing fields, validation error.
  401 Unauthorized:      Not authenticated. Send credentials. (confusing name — means "unauthenticated")
  403 Forbidden:         Authenticated but not authorized. You don't have permission.
  404 Not Found:         Resource doesn't exist.
  405 Method Not Allowed: HTTP method not supported on this endpoint.
  409 Conflict:          Resource conflict (e.g., duplicate creation).
  422 Unprocessable:     Syntactically valid but semantically wrong (FastAPI uses this for validation).
  429 Too Many Requests: Rate limit exceeded. Include Retry-After header.

5xx — Server Errors (server's fault)
  500 Internal Server Error: Unhandled exception. Generic server error.
  502 Bad Gateway:           Upstream server returned invalid response (proxy/LB issue).
  503 Service Unavailable:   Server temporarily down (overloaded or maintenance).
  504 Gateway Timeout:       Upstream server took too long to respond.
```

### ML API Status Code Decision Tree

```
User sends text for sentiment analysis:
  → Missing "text" field:            400 Bad Request
  → No API key provided:             401 Unauthorized
  → API key valid but no ML access:  403 Forbidden
  → Model "gpt-5" doesn't exist:     404 Not Found
  → 100 requests/minute exceeded:    429 Too Many Requests
  → Model throws NaN error:          500 Internal Server Error
  → GPU server down:                 503 Service Unavailable
  → Success:                         200 OK
```

---

## 5. Authentication & Authorization

> **Interview Q:** *What is the difference between authentication and authorization? Explain JWT.*

```
Authentication: Verifying WHO you are. (Login — "Are you really Alice?")
Authorization:  Verifying WHAT you can do. (Permissions — "Can Alice access this model?")

AuthN → AuthZ (authentication must come before authorization)
```

### API Keys

```
Simplest authentication method. Client sends a secret key with each request.

Header: X-API-Key: sk_live_abc123xyz
  or
Header: Authorization: ApiKey sk_live_abc123xyz

Pros:  Simple, stateless, easy to revoke per key.
Cons:  No expiry by default, no user identity, if leaked must rotate.
Use:   Server-to-server ML API calls, simple developer APIs.
```

### JWT — JSON Web Token

> **Interview Q:** *What is a JWT? What are its three parts?*

```
JWT = Header.Payload.Signature   (three Base64URL parts separated by dots)

Example:
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJ1c2VySWQiOiIxMjMiLCJyb2xlIjoibWwtZW5naW5lZXIiLCJleHAiOjE3MDAwMDAwMDB9.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

Part 1 - Header (Base64URL encoded):
{
  "alg": "HS256",   // signing algorithm
  "typ": "JWT"
}

Part 2 - Payload (Base64URL encoded):
{
  "userId": "123",
  "role": "ml-engineer",
  "exp": 1700000000,    // expiry timestamp
  "iat": 1699990000     // issued at timestamp
}

Part 3 - Signature:
  HMAC_SHA256(base64(header) + "." + base64(payload), secret_key)
  → Verifies the token wasn't tampered with.

Verification: Server recomputes signature using its secret key.
              If it matches → token valid. If not → tampered, reject.

Key properties:
  ✓ Stateless: server doesn't need to store session
  ✓ Self-contained: payload carries user info
  ✗ Cannot be revoked before expiry (use short expiry + refresh tokens)
  ✗ Payload is Base64 encoded, NOT encrypted — don't store secrets in it!
```

```python
# FastAPI JWT example
import jwt
from datetime import datetime, timedelta

SECRET_KEY = "your-secret-key"
ALGORITHM = "HS256"

def create_token(user_id: str) -> str:
    payload = {
        "sub": user_id,
        "exp": datetime.utcnow() + timedelta(hours=24),
        "iat": datetime.utcnow()
    }
    return jwt.encode(payload, SECRET_KEY, algorithm=ALGORITHM)

def verify_token(token: str) -> dict:
    return jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
    # Raises jwt.ExpiredSignatureError or jwt.InvalidTokenError if invalid
```

### OAuth 2.0

```
Framework for delegated authorization. "Login with Google/GitHub."

Flow (Authorization Code):
  1. User clicks "Login with Google"
  2. Your app redirects to Google's auth server
  3. User logs in to Google and grants permission
  4. Google redirects back with an authorization CODE
  5. Your server exchanges code for access_token (server-to-server, secure)
  6. Use access_token to access Google APIs on behalf of user

Key terms:
  Resource Owner:    The user
  Client:            Your application
  Authorization Server: Google/GitHub/Okta
  Resource Server:   The API you want to access (Google Drive, GitHub repos)
  Access Token:      Short-lived token for API access
  Refresh Token:     Long-lived token to get new access tokens

In ML: Used when your training pipeline accesses user data from Google Drive,
       or when your app has "Sign in with GitHub" to access user's repos.
```

---

## 6. API Design Best Practices

### Versioning

```
Why: When you change your API, old clients break.
     Versioning lets old and new clients coexist.

Option 1 — URL versioning (most common):
  /api/v1/models
  /api/v2/models

Option 2 — Header versioning:
  GET /api/models
  Accept: application/vnd.myapi.v2+json

Option 3 — Query parameter:
  GET /api/models?version=2

Recommendation: URL versioning for public APIs (explicit, easy to test in browser).
```

### Pagination

```python
# Offset-based pagination (simple but slow for deep pages)
GET /models?page=5&limit=20
# SQL: LIMIT 20 OFFSET 100

# Keyset/Cursor pagination (better for large datasets and ML logs)
GET /experiments?after=exp_id_xyz&limit=20
# SQL: WHERE id > 'exp_id_xyz' ORDER BY id LIMIT 20
# Much faster — no need to skip N rows

# Response format
{
  "data": [...],
  "pagination": {
    "total": 1000,
    "page": 5,
    "limit": 20,
    "next": "/models?page=6&limit=20",
    "has_more": true
  }
}
```

### Rate Limiting

```
Prevent API abuse. Protect server resources.

Common strategies:
  Fixed Window:    100 requests per minute. Resets at :00.
  Sliding Window:  100 requests in any 60-second window.
  Token Bucket:    Tokens refill at rate r. Each request costs 1 token.
                   Allows bursts (drain the bucket) then rate-limits.

Headers to return:
  X-RateLimit-Limit: 100
  X-RateLimit-Remaining: 42
  X-RateLimit-Reset: 1700000060
  Retry-After: 30      ← return this with 429
```

### Error Response Format

```json
// Consistent error format
{
  "error": {
    "code": "INVALID_INPUT",
    "message": "The 'text' field is required and must be a string",
    "field": "text",
    "request_id": "req_abc123",
    "documentation": "https://docs.myapi.com/errors#INVALID_INPUT"
  }
}
```

### CORS — Cross-Origin Resource Sharing

```
Browser security policy: JavaScript can only make requests to the SAME origin
(same protocol + domain + port) by default.

CORS: Server tells the browser which other origins are allowed.

Request from https://myapp.com to https://api.myapp.com:
  Browser sends: Origin: https://myapp.com
  Server responds: Access-Control-Allow-Origin: https://myapp.com

Preflight request (OPTIONS):
  For non-simple requests (PUT, DELETE, custom headers),
  browser first sends OPTIONS to check if the actual request is allowed.

In FastAPI:
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://myapp.com"],
    allow_methods=["GET", "POST"],
    allow_headers=["Authorization", "Content-Type"],
)
```

---

## 7. REST vs GraphQL vs gRPC

> **Interview Q:** *When would you choose gRPC over REST for model serving?*

| | REST | GraphQL | gRPC |
|---|---|---|---|
| Protocol | HTTP/1.1 | HTTP/1.1 or 2 | HTTP/2 |
| Format | JSON (usually) | JSON | Protocol Buffers (binary) |
| Typing | Loose | Strongly typed schema | Strongly typed (proto) |
| Over/Underfetching | Common | Solved | N/A (procedure calls) |
| Streaming | Limited | Subscriptions | Bidirectional streaming |
| Browser support | Native | Native | Needs grpc-web |
| Performance | Medium | Medium | Fastest (binary + HTTP/2) |
| Use case | Public APIs, simple services | Complex graphs of data | Internal microservices, real-time |

```
ML Serving choices:

REST (FastAPI): General purpose. Easy to test. Good for external APIs.
                Use when: Model API for end users, simple request-response.

gRPC:           High performance internal service.
                Use when: High-throughput inference (thousands req/sec),
                          streaming predictions, microservice-to-microservice.
                Real examples: TensorFlow Serving, Triton Inference Server.

GraphQL:        Flexible queries for complex data.
                Use when: Experiment tracking UI (query exactly what you need),
                          building developer portals.
```

---

## 8. Model Serving APIs (FastAPI)

```python
from fastapi import FastAPI, HTTPException, Depends, Header
from pydantic import BaseModel, validator
from typing import Optional
import numpy as np

app = FastAPI(title="ML Inference API", version="1.0.0")

# Request/Response models with validation
class PredictionRequest(BaseModel):
    text: str
    model_version: Optional[str] = "latest"
    max_length: Optional[int] = 512

    @validator('text')
    def text_must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError('text cannot be empty')
        return v

    @validator('max_length')
    def max_length_must_be_positive(cls, v):
        if v <= 0:
            raise ValueError('max_length must be positive')
        return v

class PredictionResponse(BaseModel):
    label: str
    confidence: float
    model_version: str
    latency_ms: float

# Simple API key auth
async def verify_api_key(x_api_key: str = Header(...)):
    if x_api_key != "expected-key":
        raise HTTPException(status_code=401, detail="Invalid API key")
    return x_api_key

# Endpoints
@app.get("/health")
async def health_check():
    """Health check for load balancer."""
    return {"status": "healthy", "model_loaded": model is not None}

@app.post("/v1/predict", response_model=PredictionResponse)
async def predict(
    request: PredictionRequest,
    api_key: str = Depends(verify_api_key)
):
    """Run inference on input text."""
    import time
    start = time.time()

    try:
        result = model.predict(request.text)
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"Model error: {str(e)}")

    latency = (time.time() - start) * 1000

    return PredictionResponse(
        label=result["label"],
        confidence=result["confidence"],
        model_version=request.model_version,
        latency_ms=latency
    )

@app.get("/v1/models")
async def list_models():
    return {"models": ["bert-base", "roberta-large"]}
```

### Async for ML Serving

```python
import asyncio
from concurrent.futures import ThreadPoolExecutor

executor = ThreadPoolExecutor(max_workers=4)

@app.post("/v1/predict")
async def predict_async(request: PredictionRequest):
    # Run CPU/GPU-bound inference in thread pool (don't block event loop)
    loop = asyncio.get_event_loop()
    result = await loop.run_in_executor(
        executor,
        lambda: model.predict(request.text)  # blocking call in thread pool
    )
    return result
```

---

## 9. Performance & Scaling

### Caching Strategies

```
Client-side caching:    Browser/client caches response.
                        HTTP headers: Cache-Control, ETag, Expires.

Server-side caching:    Cache responses in Redis/Memcached.
                        Cache prediction results for repeated identical inputs.

CDN caching:            Cache static assets at edge locations.

In ML serving:
  Input hash → cached prediction (semantic cache)
  Model weights cached in GPU memory (not reloaded per request)
  Feature vectors cached in Redis for real-time recommendations
```

### Load Balancing

```
Distributes requests across multiple server instances.

Algorithms:
  Round Robin:    Request 1→Server1, Request 2→Server2, Request 3→Server3...
  Least Connections: Route to server with fewest active connections.
  IP Hash:        Same client always goes to same server (session affinity).
  Weighted:       Servers with more resources get more traffic.

For ML serving:
  Stateless inference servers → round robin works perfectly.
  GPU servers are expensive → route based on GPU availability.
  Use health checks to remove failed servers automatically.
```

---

## 10. Interview Q&A

**Q: What is idempotency and why does it matter?**
> Idempotency means calling the same operation multiple times has the same effect as calling it once. GET, PUT, DELETE are idempotent; POST is not. This matters for retry logic — if a network request times out, you don't know if it succeeded. Retrying a GET or DELETE is safe. Retrying a POST could create duplicate records (duplicate payment, duplicate order). For ML: retrying a prediction request (GET/POST) is usually safe if the server is stateless.

**Q: What is the difference between 401 and 403?**
> **401 Unauthorized** (confusingly named) means "not authenticated" — the client hasn't provided valid credentials or the token is missing/expired. Client should log in and retry. **403 Forbidden** means "authenticated but not authorized" — the server knows who you are, but you don't have permission for this resource. Don't retry — you need different permissions.

**Q: How would you design a REST API for model serving?**
> Design with versioning (`/api/v1/`), resource-based URLs (`/models/{model_id}/predict`), proper HTTP methods (POST for prediction), appropriate status codes (200 for success, 422 for invalid input, 503 if model unavailable), authentication via API key or JWT header, rate limiting headers, async processing for long-running inference with a job ID returned (202 Accepted), and health check endpoint (`/health`) for load balancers.

**Q: What is the difference between synchronous and asynchronous API?**
> **Synchronous**: Client sends request and waits (blocking). Response comes immediately. Good for fast operations (<1 second). Example: real-time sentiment analysis. **Asynchronous**: Client sends request, server returns a job ID immediately (202 Accepted). Client polls for result (`GET /jobs/{id}`) or gets notified via webhook. Good for slow operations. Example: fine-tuning a model, batch inference on 10,000 rows.

**Q: What is a webhook?**
> A webhook is a reverse API call — instead of client polling server for updates, the server calls the client when an event happens. Client registers a callback URL. Server sends HTTP POST to that URL when the event occurs. Example: "Call my URL when the model training job finishes." More efficient than polling. Used in MLOps for training completion notifications, CI/CD pipeline triggers, payment confirmations.

**Q: What is API Gateway and why is it used?**
> An API Gateway sits in front of all your backend services and handles cross-cutting concerns: authentication, rate limiting, SSL termination, logging, routing, request transformation, versioning. Clients talk to one endpoint (the gateway); it routes to the right internal service. Examples: AWS API Gateway, Kong, Nginx. In ML: gateway routes `/predict` to inference service, `/train` to training service, `/data` to data service.

**Q: What is the difference between REST and WebSocket?**
> **REST** is request-response — client initiates, server responds, connection closes. Stateless. Good for CRUD operations. **WebSocket** is a persistent, bidirectional connection — both sides can send messages anytime after the initial HTTP handshake. Good for real-time use cases: live model training metrics dashboard, streaming token-by-token LLM output, real-time trading signals. LLM streaming responses (ChatGPT-style) use Server-Sent Events (SSE) or WebSocket.

---

*Sources: DataCamp REST API Interview Guide 2026, InterviewBit REST API Questions, Merge.dev REST API Interview Questions, BrowserStack REST API Interview Questions 2025.*
