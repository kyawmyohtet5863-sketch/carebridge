# CareBridge API Conventions

## Base URL

```text
/api/v1
```

## Format

- HTTPS in non-local environments
- JSON request and response bodies
- ISO 8601 date and time values
- UUIDs or non-sequential public identifiers for externally exposed records
- Consistent pagination, validation, and error envelopes

## Example success response

```json
{
  "data": {
    "id": "student-public-id",
    "name": "Example Student"
  }
}
```

## Example validation error

```json
{
  "message": "The submitted data is invalid.",
  "errors": {
    "mark": ["The mark must not exceed the configured maximum."]
  }
}
```

## Authorization

Every protected request must check authentication and resource authorization. Controllers must not depend on frontend role checks.

Examples:

- A teacher requesting a student must have an active assignment covering the student's section and requested subject.
- A guardian requesting a student must have an active `guardian_students` relationship.
- A guardian requesting a report must also satisfy `status = published`.

## Initial endpoint groups

```text
POST   /api/v1/auth/login
POST   /api/v1/auth/logout
GET    /api/v1/me

GET    /api/v1/students
GET    /api/v1/students/{student}
GET    /api/v1/classes/{class}/students

GET    /api/v1/attendance
POST   /api/v1/attendance/bulk

GET    /api/v1/exam-periods
POST   /api/v1/results/bulk

GET    /api/v1/monthly-reports/{report}
POST   /api/v1/monthly-reports/{report}/submit
POST   /api/v1/monthly-reports/{report}/return
POST   /api/v1/monthly-reports/{report}/publish
```

The final endpoint design must be represented in an OpenAPI document and tested against both React and Flutter clients.
