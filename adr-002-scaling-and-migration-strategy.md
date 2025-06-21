# ADR-002: Scaling and Migration Strategy (Post-MVP)

## Status
Accepted

## Context
The Phoenix CRM MVP is built on a modern serverless stack (Vercel + Supabase) to enable rapid development. However, our long-term vision includes owning and operating our infrastructure to support:

- Cost optimization at scale
- Enhanced control and security
- Flexibility for custom compliance or integrations
- Reduced vendor dependency

This ADR documents our planned migration path once usage outgrows the MVP stack.

## Decision
We will begin development with:
- **Vercel** for frontend hosting
- **Supabase** for backend (Postgres, Auth, Realtime, Edge Functions)

But we will design Phoenix to be **portable and self-hostable** post-MVP.

## Migration Path

| Component | MVP (Now) | Long-Term (Post-MVP) |
|----------|-----------|----------------------|
| Frontend | Vercel | Self-hosted Next.js or Cloudflare Pages |
| Database | Supabase PostgreSQL | Self-hosted PostgreSQL (DO, RDS, Hetzner, K8s) |
| Auth | Supabase Auth | Auth.js, Clerk, or Keycloak |
| Realtime | Supabase Realtime | Redis Pub/Sub or NATS |
| Storage | Supabase Storage | S3-compatible (MinIO, R2, etc.) |
| Server Functions | Supabase Edge | Node.js/Golang microservices |

## Rationale
- **Speed to market**: Use Supabase + Vercel for MVP to ship quickly.
- **Scalability**: Prepared to migrate components as usage or cost increases.
- **Vendor independence**: No long-term lock-in; can move pieces gradually.

## Consequences
- Requires disciplined abstraction of services (e.g., auth, storage, sockets)
- Adds migration overhead later, but minimizes upfront complexity
- Early design choices (RBAC, DB schema) must align with long-term platform plans

## Mitigation
- Use environment-driven config for all external services
- Structure backend as modular API services (replaceable)
- Regularly export Supabase DB and Auth data
- Document customizations and dependencies clearly

## Follow-ups
- Create a service abstraction plan (AuthService, FileService, etc.)
- Document pg_dump / restore process for PostgreSQL
- Track Supabase usage metrics in dev and staging

## Date
2025-06-21
