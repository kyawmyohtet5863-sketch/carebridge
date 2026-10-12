# CareBridge: panels, permissions, settings and development plan

Research date: 2026-10-11 UTC. Updated: 2026-10-12. Version: 1.1.

Status: project direction confirmed by Kyaw; school-specific policies require validation. This is the first confirmed CareBridge planning report, updated with branding and bilingual requirements. It does not claim implemented features, production readiness or GitHub publication.

## 1. Product purpose and evidence

CareBridge helps a school and verified guardians understand academic progress, notice a student's interests inside and outside class, and agree on practical support. It must preserve the student's voice and allow interests to change. A portfolio observation is contextual evidence, not a permanent talent classification.

The plan assumes a single-school middle/secondary-school pilot. English and Myanmar (Burmese) are confirmed interface languages. School jurisdiction, age range, policies and operating practices still need confirmation. Global guidance informs design; it does not supply one universal grading, attendance or privacy policy.

| Research finding | Proposed CareBridge decision | Evidence limit |
| --- | --- | --- |
| OECD includes agency, wellbeing and broad competencies; its framework requires local contextualization [S1] | Keep academic and growth views together; record the child's own words and changing interests | A framework does not establish that this app improves learning |
| UNESCO places learners and teachers at the centre and questions technology's suitability and value [S2] | Pilot a small set of useful tasks; measure workload and access before adding modules | No automatic benefit from digitizing every school process |
| EEF reviews evidence for school-parent support of learning [S3] | Pair feedback with a practical next step and a route for family context | Evidence about parental engagement is not evidence for CareBridge specifically |
| UNICEF describes school data risks and safeguards [S4] | Minimize collection, verify relationships and define retention before real-data use | This guidance has an EU/GDPR orientation; local legal applicability requires separate assessment |
| OWASP recommends least privilege, default denial and checks on every request [S5] | Enforce capability, relationship/scope and lifecycle checks in Laravel | Hidden menus are usability controls, not security |
| Moodle combines roles with contexts [S6] | Teacher and Sub-admin access is bounded by assignment, not job title alone | A useful established-system pattern, not a requirement to copy Moodle |
| W3C recommends clear submission feedback and actionable error messages [S7] | Preserve form values, label errors and confirm successful saves accessibly | Accessibility requires manual evaluation as well as automated checks |

## 2. Three panels, four account roles

| Panel | Account role | Main purpose | Access scope |
| --- | --- | --- | --- |
| Administration web | Main Admin | Set school policy, provision relationships, delegate work, oversee publication | School records needed for administration; private portfolio drafts are not automatically available |
| Administration web | Sub-admin | Perform specifically delegated administrative work | Selected classes or explicitly granted school-wide scope, per capability |
| Teacher web | Teacher | Record attendance, assessment feedback, reports and growth support | Assigned classes/subjects and explicit portfolio responsibilities |
| Family web and Flutter | Guardian | Understand published progress, contribute observations and agree on support | Verified linked children only; each relationship evaluated independently |

Students have no account in the first release. Adults can record their words with attribution and confirm understanding with the student. That accommodation is not a reason to exclude students from design interviews or decisions about sharing.

Sub-admin is a restricted account role using the Administration panel. It is not a fourth panel. Mobile is another client for the Family panel, not another permission model.

## 3. Task-oriented navigation and usage

### Administration

| Area | Typical task | Completion result |
| --- | --- | --- |
| Overview | Review pending links, submitted reports and corrections | A prioritized work queue, without student rankings |
| People and access | Invite/deactivate users; grant scoped Sub-admin capabilities | Account and delegation history |
| School setup | Set year, classes, subjects and reporting periods | Versioned structure and rules |
| Students and relationships | Enroll students, assign staff, verify/revoke guardians | Explicit current access relationships |
| Report review | Check submitted reports; return with a reason or publish | Reviewed version visible to the intended family |
| Support oversight | See shared goals and overdue agreed follow-ups | Support coordination within permitted scope |
| Requests and corrections | Resolve factual correction and conversation requests | Recorded response and resolution |
| Settings and audit | Maintain governance; inspect relevant changes | Traceable policy and record history |

Sub-admin sees only authorized areas and actions. For example, a class-office Sub-admin may manage enrollment and attendance corrections in Class 7A while having no report-publication, account-delegation or school-policy rights.

