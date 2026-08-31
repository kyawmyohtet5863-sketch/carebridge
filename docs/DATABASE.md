# CareBridge Database Design

## Database system

CareBridge uses PostgreSQL. SQL is the query language; PostgreSQL is the relational database management system.

## Core tables

| Domain | Tables |
| --- | --- |
| Identity | `users`, `password_reset_tokens`, `personal_access_tokens` |
| People | `students`, `teachers`, `guardians`, `guardian_students` |
| Academic | `academic_years`, `classes`, `sections`, `subjects`, `teacher_assignments` |
| Attendance | `attendance_records` |
| Assessment | `exam_periods`, `assessment_results`, `report_comments` |
| Publishing | `monthly_reports`, `report_publications` |
| Governance | `audit_logs` |

## Core relationships

- A school has academic years, users, students, and academic structures.
- A class contains sections; students belong to a section for an academic year.
- Teachers are related to sections and subjects through `teacher_assignments`.
- Guardians and students are many-to-many through `guardian_students`.
- Attendance belongs to a student and school date.
- Assessment results belong to a student, subject, and examination period.
- Monthly reports group approved attendance summaries, results, and comments.

## Required constraints

- Unique attendance: `student_id + attendance_date`
- Unique result: `student_id + exam_period_id + subject_id`
- Marks must be between zero and the configured maximum.
- Attendance status must be `present`, `absent`, `late`, or `leave`.
- Foreign keys prevent orphaned academic records.
- Published reports record publisher, publication time, and revision.
- Critical changes record the responsible user and timestamps.

## Migration rules

- Never edit a migration already applied to a shared environment.
- Create a new migration for every schema change.
- Include a safe rollback when possible.
- Review indexes for all foreign keys and common report filters.
- Seed only synthetic demonstration data.
