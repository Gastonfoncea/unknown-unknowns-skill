# Security Unknown Unknowns Checklist

These are the things developers most commonly don't know they're missing, organized by what they're building.

## Authentication & Authorization
- OAuth state parameter missing → CSRF attacks on login
- JWT stored in localStorage → XSS can steal sessions
- No token rotation → stolen tokens work forever
- Missing rate limiting on login → brute force attacks
- Password reset tokens that don't expire
- No account lockout after failed attempts
- API keys in client-side code or git history
- Missing RBAC/permission checks on API endpoints (not just UI hiding)

## API Security
- No input validation on server side (trusting client validation)
- Missing webhook signature verification (Stripe, GitHub, etc.)
- IDOR vulnerabilities — sequential IDs expose other users' data
- No request size limits → memory exhaustion attacks
- Missing CORS configuration or wildcard CORS
- GraphQL introspection enabled in production
- SQL injection via raw queries (even with ORMs, raw queries sneak in)
- Mass assignment — accepting all fields from request body

## Infrastructure
- Secrets in environment variables readable by any process
- No encryption at rest for databases
- Default credentials on databases, admin panels, caches
- S3 buckets or cloud storage publicly accessible
- No WAF or DDoS protection
- Missing security headers (CSP, HSTS, X-Frame-Options)
- Debug mode / stack traces exposed in production
- No audit logging — can't tell what happened after a breach

## Data Handling
- PII stored without encryption
- Logs containing sensitive data (passwords, tokens, PII)
- No data retention policy — storing everything forever
- Backups not encrypted or not tested for restoration
- Cache containing sensitive data without TTL
- File uploads without type/size validation → malicious files
- User-generated content rendered without sanitization → XSS

## Third-Party Dependencies
- No dependency scanning for known vulnerabilities
- Outdated packages with known CVEs
- Supply chain attacks via compromised npm/pip packages
- Third-party scripts (analytics, ads) with full DOM access
- No SRI (Subresource Integrity) for CDN-loaded scripts

## Common by Project Type

### E-commerce / Payments
- PCI compliance requirements (even with Stripe, there are obligations)
- Webhook replay attacks
- Price manipulation via client-side values
- Inventory race conditions (overselling)

### Mobile Apps
- Certificate pinning missing
- Sensitive data in app sandbox readable on rooted devices
- Deep link hijacking
- API keys extractable from APK/IPA

### SaaS / Multi-tenant
- Tenant isolation failures — data leaking between tenants
- Shared infrastructure side-channel attacks
- Missing tenant context in every database query
- Admin endpoints accessible to regular tenants

### Real-time / WebSocket
- No authentication on WebSocket connections
- Missing message validation
- No rate limiting on messages
- Connection exhaustion attacks