### Teacher

| Area | Typical frequency | Primary action |
| --- | --- | --- |
| Today | Daily | Open assigned tasks and resume unfinished work |
| My students | As needed | View permitted student context and verified family links |
| Attendance | Each school day | Bulk entry; review missing entries; correct with a reason |
| Assessments | Per assessment | Enter marks, maximum marks and actionable feedback |
| Monthly reports | Per reporting period | Assemble, preview and submit a report |
| Growth and support | When meaningful evidence exists | Record activities/reflection; agree one practical support step |
| Family responses | As needed | Address context, corrections and conversation requests |

Attendance remains unset until recorded: opening a screen must not silently mark everyone present. Growth logging has no compulsory daily quota. Teacher dashboards prioritize work rather than comparisons between children.

### Family

| Area | Primary action |
| --- | --- |
| Family overview | Select a linked child; see newly published information and agreed next steps |
| Child profile | Check factual details; request corrections |
| Attendance | Understand recorded attendance and report an error |
| Reports and feedback | Read published/archived versions; acknowledge receipt; download an authorized PDF |
| Growth portfolio | View shared school/home activities and add a personal draft observation |
| Support and requests | Respond to a goal, share relevant context or request a conversation |
| Account and privacy | Manage password, language, active sessions and permitted sharing choices |

Acknowledgement means receipt, not agreement. A parent must be able to disagree respectfully or explain context. Family responses are structured requests/comments in v1; real-time chat is deferred.

## 4. Permission model

An action is allowed only when all relevant conditions hold: authenticated active account, permitted capability, permitted school/class/student relationship, allowed record visibility, and valid lifecycle transition. Explicit restrictions take precedence over broad grants. The API derives identities from the authenticated session, never from caller-supplied role names.

Use a small documented capability catalogue. Examples: `students.manage`, `guardian_links.verify`, `attendance.record`, `attendance.correct`, `assessments.edit`, `reports.review`, `reports.publish`, `reports.export`, `portfolio.create`, `portfolio.share`, `support.manage`, `audit.read`, `settings.academic`, `access.delegate`.

| Action/resource | Main Admin | Sub-admin | Teacher | Guardian |
| --- | --- | --- | --- | --- |
| Invite/deactivate accounts | Yes, audited | No by default; bounded invitation may be delegated | No | No |
| Grant roles/capabilities | Yes; cannot remove last active Main Admin | No; cannot elevate self or others | No | No |
| Maintain class/student structure | Yes | Explicit capability + scope | Read assigned context | Read linked child's permitted details |
| Verify/revoke guardian link | Yes; evidence and reason | Explicit capability + scope | Request verification | Request link; cannot approve own claim |
| Record/correct attendance | Separate capabilities | Separate capabilities + scope | Assigned class; correction reason | Read linked child; request correction |
| Enter marks/feedback | Only with appropriate academic responsibility | Explicit capability + scope | Assigned subject/period | Published information only |
| Draft/submit report | Assigned responsibility | Explicit capability + scope | Assigned report responsibility | No |
| Review/publish report | Separate capabilities | Explicit capability + scope | No by default | No |
| Read unpublished academic report | Within review responsibility | Within review grant + scope | Within assigned responsibility | No |
| Read private growth draft | Own draft; no automatic admin override | Own draft | Own draft | Own draft |
| Read shared growth/support | Explicit oversight responsibility | Grant + scope | Explicit portfolio responsibility | Verified child relationship + audience |
| Edit growth observation | Own allowed record; correction process for another author | Same | Same | Same |
| Export | Approved task + authorized records | Separate export grant + scope | Assigned permitted records | Own child's published records |
| Change global privacy/delegation rules | Yes | No | No | No |
| Read audit | Relevant governance events | Explicit limited grant | Relevant record history | Relevant shared revision history |

Deletion is not a generic permission toggle for academic history. Use withdrawal/archive/correction workflows; approved retention disposal is a distinct administrative procedure. Do not add a broad client-visible “delete everything” permission.

Grants record actor, capability, scope, grant time, reason and optional expiry. Revocation applies on the next request; clients refresh their permissions and remove cached restricted data. Accounts with multiple real responsibilities receive explicit assignments, not an implicit union of all school data.

