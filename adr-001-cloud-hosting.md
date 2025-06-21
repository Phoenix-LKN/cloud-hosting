# ADR-001: Cloud Hosting Provider for MVP and Beyond

## Status
Accepted

## Context
Phoenix CRM is a modular, dealership-focused CRM platform designed for lead management, sales operations, training, and floor plan financing. The goal of Phase 1 is to launch a working MVP with a fast turnaround using modern tools that are developer-friendly. However, we must also account for post-MVP scalability in case of rapid growth or enterprise interest.

This ADR documents the decision for initial cloud hosting and backend architecture.

## Decision
We will use the following for our MVP infrastructure:

- **Frontend Hosting:** [Vercel](https://vercel.com) (optimized for Next.js)
- **Backend Infrastructure:** [Supabase](https://supabase.com)
  - PostgreSQL (primary datastore)
  - Supabase Auth (RBAC)
  - Supabase Realtime (WebSocket-based updates)
  - Supabase Storage (for file uploads and training material)
  - Edge Functions (custom serverless logic)
- **CI/CD & DevOps:** GitHub Actions (for builds, secrets, and deployment)

## Rationale
We prioritize:
- **Speed to MVP**: Vercel and Supabase offer low-friction deployment and configuration.
- **Developer Velocity**: Supabase provides built-in Auth, Storage, and Realtime features.
- **Native Integration**: Vercel works seamlessly with Next.js, our chosen frontend.
- **Cost Efficiency**: Both platforms offer generous free tiers and transparent pricing.

## Alternatives Considered
| Option | Pros | Cons |
|-------|------|------|
| Firebase + Cloud Run | Battle-tested, global scale | More setup overhead, requires Firebase Auth + Firestore, not ideal for SQL use |
| Heroku + Hasura | Easy Postgres + GraphQL | Heroku platform uncertainty, higher cost at scale |
| AWS Amplify + AppSync | Highly scalable | Complex setup, slower to develop MVP, vendor lock-in |
| Self-hosted Kubernetes | Ultimate flexibility | Not justified for MVP; too complex |

## Consequences
- We are initially limited by **Supabase’s project limits** (row counts, connection limits).
- **Vendor lock-in** may occur with Vercel's proprietary deployment process.
- We’ll be **tied to PostgreSQL** — future migrations to more advanced architectures (e.g., distributed SQL or CQRS) would require planning.
- Supabase pricing can escalate if usage scales rapidly.

## Future Mitigation
To prepare for post-MVP scale:
- Write clean, modular code with minimal platform-specific lock-in
- Use environment variables and centralized config for backend URL and API keys
- Monitor Supabase project limits using built-in analytics
- Plan for optional **migration to self-hosted Supabase** or **managed PostgreSQL (e.g., Neon, RDS)** if limits are hit
- Explore **API caching layers** (e.g., Redis, CDN) to reduce realtime and DB strain

## Related Tasks
- Research Supabase pricing and scaling thresholds
- Evaluate Vercel bandwidth and cold start impact
- Define backup/export strategy for Supabase data
- Add CI/CD workflow in GitHub Actions for production deployment

## Date
2025-06-21
