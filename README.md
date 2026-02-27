Core Project Prompt / Objective  
Design and implement a high-performance, RESTful URL shortening service using Java Spring Boot that:

- Accepts a long URL via a POST request and returns a short alias (e.g., yourdomain.com/abc123 or localhost:8080/abc123).  
- Redirects users from the short alias to the original long URL with low latency (target <100ms for lookups).  
- Generates unique short codes that handle at least 10 million URLs without collisions.  
- Includes basic abuse prevention (e.g., validate URLs, prevent invalid or malicious inputs, avoid redirect loops).  
- Tracks basic analytics (e.g., click count per short URL).  
- Stores data persistently and retrieves it reliably.  

Phase 1 Focused Scope – Build the Minimum Viable Core

Primary Endpoints:
- POST /api/shorten
- Input: JSON body with { "url": "https://very-long-url.com/with/parameters" }
- Output: JSON with the short code/URL (e.g., { "shortUrl": "http://localhost:8080/abc123" })
- GET /{shortCode}
- Behavior: 301 or 302 HTTP redirect to the original URL + increment click count

Phase 2 Enterprise Integration - Upgrade the Codebase to Enterprise-level Viability

- Add Redis for caching redirects (hot keys) and rate limiting basics.
- Implement rate limiting (e.g., Bucket4j library in Java) per IP/user.
- Add analytics endpoint (GET /api/stats/{shortCode}) showing clicks over time.
- Scaling plan: Horizontal scaling, sharding DB by shortCode hash, distributed counter (Redis incr or Snowflake IDs).
- Trade-offs: Counter vs hash for short codes (uniqueness vs length), eventual consistency for analytics.
- Failure modes: What if Redis down? Fallback to DB.
- Future: Kafka for async click events, Prometheus metrics endpoint.