## 5. Core workflows

### Academic reporting

Draft -> Submitted -> Published -> Archived.
Submitted -> Returned -> Draft -> Submitted.

Submission freezes the review version. Reviewers return specific issues or publish that exact version. Default rule: a reviewer does not publish their own report; a small-school exception requires an explicit audited school policy. Publication records reviewer, timestamp, period and policy version.

Published reports are immutable snapshots. A factual correction creates a superseding version, links to the prior version and records the reason. Family views explain that a correction occurred; history is retained within authorized visibility and retention rules. Report export uses the same authorization as screen access.

### Attendance and assessments

Teacher opens assigned class/date -> records values -> reviews summary -> saves validated entries. Repeated saves must not create duplicate daily records. A concurrent change produces a conflict requiring review, not silent overwrite. Later corrections preserve prior value, reason, actor and time.

Assessment marks require a valid maximum and an explicit missing/not-assessed state. Zero is a score, not the substitute for missing data. Reports use the grading policy effective for that assessment/report period.

### Growth and student voice

Create private draft -> record activity/date/context -> record student words when offered -> preview audience -> share -> revise or withdraw through a recorded action.

Suggested fields: activity/interest/achievement type, school/home context, factual observation, student's own words, author, date, audience and optional next step. Separate observation (“practised passing with teammates twice”) from interpretation (“showed persistence in this activity”). Never convert an observation into a fixed career prediction or talent score.

For optional growth sharing, ask for age-appropriate student agreement and show whom it will reach. This agreement is a product safeguard, not a substitute for the school's lawful data-processing basis. Declining optional reflection/sharing must not block core school services. Changing future visibility does not erase copies already legitimately downloaded; explain this clearly.

Drafts are private to their author. Shared audiences are selected from verified guardians and specifically assigned staff, not the whole school. No open-ended confidential/safeguarding dossier in the general portfolio. Urgent concerns follow the school's existing safeguarding procedure; CareBridge is not an emergency reporting service.

### Support goals

Proposed -> Agreed -> Active -> Reviewed -> Completed or Revised/Closed.

Record student preference, one realistic action, responsible adult, follow-up date and evidence of progress. Completion records reflection, not just a checkbox. A child can change their interest; goal closure must not be presented as failure.

### Guardian linking and requests

Link request -> school verification -> active relationship -> expiry/revocation where appropriate. Knowing a child's name, phone number or student ID is insufficient proof. Relationship verification and exceptional access restrictions follow approved school practice.

Correction request -> assigned reviewer -> response -> accepted change or reasoned decline -> resolved. A conversation request has an owner and status; it must not imply immediate response or successful delivery through a notification channel not yet built.

## 6. Settings catalogue

| Setting group | Owner | Delegation | Change rule |
| --- | --- | --- | --- |
| School identity, timezone and default language | Main Admin | Identity changes may be narrowly delegated | Preview date/report impact; store event timestamps consistently |
| Academic year, terms, classes and subjects | Main Admin | Selected academic capability + scope | Effective dates; preserve historical enrollment |
| Assessment maxima, grade bands and missing-data rules | Academic policy owner | Explicit policy-maintenance grant if school approves | Version by period; no silent recalculation of published reports |
| School calendar and attendance categories | Main Admin | Limited academic administration | Distinguish non-school days, missing records and absence |
| Reporting periods, reviewer assignments and publication rules | Main Admin | Reviewer assignments may be delegated | Separate review and publish capabilities |
| Portfolio audiences and sharing defaults | Main Admin governance; author chooses permitted audience | No broad override | Private draft default; restrict future sharing explicitly |
| Guardian verification and restricted relationships | Main Admin | Qualified scoped verifier | Audit activation/revocation; do not reveal verification evidence broadly |
| Retention, export and disposal | Main Admin + school policy owner | No general Sub-admin delegation | Decide per record class before live pilot; approved disposal respects holds/backups |
| Account capabilities and scope | Main Admin | No self-delegation | Preview effective access; require reason; audit changes |
| Password, language and session management | Each account | Not applicable | Personal preferences cannot change school authorization |

Do not invent a universal retention duration or grade scheme. School policy and applicable jurisdiction determine them. Credentials, database connection strings and service keys belong in server configuration, not panel settings. Notifications remain deferred, so no nonfunctional notification switches.

