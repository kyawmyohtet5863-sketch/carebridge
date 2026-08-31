# CareBridge Architecture

## Decision

CareBridge uses a modular monolith. React and Flutter are independent presentation clients. Laravel is the application and security boundary. PostgreSQL is the single source of truth.

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
| Audit | Security-sensitive and academic-record change history |

## Trust boundaries

1. All external requests are untrusted.
2. Laravel authenticates and authorizes every protected request.
3. A valid role alone is insufficient; teacher assignment and guardian-student relationship checks are mandatory.
4. Parents may receive only published reports.
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
