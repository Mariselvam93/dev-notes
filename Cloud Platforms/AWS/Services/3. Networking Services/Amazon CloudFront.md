## 🔷 What is Amazon CloudFront?

**Amazon CloudFront** is a **Content Delivery Network (CDN)** service that securely delivers data, videos, applications, and APIs to users globally with **low latency and high transfer speeds**, leveraging Amazon’s global infrastructure.

---

## 🔷 CloudFront Architecture

* **Edge Locations**: Physical data centers located in major cities worldwide where CloudFront caches content closer to end users.
* **Regional Edge Caches**: Mid-tier caches between edge locations and the origin. They increase cache hit ratio and reduce origin load.
* **Origins**:

  * **Amazon S3** (for static content)
  * **Custom origins** (like EC2, Elastic Load Balancing, or even non-AWS web servers)
* **Distributions**:

  * **Web Distribution**: For websites and APIs (HTTP/S)
  * **RTMP Distribution**: Deprecated; was used for streaming media over RTMP.

---

## 🔷 Key Features

### 1. **Global Edge Network**

* 400+ Points of Presence across 90+ cities in 50+ countries.
* Requests are routed to the nearest edge location.

### 2. **Caching Mechanism**

* **Object Caching**: Stores content closer to users.
* **Cache Control**: Uses headers like `Cache-Control`, `Expires`, `ETag`, and `Last-Modified`.
* **TTL Settings**: You can specify minimum, maximum, and default TTLs per behavior.
* **Invalidations**: Force-refresh content in cache using `CreateInvalidation`.

### 3. **Origin Types**

* **Amazon S3**: Use S3 as a static website or private content store.
* **Custom Origin**: Any HTTP/HTTPS server (EC2, NLB, on-premises).
* **Origin Groups**: Provide failover between multiple origins.

### 4. **Behaviors**

* Define **how CloudFront handles requests**:

  * URL path pattern (e.g., `/images/*`)
  * Caching rules
  * Origin selection
  * Forwarded headers/cookies/query strings
  * Lambda\@Edge/CloudFront Functions triggers

### 5. **Signed URLs and Signed Cookies**

* Restrict access to private content:

  * **Signed URLs**: For individual files
  * **Signed Cookies**: For multiple files behind a single authorization
* Use CloudFront key pairs or a trusted key group with public/private keys.

### 6. **Security**

* **HTTPS Support**: TLS encryption, free AWS-managed ACM certificates.
* **Field-Level Encryption**: Encrypt sensitive fields (e.g., credit card data).
* **Origin Access Control (OAC)** or **Origin Access Identity (OAI)**: Secure S3 content by denying public access and allowing CloudFront only.
* **AWS WAF**: Protect against SQL injection, XSS, and other attacks.
* **AWS Shield**:

  * **Shield Standard**: Free, basic DDoS protection.
  * **Shield Advanced**: Enhanced DDoS protection and monitoring.

---

## 🔷 Integration with Other AWS Services

| Service                                     | Integration                                                            |
| ------------------------------------------- | ---------------------------------------------------------------------- |
| **S3**                                      | Static web hosting and private content delivery                        |
| **EC2 / ALB / NLB**                         | For dynamic content or custom web apps                                 |
| **Lambda\@Edge / CloudFront Functions**     | For running code close to users (auth, header manipulation, redirects) |
| **AWS Certificate Manager (ACM)**           | Free TLS certificates                                                  |
| **AWS WAF / AWS Shield**                    | Web and DDoS protection                                                |
| **Amazon Route 53**                         | DNS-based routing with geo-location awareness                          |
| **CloudWatch**                              | Metrics and logging                                                    |
| **CloudTrail**                              | Auditing and access tracking                                           |
| **S3 Access Logs / CloudFront Access Logs** | Request-level logging                                                  |

---

## 🔷 Pricing

### 1. **Data Transfer Out**

* Charged per GB, based on region.
* First 1 TB per month is free (with Free Tier).

### 2. **Requests**

* Charged per 10,000 HTTP or HTTPS requests.