### Confirmed branding and design settings

Design responsive Administration, Teacher and Family web panels and a Flutter Family client. Use shared color, typography and spacing tokens while adapting navigation and interactions to each platform. The initial proposed visual direction is calm blue and teal, readable text and accessible contrast. Supply a clearly identified CareBridge placeholder logo until an approved logo replaces it.

Main Admin can manage school display name, logo and theme colors in Administration > Settings > Branding. Sub-admin may edit only with an explicit branding capability; publishing is a separate grant. Neither permission grants student-data access or account-delegation rights.

Workflow: edit draft -> preview representative web/mobile screens -> validate -> publish. Provide reset-to-default, prior-version recovery and an audit record. Both clients consume the same published configuration, with a bundled fallback if fetching fails. A client may retain its last valid version; publishing need not update offline devices immediately.

Customization uses controlled tokens rather than arbitrary CSS or executable content. Validate color contrast, logo format/size and readable Myanmar typography before publishing. Logo asset upload is part of branding scope; student evidence attachments remain deferred. Each user may select a personal light/dark preference without changing the school's branding.

### Confirmed Sub-admin access management

Administration > People and access > Sub-admin Access lets Main Admin create/deactivate Sub-admin accounts and assign modules, actions, class/school scope and optional expiry. Show effective access before saving and record the change reason. Sub-admin cannot change their own grants or delegate privileges to others. Laravel enforces every capability and record scope; web/mobile menus reflect those server decisions.

### Confirmed English and Myanmar support

All panels and both clients support English and Myanmar (Burmese), identified by locale codes `en` and `my`. Include an English / မြန်မာ switch. Main Admin chooses the school default; a Sub-admin can change it only through an explicit language-settings grant. Each account controls its own display preference, which takes precedence over the school default.

Translate navigation, buttons, instructions, empty/loading states, validation messages, status labels and report/PDF labels. Backend errors expose stable codes that clients translate rather than presenting raw technical exceptions. Keep identifiers and stored workflow values independent of translated labels.

Student names, comments, observations and student voice retain their original language. Do not automatically translate user-written content in v1. Switching display language preserves the current page and unsaved input. Use fonts with correct Myanmar shaping, embed suitable fonts in PDFs, and test line wrapping, search/input and both language layouts on real devices. Use English as the missing-translation fallback during development; missing keys and unreviewed translations must be resolved before release.

Human review of Myanmar wording is a release check. Language choice does not change permissions, published academic values or the school timezone. Additional pilot languages can be considered later without replacing the two confirmed languages.

## 7. First release boundaries

Build: verified accounts and access; Main Admin management of scoped Sub-admin permissions; academic structure; attendance; assessments and feedback; reviewed monthly reports and authorized PDF; growth portfolio with student voice; practical support goals; family acknowledgement/context/corrections; relevant history, privacy and accessible responsive interfaces; configurable school branding with a placeholder logo; English/Myanmar interfaces and report labels on web/mobile.

Defer until pilot evidence: attachments, activity/opportunity directories, proactive notifications, announcements/calendar integrations, leave-request automation, imports and advanced analytics.

Exclude from v1: fees, instant chat, homework submission, public talent rankings, AI career/talent predictions, automated mental-health diagnosis and compulsory daily growth scores.

## 8. Architecture and current implementation gap

Keep React web and Flutter family clients on a Laravel API with PostgreSQL. Laravel owns authorization and transactions. Shared policy applies identically to both clients. A school identifier on records provides a clear boundary without building a multi-school product now.

The local starter contains authentication scaffolding and a partial growth portfolio. It currently uses three role values and explicit portfolio membership. It does not yet implement this Sub-admin capability catalogue, the complete academic model or all visibility/workflow rules above. In particular, administrator draft visibility in the current portfolio must be reconciled with the proposed author-private rule before real-data use. Backend and Flutter runtime validation remain outstanding. A successful web build does not prove the whole product works.

Proposed module boundaries: Authentication/Users; AcademicStructure; StudentsAndGuardians; Attendance; AssessmentsAndReports; GrowthPortfolio; SupportAndFamilyRequests; AuditAndGovernance. Keep state transitions and policy checks in backend services/policies rather than duplicating them in screen handlers.

