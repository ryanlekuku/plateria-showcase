# Architecture

## Product topology

Plateria is organized as a monorepo with distinct application surfaces and shared packages. The separation keeps each interface focused while allowing validation, API clients, UI primitives, translations, and database types to evolve together.

```mermaid
flowchart TB
    subgraph Interfaces
        Ordering[Web ordering]
        Mobile[Mobile app]
        Owner[Owner dashboard]
        KDS[Kitchen display]
        Onboarding[Onboarding portal]
        Admin[Platform admin]
    end

    subgraph Services
        API[Express + TypeScript API]
        Validation[Zod validation]
        Jobs[Background worker]
    end

    subgraph Platform
        DB[(PostgreSQL)]
        Auth[Supabase Auth]
        Storage[Supabase Storage]
        Stripe[Stripe]
        Email[Resend]
    end

    Ordering --> API
    Mobile --> API
    Owner --> API
    KDS --> API
    Onboarding --> API
    Admin --> API
    API --> Validation
    API --> Jobs
    API --> DB
    API --> Auth
    API --> Storage
    API --> Stripe
    API --> Email
```

## Tenant boundary

Restaurant identity is carried through the application and data layers. Database constraints and row-level security provide a second enforcement boundary beneath API authorization. Tenant-isolation tests exercise the two-restaurant case so access controls are verified rather than assumed.

## Controlled AI engineering loop

```mermaid
flowchart LR
    Scope[Human-defined scope] --> Preflight[Repository + PR preflight]
    Preflight --> CI[Exact-head CI evidence]
    CI --> Review[Structured report-only review]
    Review --> Policy{Policy gate}
    Policy -->|No safe action| Stop[Safe stop + evidence]
    Policy -->|Authorized finding| Remediate[Bounded remediation]
    Remediate --> Recheck[CI + re-review]
    Recheck --> Stall{Repeat or round limit?}
    Stall -->|Yes| Stop
    Stall -->|No| Policy
    Policy -->|Resolved| Ready[READY FOR RYAN]
    Ready --> Human[Human decision]
```

The controller binds work to an exact repository, pull request, base commit, head commit, allowed file set, and required check names. Model output does not grant authority: merge, deployment, credentials, and production approval stay outside the automated loop.
