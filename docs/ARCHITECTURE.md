# CareBridge Architecture

## Decision

CareBridge uses a modular monolith. React and Flutter are independent presentation clients. Laravel is the application and security boundary. PostgreSQL is the single source of truth.

## Confirmed target scope

[Planning report Version 1.1](PANELS_AND_PERMISSIONS_PLAN.md) defines three panels and four roles: Main Admin, restricted Sub-admin, Teacher and Guardian. Shared English/Myanmar localization and controlled branding apply to web/mobile. This is target architecture, not a deployed-feature claim.

## Context

```text
Administrator ─┐
Teacher ───────┼── React Web ──────┐
Parent ────────┘                    │
                                    ├── Laravel REST API ── PostgreSQL
Parent ─────────── Flutter Mobile ──┘           │
                                                └── Audit log
```

## Backend modules

| Module | Responsibility |
| --- | --- |
| Authentication and Users | Sessions, tokens, account status, password reset, and roles |
| Academic Structure | Academic years, classes, sections, subjects, and assignments |
| Students and Guardians | Student records and verified guardian-student relationships |
| Attendance | Daily status entry, correction rules, and summaries |
| Results and Reports | Marks, comments, approval, publication, and history |
| Growth Portfolio | Activities, interests, student voice, author-private drafts and explicit sharing audiences |
| Support and Family Requests | Agreed goals, acknowledgement, context, correction and conversation requests |
| Branding and Localization | Published branding versions, en/my labels and personal language preferences |
| Audit | Security-sensitive and academic-record change history |

## Trust boundaries

1. All external requests are untrusted.
2. Laravel authenticates and authorizes every protected request.
3. A valid role alone is insufficient; teacher assignment and guardian-student relationship checks are mandatory.
4. Guardians may receive only published/archived academic reports and growth records explicitly shared with their verified relationship.
5. PostgreSQL credentials remain server-side.
6. Real student data must not be used in public demonstrations without approval and anonymization.

## Why a modular monolith

- One deployment and database are manageable for a senior-project team.
- Business modules remain clearly separated.
- Transactions are simpler for result publication.
- Testing and local development require less infrastructure than microservices.
- Modules can be separated later if proven scale requires it.

## API versioning

All public application endpoints begin with `/api/v1`. Breaking changes require a new major API version or an agreed migration period.

## Additional permission and settings boundaries

Sub-admin access requires both a capability and record scope on every API request; self-elevation and further delegation are forbidden. Portfolio drafts are author-private, including against automatic administrator access. Academic review/publication and branding edit/publication are separate capabilities. Theme and language changes never grant data access. Published academic records use versioned corrections. Both clients use the same published branding and authorization rules, with safe configuration fallbacks.