## 9. Development sequence and exit criteria

| Phase | Deliverable | Exit evidence |
| --- | --- | --- |
| 0: Validate design | School interviews, workflow map, approved permission matrix and settings defaults | Responsible school person answers the open decisions below; synthetic-data prototype tested |
| 1: Access foundation | Four roles, explicit grants, assignments, verified links, session/revocation handling; shared design tokens and English/Myanmar translation structure | API tests deny cross-child/class access, self-elevation and revoked access; language switch preserves unsaved work |
| 2: School records | Structure, attendance, assessments, revision rules | Bulk entry and concurrent correction tested; missing values remain distinct from zero/absence |
| 3: Reporting | Draft/review/publication, version snapshots, family access and PDF | Guardian cannot retrieve any unpublished version or another child's export |
| 4: Growth and support | Audiences, student voice, shared timeline, agreed goal and correction workflows | Private draft boundaries, withdrawal and goal revision tested |
| 5: Family clients and configuration | Responsive web flows followed by Flutter against the same API; branding preview/publication and bilingual report rendering | All linked-child flows work; no privilege differences between clients; branding and both languages verified on web/mobile/PDF |
| 6: Pilot | Staff training, restore rehearsal, accessibility review and controlled rollout | Document workload, comprehension, errors and access incidents; decide next scope from evidence |

Do not promise dates before agreeing team capacity. Build one working end-to-end task in each phase before expanding screens. Family web can be usable before the mobile client is complete.

## 10. Validation plan

Recruit a small purposive sample: one Main Admin, one prospective Sub-admin, several teachers, guardians with varied language/device access, and students with appropriate school/guardian arrangements and voluntary participation. This is discovery, not a representative global study.

Ask participants to show their current process using anonymized examples: attendance recording, report correction, family linking, sharing an activity and following up on a goal. Identify duplicate work, exceptions and who actually has authority. Test realistic prototypes, including wrong-child selection, missing data, revoked access, a returned report and an interrupted save.

Measure unassisted task completion, mistakes, time compared with the existing process, teacher workload, family understanding of the next action and student comfort with the wording/audience. Agree pilot thresholds with the school after observing the baseline; do not interpret short-term usability results as improved attainment or proven talent detection.

Mandatory verification: direct API requests with forged IDs/scopes; inaccessible exports; omitted/revoked grants; assignment changes; forbidden lifecycle transitions; stale concurrent edits; unauthorized audit/history endpoints; keyboard/screen-reader forms; low-bandwidth failure recovery; realistic backup/restore. Use synthetic student records until access and governance checks pass.

## 11. Decisions needed before full implementation

1. Which country/jurisdiction and school ages does the first pilot serve, and who reviews the confirmed English/Myanmar wording?
2. Who verifies guardian links, handles restricted relationships and approves exceptional access?
3. Who owns attendance, subject marks and final reports; can reviewers ever publish their own work?
4. What reporting periods, grading rules and retention policies already exist?
5. Which exact tasks and classes should the first Sub-admin manage?
6. Which staff/guardians may receive growth entries, and how are student objections/corrections handled?
7. What devices/connectivity do families actually have, and what non-digital alternative remains available?

Next implementation task after school validation: deliver the access foundation and a restricted Sub-admin demonstration using synthetic Class 7A/7B data, with tests proving that a 7A grant cannot read or modify 7B.

## Sources

All sources accessed 2026-10-11 UTC. Design proposals above are CareBridge decisions informed by these sources, not endorsed by the organizations.

- [S1: OECD Learning Compass 2030](https://www.oecd.org/en/data/tools/oecd-learning-compass-2030.html)
- [S2: UNESCO GEM 2023 recommendations](https://gem-report-2023.unesco.org/recommendations/)
- [S3: EEF Working with Parents to Support Children's Learning](https://educationendowmentfoundation.org.uk/education-evidence/guidance-reports/supporting-parents)
- [S4: UNICEF Data protection in schools, August 2024](https://www.unicef.org/eca/reports/data-protection-schools)
- [S5: OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [S6: Moodle Roles and permissions](https://docs.moodle.org/en/Roles_and_permissions)
- [S7: W3C WAI Forms: User Notification](https://www.w3.org/WAI/tutorials/forms/notifications/)