### 3. **Invalidations**

* First 1,000 paths/month free, then \$0.005 per path.

### 4. **Field-Level Encryption**, **Real-Time Logs**, and **Function executions** (Lambda\@Edge/CloudFront Functions) have additional costs.

> 💡 **Tip for the Exam**: Know how CloudFront charges differ from S3 and understand when caching reduces S3 and EC2 cost.

---

## 🔷 Comparison with Other Solutions

| Feature            | CloudFront                | Azure CDN | Akamai/Cloudflare | S3 Static Website |
| ------------------ | ------------------------- | --------- | ----------------- | ----------------- |
| Native AWS Support | ✅                         | ❌         | ❌                 | ✅                 |
| Dynamic Content    | ✅ (via custom origins)    | ✅         | ✅                 | ❌                 |
| WAF Integration    | ✅ (AWS WAF)               | ✅         | ✅                 | ❌                 |
| Private Access     | ✅ (Signed URLs, OAI/OAC)  | ✅         | ✅                 | ❌                 |
| Edge Computing     | ✅ (Lambda\@Edge, CF Func) | ❌         | ✅ (Workers, etc.) | ❌                 |

---

## 🔷 Best Practices

1. **Use OAC/OAI with S3** to protect private content.
2. **Configure caching headers** (`Cache-Control`, `ETag`) for optimal caching.
3. **Use Signed URLs/Cookies** for controlled access.
4. **Implement WAF** to block common threats.
5. **Use behaviors** to route dynamic/static content differently.
6. **Enable logging** to CloudWatch or S3.
7. **Use Lambda\@Edge** to perform auth, URL rewrites, and header injections.
8. **Group multiple invalidation paths** in a single request when possible.

---

## 🔷 Real-World Use Cases

* **Static website hosting**: Serve React/Angular apps from S3 + CloudFront.
* **API caching**: Reduce load on API Gateway or EC2-hosted APIs.
* **Private media delivery**: Use signed URLs for paid video content.
* **Global app acceleration**: Reduce latency for users in different continents.
* **Authentication and geo-blocking**: With Lambda\@Edge.

---

## 🔷 Performance Optimization Strategies

* **Compress content** using gzip or Brotli.
* **Enable HTTP/2 or HTTP/3** for faster delivery.
* **Use long TTLs** for assets that don’t change often (e.g., versioned JS/CSS).
* **Minimize forwarded headers/cookies** to increase cache hit ratio.
* **Enable Origin Shield** for a central cache layer before the origin.
* **Use Regional Edge Caches** (default for large files) for better caching hierarchy.

---

## 🔷 Troubleshooting Techniques

| Issue                                | Troubleshooting Steps                                            |
| ------------------------------------ | ---------------------------------------------------------------- |
| **Cache Miss**                       | Check TTL, Cache-Control, query strings, forwarded headers.      |
| **403 Forbidden**                    | Ensure OAI/OAC is configured, S3 policy allows CloudFront.       |
| **HTTPS Errors**                     | Validate ACM certificate status, match domain names.             |
| **Content Not Updating**             | Invalidate cache or reduce TTL.                                  |
| **Latency**                          | Enable logging, analyze regional performance, use Origin Shield. |
| **Access Denied to Private Content** | Ensure signed URLs/cookies and trust policies are correct.       |

---

## 🔷 Summary for AWS Exam Focus

| Topic              | Key Points                                        |
| ------------------ | ------------------------------------------------- |
| **Edge Locations** | Bring content closer to users                     |
| **Origins**        | S3, EC2, ALB, Custom HTTP                         |
| **Caching**        | TTLs, headers, invalidations                      |
| **Security**       | HTTPS, WAF, Shield, signed URLs, OAI/OAC          |
| **Integration**    | S3, Lambda\@Edge, ACM, WAF, CloudWatch            |
| **Pricing**        | Data transfer + request-based + invalidations     |
| **Use Cases**      | Static hosting, secure delivery, API acceleration |
| **Best Practices** | Use signed access, optimize TTLs, secure origins  |

---

