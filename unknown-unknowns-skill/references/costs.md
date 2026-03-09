# Costs & Operations Unknown Unknowns Checklist

## Cloud Cost Surprises
- Egress bandwidth costs (AWS charges for data leaving, often the biggest surprise)
- NAT Gateway costs in VPCs — can exceed compute costs
- Idle resources still billing (stopped but not terminated instances, unattached EBS volumes)
- Cross-region data transfer charges
- CloudWatch / logging costs at scale
- Load balancer costs (per-hour + per-request)
- DNS query charges (Route 53 charges per query)
- SSL certificate costs if not using free options (ACM, Let's Encrypt)
- Reserved instances vs on-demand — 40-60% savings if committed
- Spot instances for non-critical workloads — 60-90% savings
- Serverless can be MORE expensive at high throughput than containers
- Auto-scaling without cost caps — surprise bills during traffic spikes
- Development/staging environments running 24/7 when only needed during work hours

## Third-Party Service Costs
- API rate limits and pricing tiers (often hit sooner than expected)
- Stripe fees: 2.9% + 30¢ per transaction adds up fast
- Email service costs at scale (SendGrid, SES, etc.)
- SMS costs for 2FA (especially international)
- CDN costs at video/image scale
- Search service costs (Algolia, Elasticsearch hosting)
- Monitoring tool costs (Datadog can get very expensive)
- Error tracking costs (Sentry pricing tiers)
- Analytics costs if self-hosting (ClickHouse, etc.)

## AI/LLM-Specific Costs
- Token costs scale with usage in ways that are hard to predict
- Long context windows multiply costs dramatically
- Embedding generation costs for RAG systems
- Vector database hosting costs
- Fine-tuning costs and hosting fine-tuned models
- No cost caps by default on most AI APIs — a bug can drain your budget
- Caching AI responses can reduce costs 50-90% for common queries
- Prompt engineering to reduce token usage (shorter system prompts, etc.)

## Operational Blind Spots
- No monitoring → can't detect problems before users report them
- No alerting → problems discovered hours or days late
- No runbooks → every incident is handled ad-hoc
- No on-call rotation → single person is the bottleneck
- No incident post-mortems → same problems repeat
- No SLA defined → unclear what "up" means
- No status page → users have no way to know about outages
- No automated backups verification → backups might be corrupt
- Log retention costs vs log deletion risks
- No cost alerting / budget alarms

## Scaling Operations
- Database connection limits hit before CPU/memory limits
- File descriptor limits on Linux (default 1024, often too low)
- DNS TTL too high → can't failover quickly
- SSL certificate renewal not automated → surprise expiration
- Disk space monitoring — logs and temp files fill disks
- Memory leaks only visible under sustained load
- Time zone handling breaks at international scale
- Character encoding issues (emoji, CJK characters)

## Team & Process
- Bus factor of 1 — only one person knows how to deploy/debug
- No documentation for infrastructure setup
- Secrets management — who has access to production credentials
- No access review process — ex-employees still have access
- No change management for infrastructure changes
- No disaster recovery plan
- No data migration strategy for schema changes
- Technical debt tracking and prioritization

## Hidden Costs of "Free"
- Free tier limits that reset monthly (not annually)
- Free tools that require paid upgrades at scale
- Open source tools with enterprise features behind paywalls
- Self-hosting "free" tools requires infrastructure and maintenance time
- Integration costs — connecting free tools often needs custom code
- Support costs — free tools have community-only support
