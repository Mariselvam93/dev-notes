# API Gateway Responsibilities

An **API Gateway** serves as an intermediary between clients and backend services in a microservices architecture. Its responsibilities include:


## 1. Request Routing and Forwarding
- Routes incoming API requests to the appropriate backend service.
- Supports path-based, query-based, or header-based routing.
- **Example**: A request to `GET /users` might be routed to the `UserService`, while a request to `POST /orders` might be routed to the `OrderService`.
- **Diagram**: 

  ```text
  Client → API Gateway → UserService (GET /users)
                       → OrderService (POST /orders)                           
                       → PaymentService

## 2. Authentication and Authorization
- Verifies client credentials (e.g., API keys, OAuth tokens, JWTs) before allowing access to backend services.
- Integrates with identity providers (e.g., OAuth2, OpenID Connect, AWS Cognito, or custom STS).

- **Example**: When a request comes to the API Gateway, it first checks if the request contains a valid JWT token. If not, it returns a 401 Unauthorized response.

``` bash
 curl -X GET "https://api.example.com/orders" -H "Authorization: Bearer <JWT_TOKEN>"
```


## 3. Rate Limiting and Throttling
- Limits the number of requests a client can make within a specified time window to prevent abuse.
- Throttles excessive traffic to avoid overloading backend services.
- **Example**: If a client exceeds 100 requests per minute, the API Gateway returns a `429 Too Many Requests` response.

### Example Flow:
- The client sends a request for an API.
- The API Gateway checks the rate limit for the client's IP address or API key.
- If the rate limit is exceeded (e.g., more than 100 requests in a minute), the gateway responds with a 429 error.

```bash
curl -X GET "https://api.example.com/resource" -H "Authorization: Bearer <JWT_TOKEN>"
```

```json
{
  "status": "error",
  "message": "Rate limit exceeded. Please try again later."
}

```

### NGINX Configuration Example for Throttling:
``` NGINX
http {
    limit_req_zone $binary_remote_addr zone=mylimit:10m rate=10r/m;  # Allow 10 requests per minute per IP

    server {
        listen 80;
        server_name api.example.com;

        location / {
            limit_req zone=mylimit burst=20 nodelay;  # Allow burst of up to 20 requests
            proxy_pass http://backend-service;
            proxy_set_header Authorization $http_authorization;
        }
    }
}

```
In this configuration:

- The limit_req_zone sets a rate of 10 requests per minute.
- The limit_req directive allows a burst of up to 20 requests but limits the rate to prevent overload.

## 4. Load Balancing
- Distributes incoming requests across multiple instances of backend services to ensure high availability and reliability.
- **Example**: The API Gateway sends traffic to multiple instances of OrderService to balance load and reduce latency.
```text
Client → API Gateway → OrderService (Instance 1)
                     → OrderService (Instance 2)
```
## 5. Protocol Translation
- Translates requests between different protocols, such as HTTP to gRPC or WebSocket.
- **Example**: A client may send a gRPC request to the API Gateway, and it will convert it into HTTP requests to reach the backend service.

``` text
Client (gRPC) → API Gateway (HTTP) → UserService
```
## 6. API Composition (Aggregation)
- Aggregates responses from multiple backend services into a single response for the client, reducing the number of client calls.
- **Example**: A request for a user profile could involve fetching data from both the UserService and OrderService, and the API Gateway combines the data into one response.

```json
{
  "user": { "id": 1, "name": "John Doe" },
  "orders": [{ "id": 1, "amount": 100 }]
}
```
## 7. Request and Response Transformation
- Modifies requests before forwarding them to the backend (e.g., adding headers, rewriting URLs).
- Transforms responses before sending them back to clients (e.g., masking sensitive data).
- **Example**: The API Gateway might add an X-Request-ID header to every request to track requests across services.
```bash
curl -X GET "https://api.example.com/users" -H "X-Request-ID: 12345"
```

## 8. Security (Traffic Filtering, SSL Termination)
- Enforces HTTPS connections by handling SSL/TLS termination.
- Filters incoming traffic using IP whitelisting, blacklisting, and firewalls.
- **Example**: The API Gateway terminates SSL at the edge, allowing backend services to communicate over HTTP, reducing load on backend servers.
```
Client → (HTTPS) API Gateway → (HTTP) Backend Service
```
## 9. Monitoring and Logging
- Captures metrics (e.g., request counts, latency, error rates) for observability.
- Logs all requests and responses for audit, debugging, and troubleshooting purposes.
- **Example**: The API Gateway logs requests with details such as IP, endpoint, and response time for future troubleshooting.
```json
{
  "timestamp": "2025-01-05T12:34:56Z",
  "client_ip": "192.168.1.1",
  "endpoint": "/users",
  "response_time": "120ms"
}
```
## 10. Caching
- Caches responses from backend services to reduce latency and improve performance.
- **Example**: The API Gateway can cache a response from a GET /users request for 5 minutes to avoid hitting the backend for each request.

## 11. Request Validation
- Validates request parameters, headers, and payloads against predefined schemas to ensure data integrity before processing.
- **Example**: If a client sends invalid data in the request body, the API Gateway rejects the request with a 400 Bad Request.
```json
{
  "error": "Invalid input",
  "message": "Missing required field 'email'."
}
```
## 12. Cross-Origin Resource Sharing (CORS)
- Handles CORS policies to allow cross-origin requests while ensuring security.
- **Example**: The API Gateway adds appropriate CORS headers to enable requests from different domains.
```text
Access-Control-Allow-Origin: *
```
---

# NGINX as an API Gateway :

Here’s an example of an NGINX configuration to act as an API Gateway:
```nginx
server {
    listen 80;
    server_name api.example.com;

    location /users {
        proxy_pass http://user-service;
        proxy_set_header Authorization $http_authorization;
        proxy_set_header X-Request-ID $request_id;
    }

    location /orders {
        proxy_pass http://order-service;
        proxy_set_header Authorization $http_authorization;
    }

    # SSL Termination (HTTPS)
    listen 443 ssl;
    ssl_certificate /etc/nginx/ssl/api.example.com.crt;
    ssl_certificate_key /etc/nginx/ssl/api.example.com.key;
}
```
In this example:

- Requests to `/users` and `/orders` are routed to different backend services (`user-service` and `order-service`).
- SSL termination is handled by NGINX.
- The `Authorization` header is forwarded to the backend for authentication.
- A custom `X-Request-ID` is included to trace requests.
---
# Summary

An API Gateway consolidates multiple responsibilities that improve the security, performance, and manageability of an API ecosystem. By using a gateway like NGINX, these features (rate limiting, load balancing, security, etc.) can be implemented centrally, ensuring a streamlined flow of traffic between clients and backend services.


