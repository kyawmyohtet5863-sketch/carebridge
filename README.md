# CareBridge

CareBridge connects academic reporting with student growth, school/home activities, student voice and practical family support for one school. React web serves Administration, Teacher and Family panels; Flutter provides the Family mobile client. These are confirmed planning requirements, not claims of implemented capabilities.

## Confirmed planning report

[Planning report Version 1.1](docs/PANELS_AND_PERMISSIONS_PLAN.md), updated 2026-10-12, defines panel usage, scoped permissions, branding, English/Myanmar support and development exit criteria. School-specific policies require validation.

## Final technology stack

| Layer | Technology |
| --- | --- |
| Web frontend | React, TypeScript, Vite, Tailwind CSS |
| Backend | PHP, Laravel, Laravel Sanctum |
| Database | PostgreSQL |
| Mobile | Flutter and Dart |
| API | REST, JSON, OpenAPI |
| Local environment | Docker Compose |
| Version control | GitHub flow with `main`, `develop`, and feature branches |

## Architecture

```text
React Web  ─────┐
                ├── HTTPS / JSON ── Laravel REST API ── PostgreSQL
Flutter Mobile ─┘                         │
                                         └── Audit logs
```

The Laravel API is the only application layer allowed to access PostgreSQL. Web and mobile clients must not connect directly to the database. Authentication, validation, authorization, report publishing, and other business rules belong in Laravel.

See [Architecture](docs/ARCHITECTURE.md), [Database Design](docs/DATABASE.md), and [API Conventions](docs/API_CONVENTIONS.md).

## Repository layout

```text
carebridge/
├── backend/                  Laravel REST API
├── web/                      React and TypeScript web application
├── mobile/                   Flutter parent mobile application
├── docs/                     Architecture and project documentation
├── .github/                  Pull request and CI configuration
├── docker-compose.yml        Local PostgreSQL environment
├── .env.example              Shared local configuration example
├── CONTRIBUTING.md           Team development rules
└── PROJECT_STRUCTURE.md      Detailed ownership and folder plan
```

## User roles

- **Main Admin:** manages accounts, students, guardians, classes, subjects, assignments, and report publication.
- **Sub-admin:** uses the Administration panel with explicit module/action grants and class/school scope; cannot delegate or elevate permissions.
- **Teacher:** manages attendance and results only for assigned classes and subjects.
- **Parent / Guardian:** views linked children's published academic reports and explicitly shared growth records; contributes observations and family responses.
- **Student:** no direct account in the prototype.

## Confirmed first-release additions

- Activities, interests, achievements, student reflections and agreed support goals.
- Family acknowledgement, context, correction and conversation requests.
- Admin-managed logo/theme with preview, publication, recovery and audited Sub-admin delegation.
- English / မြန်မာ on web/mobile and bilingual report labels; original user-written content remains unchanged.
- Author-private portfolio drafts, explicit audiences and versioned academic report corrections.

See the planning report for deferred features and privacy boundaries.

## Core report workflow

```text
Draft → Submitted → Published → Archived
          │
          └── Returned → Draft
```

Parents can access only `Published` and historical `Archived` reports.

## Getting started

The repository structure and documentation are initialized first. Application frameworks should be generated on machines with PHP/Composer and Flutter installed.

1. Copy `.env.example` to `.env`.
2. Start PostgreSQL with `docker compose up -d database`.
3. Follow [backend/README.md](backend/README.md) to initialize Laravel.
4. Follow [web/README.md](web/README.md) to initialize React and TypeScript.
5. Follow [mobile/README.md](mobile/README.md) to initialize Flutter.
6. Develop new work from `develop` using `feature/<issue>-<short-name>` branches.

## Initial delivery order

1. Requirements, ER diagram, API contract, and user flows
2. Authentication and role/relationship authorization
3. Academic structure and guardian-student links
4. Attendance workflow
5. Results, comments, and report publication
6. Parent web dashboard
7. Flutter parent application
8. Security, integration, acceptance testing, and deployment

## Project status

Confirmed planning and repository foundation. Full implementation, runtime verification and school-policy validation remain outstanding. See [updated roadmap](docs/ROADMAP.md); the local uncommitted starter described in the report is not published by this documentation update.

## License

No open-source license has been selected. All rights are reserved by the CareBridge project team until the team chooses a license.
