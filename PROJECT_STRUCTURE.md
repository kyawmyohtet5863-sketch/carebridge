# CareBridge Project Structure

## Top-level ownership

| Directory | Owner | Purpose |
| --- | --- | --- |
| `backend/` | Backend and database developer | Laravel API, authorization, business rules, migrations, and backend tests |
| `web/` | Web frontend developer | React administrator, teacher, and parent dashboards |
| `mobile/` | Mobile developer | Flutter parent application |
| `docs/` | Project lead and QA/documentation | Architecture, requirements, API, database, workflow, and test documentation |
| `.github/` | Project lead | Pull request templates, ownership, and automation |

## Backend target structure

```text
backend/
├── app/
│   ├── Http/
│   │   ├── Controllers/Api/V1/
│   │   ├── Middleware/
│   │   ├── Requests/
│   │   └── Resources/
│   ├── Models/
│   ├── Policies/
│   ├── Services/
│   └── Modules/
│       ├── Authentication/
│       ├── Users/
│       ├── AcademicStructure/
│       ├── StudentsAndGuardians/
│       ├── Attendance/
│       ├── ResultsAndReports/
│       └── Audit/
├── database/
│   ├── factories/
│   ├── migrations/
│   └── seeders/
├── routes/api.php
└── tests/
    ├── Feature/
    └── Unit/
```

Controllers should remain thin. Form Requests validate input, Policies authorize access, Services implement business workflows, Models represent persistence, and API Resources define stable JSON output.

## Web target structure

```text
web/src/
├── app/                      Router, providers, and application setup
├── features/
│   ├── auth/
│   ├── administration/
│   ├── academic-structure/
│   ├── attendance/
│   ├── results/
│   └── parent-reports/
├── components/               Reusable presentation components
├── api/                      Generated or typed Laravel API client
├── hooks/
├── layouts/
├── lib/
├── types/
└── tests/
```

Feature folders may import shared components and the API client. Shared components must not contain CareBridge business rules.

## Mobile target structure

```text
mobile/lib/
├── app/                      App configuration, routing, and theme
├── core/
│   ├── api/
│   ├── auth/
│   ├── errors/
│   ├── storage/
│   └── widgets/
└── features/
    ├── authentication/
    ├── child-profile/
    ├── attendance/
    ├── results/
    └── report-history/
```

Flutter stores authentication credentials only in platform secure storage. It consumes the Laravel API and does not reproduce server authorization rules.

## Cross-application contract

The Laravel OpenAPI document is the authoritative API contract. React TypeScript and Flutter Dart models should be generated or verified against that document to prevent request and response drift.
