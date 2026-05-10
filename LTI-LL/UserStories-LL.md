# LTI - User Stories, Product Backlog, Prompt Experiments, and Technical Planning

## 1. Executive Summary

- This document translates the LTI PRD into 10 MVP user stories ready for Agile refinement.
- The stories cover job vacancy management, application intake, candidate profile, pipeline, collaboration, interviews, feedback, candidate communication, assistive AI, human decision controls, RBAC, and basic reporting.
- Each user story follows the mandatory `user-story-template.md` structure and includes BDD acceptance criteria, non-functional requirements, traceability, INVEST evaluation, Definition of Ready, and Definition of Done.
- The backlog is prioritized using simplified WSJF because the MVP must balance business value, urgency, risk reduction, and delivery effort.
- Assistive AI is treated as a productivity aid only: it can summarize, suggest, and draft, but it never makes final hiring, rejection, offer, ranking, or stage-change decisions.
- `US-001: Create and Publish a Job Vacancy` is selected for technical planning because it is foundational for application intake, pipeline tracking, candidate communication, and reporting.
- Technical work tickets are decomposed into actionable frontend, backend, data, security, QA, notifications, and UX tasks using Fibonacci story points.
- Open risks remain around privacy/compliance validation, AI output review policy, email/calendar provider decisions, and stakeholder agreement on MVP workflow defaults.

## 2. Source Interpretation

The PRD defines LTI as a B2B SaaS Applicant Tracking System for medium-sized companies that need a structured, collaborative hiring workflow without enterprise-suite complexity. The MVP focuses on the core hiring lifecycle: creating vacancies, receiving applications, reviewing candidates, moving applications through a pipeline, coordinating interviews, gathering feedback, communicating with candidates, and monitoring basic operational metrics.

Relevant constraints from the PRD:

- The MVP is a responsive web application plus candidate-facing application pages.
- The backend should be a modular monolith with REST APIs, PostgreSQL, external document storage, background jobs, email delivery, basic calendar support, and an external LLM provider.
- Internal roles are Admin, Recruiter, Hiring Manager, and Interviewer. Candidate is an external persona with limited portal access.
- AI is assistive only and must remain visibly separate from human decisions.
- Advanced job board integrations, HRIS/payroll integrations, SSO, complex BI, multi-brand career sites, and enterprise governance are out of MVP scope.
- Basic auditability is required for important human decisions, stage changes, comments, evaluations, and AI insight review.

Agile principles applied:

- User stories are written from the perspective of user value rather than implementation detail.
- Stories are scoped to be independently refinable and testable where possible.
- Acceptance criteria use BDD `Given / When / Then` format.
- INVEST, Definition of Ready, and Definition of Done are applied to support refinement and implementation readiness.
- Prioritization uses a transparent method so the team can debate trade-offs with stakeholders.

## 3. User Stories

## US-001: Create and Publish a Job Vacancy

### 1. Summary

**Epic / Feature:** Job vacancy management

**Priority:** Must  
**Estimate:** 8  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 5.1 Create and publish a job vacancy, 6 Data Model, 10 Key MVP Assumptions and Constraints

### 2. User Story

**As a** Recruiter,  
**I want** to create a job vacancy, assign a hiring team, request approval, and publish it,  
**so that** candidates can apply to an approved and structured opening.

### 3. Context and Value

Vacancy creation is the starting point of the hiring workflow. It gives recruiters and hiring managers a shared source of truth for role requirements before applications arrive. The expected outcome is an approved vacancy that can be exposed through candidate-facing application pages.

**Expected value:**

- Recruiters can launch structured hiring processes without spreadsheets or disconnected documents.
- Hiring managers validate role details before publication, reducing rework and misaligned screening.
- Improves time to first candidate review and active vacancy visibility.

### 4. Scope

**Includes:**

- Create and edit vacancy drafts with title, department, location, work mode, seniority, salary range, description, required skills, and status.
- Assign hiring team members using `JobOpening` and `JobTeamMember`.
- Submit for hiring manager approval, capture approval or change request, and publish when approved.
- Authorized access for Admin, Recruiter, and assigned Hiring Manager.

**Does not include:**

- Automated posting to external job boards.
- Advanced approval chains or custom workflow builders.
- AI-generated job descriptions.

### 5. Business Rules

- Only Recruiters and Admins can create or publish job vacancies.
- A Hiring Manager must approve the vacancy before publication.
- A vacancy cannot be published if required fields or hiring manager assignment are missing.
- Published vacancies are visible on candidate-facing application pages.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Vacancy Draft Is Created

**Given** an authenticated Recruiter with vacancy creation permission,  
**when** the Recruiter enters all required vacancy fields and saves the form,  
**then** LTI creates a `JobOpening` in draft status with the provided details.

#### AC2 - Approved Vacancy Can Be Published

**Given** a draft vacancy has a hiring team and the assigned Hiring Manager has approved it,  
**when** the Recruiter selects publish,  
**then** LTI changes the vacancy status to open and makes it available on the candidate-facing application page.

#### AC3 - Invalid Publication Is Blocked

**Given** a vacancy is missing required fields or approval,  
**when** the Recruiter attempts to publish it,  
**then** LTI keeps the vacancy unpublished and shows clear validation errors.

### 7. Non-Functional Requirements

- **Security and permissions:** Only Admins and Recruiters can create or publish; assigned Hiring Managers can review and approve.
- **Privacy and compliance:** Salary range and internal notes must only be visible to authorized internal users if configured as internal.
- **Auditability:** Creation, edits, approval, change requests, and publication must record user, timestamp, status change, and changed fields.
- **Performance:** Save and publish actions should respond within 2 seconds under normal MVP load.
- **Accessibility / UX:** Forms must support keyboard navigation, labels, validation messages, and responsive layouts.

### 8. Dependencies and Assumptions

**Dependencies:**

- Organization, User, Role, JobOpening, and JobTeamMember entities.
- Authentication and role-based access control.

**Assumptions:**

- MVP uses a fixed approval flow: Recruiter submits, Hiring Manager approves, Recruiter publishes.

### 9. Data and Traceability

**Affected entities:** Organization, User, Role, JobOpening, JobTeamMember  
**Events/audit trail:** vacancy created, vacancy edited, approval requested, approved, changes requested, published  
**Notifications:** approval request to Hiring Manager, publication notice to hiring team

### 10. INVEST Evaluation

- **Independent:** Partial - depends on authentication and RBAC foundations.
- **Negotiable:** Yes - exact fields and approval copy can be refined.
- **Valuable:** Yes - enables all downstream hiring activity.
- **Estimable:** Yes - scope is clear and based on PRD use case 5.1.
- **Small:** Yes - can fit in one sprint if approval remains fixed.
- **Testable:** Yes - criteria cover creation, publication, and blocked publication.

### 11. Refinement Notes

- Confirm which vacancy fields are mandatory for MVP.
- Decide whether salary range can be marked internal-only.
- Split approval into a separate story if stakeholder workflow expands.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-001 | Define JobOpening Status Model | Data | 3 |
| TASK-002 | Implement Vacancy Create and Edit API | Backend | 5 |
| TASK-003 | Implement Approval and Publication Workflow API | Backend | 5 |
| TASK-004 | Add Vacancy Audit Events | Backend | 3 |
| TASK-005 | Build Vacancy Create/Edit Form | Frontend | 5 |
| TASK-006 | Build Hiring Manager Approval View | Frontend | 3 |
| TASK-007 | Expose Published Vacancy on Candidate Page | Frontend | 3 |
| TASK-008 | Queue Hiring Team Notifications | Notifications | 3 |
| TASK-009 | Test Vacancy Workflow End to End | QA | 5 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## US-002: Submit an Application with CV and Consent

### 1. Summary

**Epic / Feature:** Application intake

**Priority:** Must  
**Estimate:** 8  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 5.2 Manage applications in the recruitment pipeline, 6 Data Model, 7 High-Level System Design

### 2. User Story

**As a** Candidate,  
**I want** to submit my application with contact details, CV, links, screening answers, and consent,  
**so that** the hiring team can review my candidacy for an open vacancy.

### 3. Context and Value

Application intake turns an open vacancy into a real hiring pipeline. Candidates need a simple, trustworthy flow that captures the minimum information required for review. The expected outcome is a candidate profile, application record, document metadata, and confirmation that the application was received.

**Expected value:**

- Candidates can apply without internal system access.
- Recruiters receive complete applications with CVs and consent captured.
- Supports candidate response time and time to first review metrics.

### 4. Scope

**Includes:**

- Candidate-facing application form for published vacancies.
- Capture personal data, CV upload, profile links, screening answers, and consent.
- Create or update `Candidate`, create `Application`, attach `Document`, and assign initial `PipelineStage`.
- Confirmation page and application received notification.

**Does not include:**

- Candidate account management beyond the application flow.
- Advanced duplicate resolution across organizations.
- Automated rejection or ranking.

### 5. Business Rules

- Applications can only be submitted for published and open vacancies.
- Required consent must be captured before storing application data.
- A candidate cannot have more than one active application for the same job opening.
- CV parsing and AI summaries must run asynchronously and must not block submission confirmation.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Complete Application Is Submitted

**Given** a Candidate opens a published vacancy application page,  
**when** they provide required details, upload a CV, answer required screening questions, accept consent, and submit,  
**then** LTI creates the candidate, application, document metadata, and initial pipeline stage assignment.

#### AC2 - Missing Consent Blocks Submission

**Given** a Candidate completes the application form without accepting required consent,  
**when** they submit the form,  
**then** LTI blocks submission and explains that consent is required.

#### AC3 - Duplicate Active Application Is Prevented

**Given** a Candidate already has an active application for the same vacancy,  
**when** they submit another application with the same email,  
**then** LTI prevents the duplicate active application and shows a clear message.

### 7. Non-Functional Requirements

- **Security and permissions:** Candidate can only submit public application data; internal notes, evaluations, and decisions are never exposed.
- **Privacy and compliance:** Consent text must be explicit; CVs are stored in external document storage with secure references.
- **Auditability:** Application submission, consent capture, and document upload must record timestamp and source.
- **Performance:** Submission confirmation should appear within 3 seconds excluding external storage degradation; AI processing is asynchronous.
- **Accessibility / UX:** Form fields must have labels, errors, file upload feedback, and mobile support.

### 8. Dependencies and Assumptions

**Dependencies:**

- Published `JobOpening`.
- Candidate portal/application page.
- Document storage adapter and initial pipeline stages.

**Assumptions:**

- MVP accepts one CV document plus optional links.

### 9. Data and Traceability

**Affected entities:** Candidate, Application, Document, JobOpening, PipelineStage, Notification, AIInsight  
**Events/audit trail:** application submitted, consent captured, CV uploaded, async AI job queued  
**Notifications:** application received email to Candidate, internal notification to Recruiter

### 10. INVEST Evaluation

- **Independent:** Partial - requires published vacancies and document storage.
- **Negotiable:** Yes - screening question format can be refined.
- **Valuable:** Yes - creates the candidate supply for the ATS.
- **Estimable:** Yes - workflow is clearly described in PRD use case 5.2.
- **Small:** Yes - candidate account features are excluded.
- **Testable:** Yes - criteria cover successful, consent, and duplicate flows.

### 11. Refinement Notes

- Validate consent wording with legal/privacy stakeholders.
- Confirm allowed CV formats and max file size.
- Decide how to handle existing candidate records across multiple applications.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-010 | Build public application form and validation | Frontend / UX | 5 |
| TASK-011 | Implement application submission endpoint and document metadata | Backend / Data | 5 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## US-003: Manage Applications in the Recruitment Pipeline

### 1. Summary

**Epic / Feature:** Recruitment pipeline

**Priority:** Must  
**Estimate:** 8  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 5.2 Manage applications in the recruitment pipeline, 6 Data Model, 10 Key MVP Assumptions and Constraints

### 2. User Story

**As a** Recruiter,  
**I want** to view applications by stage and move candidates through the recruitment pipeline,  
**so that** I can manage hiring progress consistently and transparently.

### 3. Context and Value

Pipeline management is the operational center of the ATS. Recruiters need to know where each application stands, what actions are pending, and which candidates need follow-up. The expected outcome is a visible, auditable stage flow across Applied, Screening, Interview, Final Review, Offer, Hired, and Rejected.

**Expected value:**

- Recruiters can manage candidate flow from one workspace.
- Hiring teams gain visibility into pipeline state.
- Supports time in stage and pipeline health metrics.

### 4. Scope

**Includes:**

- Board or list view grouped by MVP pipeline stages.
- Manual stage change by authorized internal users.
- Stage change reason for final or negative outcomes.
- Audit trail for stage changes.

**Does not include:**

- Fully custom pipeline builders.
- Automated AI-driven stage movement.
- Advanced workflow automation beyond basic notifications.

### 5. Business Rules

- Only authorized Recruiters and Hiring Managers can change application stages.
- Final stages such as Hired and Rejected require explicit human confirmation.
- AI insights may be displayed as context but cannot trigger automatic stage changes.
- Each application belongs to exactly one current pipeline stage.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Applications Are Grouped by Stage

**Given** a Recruiter opens a vacancy pipeline,  
**when** applications exist across multiple stages,  
**then** LTI displays each application in its current stage with candidate name, status, and last update.

#### AC2 - Authorized Stage Change Is Saved

**Given** a Recruiter has permission to manage a vacancy,  
**when** they move an application from Applied to Screening,  
**then** LTI updates the current stage and records the user, timestamp, previous stage, and new stage.

#### AC3 - Unauthorized Stage Change Is Blocked

**Given** an Interviewer is not authorized to manage pipeline stages,  
**when** they attempt to change an application stage,  
**then** LTI blocks the action and keeps the current stage unchanged.

### 7. Non-Functional Requirements

- **Security and permissions:** Recruiters and assigned Hiring Managers can manage stages; Interviewers have read-only assigned context.
- **Privacy and compliance:** Candidate data in pipeline views must be limited to authorized internal users.
- **Auditability:** Every stage change and final outcome requires an audit entry.
- **Performance:** Pipeline views for an MVP vacancy should load within 2 seconds for typical medium-company volumes.
- **Accessibility / UX:** Stage changes must be possible without drag-and-drop only interactions.

### 8. Dependencies and Assumptions

**Dependencies:**

- Existing applications and pipeline stages.
- RBAC and audit logging.

**Assumptions:**

- MVP uses predefined pipeline stages from the PRD.

### 9. Data and Traceability

**Affected entities:** Application, PipelineStage, Candidate, JobOpening, Comment, Notification  
**Events/audit trail:** stage changed, final outcome confirmed, stage change reason added  
**Notifications:** internal update to hiring team, optional candidate status update in US-008

### 10. INVEST Evaluation

- **Independent:** Partial - depends on application intake.
- **Negotiable:** Yes - UI can be board or list.
- **Valuable:** Yes - core recruiter workflow.
- **Estimable:** Yes - fixed MVP stages constrain scope.
- **Small:** Yes - custom workflow builder is excluded.
- **Testable:** Yes - stage views and permission checks are verifiable.

### 11. Refinement Notes

- Confirm whether Hiring Managers can move all stages or only final review stages.
- Decide if stage change reasons are mandatory for all stages or only final stages.
- Split candidate notifications if communication scope grows.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-012 | Implement pipeline stage APIs and audit trail | Backend | 5 |
| TASK-013 | Build accessible pipeline list or board view | Frontend / UX | 5 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## US-004: Review Candidate Profile with Assistive AI Summary

### 1. Summary

**Epic / Feature:** Candidate profile, Assistive AI

**Priority:** Must  
**Estimate:** 8  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 5.2 Manage applications in the recruitment pipeline, 6 Data Model, 7 High-Level System Design, 10 Key MVP Assumptions and Constraints

### 2. User Story

**As a** Recruiter,  
**I want** to review a unified candidate profile with documents, application history, notes, and AI-assisted summaries,  
**so that** I can assess the application faster while keeping the final judgment human.

### 3. Context and Value

Recruiters lose time switching between CV files, emails, notes, and pipeline data. A unified profile provides the candidate context needed for review and collaboration. AI summaries reduce manual reading time but remain clearly marked as suggestions.

**Expected value:**

- Recruiters review candidate context faster.
- Hiring managers can understand candidate background without searching across tools.
- Improves time to first candidate review and collaboration quality.

### 4. Scope

**Includes:**

- Candidate profile view with contact details, application history, documents, current stage, notes, comments, communication history, and AIInsight records.
- AI-generated CV summary and role-fit signals displayed as assistive content.
- Human review marker for AI insights.
- Access limited by role and assignment.

**Does not include:**

- Automatic candidate scoring, ranking, rejection, or selection.
- Custom AI model training.
- Cross-organization candidate profile sharing.

### 5. Business Rules

- AI summaries must be labeled as AI-generated and assistive.
- AI role-fit signals cannot be used as final ranking or decision output.
- Internal users must be authorized for the vacancy or organization to view candidate profiles.
- Original CV and human-entered data remain the source of truth if AI output is incomplete or inaccurate.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Candidate Profile Shows Unified Context

**Given** a Recruiter opens an application,  
**when** the candidate has submitted details, CV, and screening answers,  
**then** LTI displays the candidate profile, documents, application history, current stage, comments, and communication history.

#### AC2 - AI Summary Is Clearly Assistive

**Given** an AI CV summary is available,  
**when** the Recruiter views the candidate profile,  
**then** LTI labels the summary as AI-generated assistance and does not display it as a decision or ranking.

#### AC3 - Assigned Access Is Enforced

**Given** an Interviewer is not assigned to a candidate interview or vacancy,  
**when** they attempt to open the candidate profile,  
**then** LTI denies access to the profile.

### 7. Non-Functional Requirements

- **Security and permissions:** Recruiters and assigned Hiring Managers can view full candidate profiles; Interviewers view assigned context only.
- **Privacy and compliance:** Candidate PII, CVs, and AI summaries are protected and not exposed externally.
- **Auditability:** AI insight generation and human review must record timestamp, type, model provider, and reviewing user.
- **Performance:** Profile should load within 2 seconds excluding document preview; AI generation is asynchronous.
- **Accessibility / UX:** AI content must be visually distinguishable and understandable without relying only on color.

### 8. Dependencies and Assumptions

**Dependencies:**

- Application intake, document storage, AI background job, RBAC.

**Assumptions:**

- MVP stores AI outputs in `AIInsight` and allows human review acknowledgment.

### 9. Data and Traceability

**Affected entities:** Candidate, Application, Document, Comment, AIInsight, PipelineStage, Notification  
**Events/audit trail:** profile viewed if required by compliance, AIInsight generated, AIInsight reviewed  
**Notifications:** optional internal notification when AI summary is ready

### 10. INVEST Evaluation

- **Independent:** Partial - depends on intake and AI job infrastructure.
- **Negotiable:** Yes - exact layout and AI summary fields can be refined.
- **Valuable:** Yes - reduces recruiter review effort.
- **Estimable:** Yes - scope excludes scoring and ranking.
- **Small:** Partial - may be split into profile view and AI summary if needed.
- **Testable:** Yes - profile visibility and AI labeling are testable.

### 11. Refinement Notes

- Define exact AI disclaimer copy.
- Confirm whether profile view auditing is required for MVP compliance.
- Validate what candidate data is visible to Interviewers.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-014 | Build candidate profile API aggregation | Backend | 5 |
| TASK-015 | Render candidate profile with AI insight review state | Frontend / AI | 5 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## US-005: Coordinate Candidate Interviews

### 1. Summary

**Epic / Feature:** Interview coordination

**Priority:** Must  
**Estimate:** 5  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 5.3 Evaluate candidates collaboratively with AI assistance, 6 Data Model, 7 High-Level System Design

### 2. User Story

**As a** Recruiter,  
**I want** to schedule interviews, assign interviewers, define evaluation criteria, and notify participants,  
**so that** interviews are coordinated consistently and feedback can be collected.

### 3. Context and Value

Interview coordination prevents scheduling confusion and missing interviewer context. Recruiters need to create an interview plan and ensure participants know when and how to evaluate the candidate. The expected outcome is a scheduled interview record connected to the application and evaluation criteria.

**Expected value:**

- Recruiters organize interview steps in the ATS.
- Interviewers receive candidate context and evaluation expectations.
- Improves feedback completion rate and reduces pending feedback.

### 4. Scope

**Includes:**

- Create interview with scheduled time, format, assigned interviewers, and evaluation criteria.
- Link interview to `Application`.
- Notify assigned interviewers and candidate where applicable.
- Store basic calendar-compatible details.

**Does not include:**

- Full two-way calendar availability search.
- Video conferencing integration.
- Automated interview scheduling by candidates.

### 5. Business Rules

- Interviews can only be scheduled for existing applications.
- Interviewers must be internal users in the organization.
- Assigned interviewers can access only the candidate context needed for the interview.
- Interview reminders are allowed, but interview outcomes require human feedback.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Interview Is Scheduled

**Given** a Recruiter opens an application in an eligible stage,  
**when** they enter interview details, assign interviewers, and save,  
**then** LTI creates an `Interview` linked to the application and stores the evaluation criteria.

#### AC2 - Interviewers Are Notified

**Given** an interview is scheduled with assigned interviewers,  
**when** the Recruiter confirms the interview,  
**then** LTI sends internal notifications to assigned interviewers with the interview details.

#### AC3 - Invalid Interviewer Is Blocked

**Given** the Recruiter tries to assign a user outside the organization,  
**when** they save the interview,  
**then** LTI blocks the assignment and explains the validation error.

### 7. Non-Functional Requirements

- **Security and permissions:** Recruiters schedule interviews; assigned Interviewers access only assigned candidate context.
- **Privacy and compliance:** Candidate details included in notifications must be minimal and role appropriate.
- **Auditability:** Interview creation, edits, cancellations, and interviewer assignments must be recorded.
- **Performance:** Scheduling actions should complete within 2 seconds, with notifications queued asynchronously.
- **Accessibility / UX:** Date/time inputs must be keyboard accessible and display timezone clearly.

### 8. Dependencies and Assumptions

**Dependencies:**

- Application and candidate profile.
- User directory and notification service.

**Assumptions:**

- MVP stores interview schedule data and can send calendar-compatible invitations without full calendar sync.

### 9. Data and Traceability

**Affected entities:** Interview, Application, User, Evaluation, Notification  
**Events/audit trail:** interview scheduled, interviewer assigned, interview updated, interview cancelled  
**Notifications:** interviewer assignment, candidate interview invitation if sent from LTI

### 10. INVEST Evaluation

- **Independent:** Partial - requires applications and users.
- **Negotiable:** Yes - calendar depth can be refined.
- **Valuable:** Yes - enables structured interview workflow.
- **Estimable:** Yes - advanced calendar integration excluded.
- **Small:** Yes - focused on MVP scheduling.
- **Testable:** Yes - scheduling, notification, and validation can be tested.

### 11. Refinement Notes

- Confirm whether candidate interview invitation belongs here or in communication story.
- Define default evaluation criteria templates.
- Decide supported interview formats for MVP.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-016 | Implement interview creation and assignment API | Backend | 3 |
| TASK-017 | Build interview scheduling UI | Frontend / UX | 5 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## US-006: Submit Structured Interview Feedback

### 1. Summary

**Epic / Feature:** Evaluations, Collaboration

**Priority:** Must  
**Estimate:** 5  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 5.3 Evaluate candidates collaboratively with AI assistance, 6 Data Model, 10 Key MVP Assumptions and Constraints

### 2. User Story

**As an** Interviewer,  
**I want** to submit structured feedback with ratings, notes, risks, and a recommendation,  
**so that** recruiters and hiring managers can make evidence-based decisions.

### 3. Context and Value

Interview feedback is often delayed or scattered across messages. Structured feedback makes evaluations comparable and visible to the hiring team. The expected outcome is a completed `Evaluation` connected to an interview and visible to authorized users.

**Expected value:**

- Interviewers provide consistent feedback after interviews.
- Recruiters and hiring managers reduce decision delays.
- Improves interview feedback completion rate and decision quality.

### 4. Scope

**Includes:**

- Evaluation form for assigned interviewers.
- Rating, recommendation, notes, risks, and optional strengths.
- Evaluation visibility to Recruiter and Hiring Manager.
- Pending feedback status and reminder support.

**Does not include:**

- AI-generated final recommendations.
- Calibration analytics across interviewers.
- Compensation or offer approval workflow.

### 5. Business Rules

- Only assigned interviewers can submit feedback for their interview.
- Feedback cannot automatically move an application to a final stage.
- Recommendations are advisory inputs for human decision-making.
- Submitted evaluations must be timestamped and attributed to the evaluator.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Assigned Interviewer Submits Feedback

**Given** an assigned Interviewer opens a completed interview,  
**when** they enter rating, recommendation, notes, and submit,  
**then** LTI stores an `Evaluation` linked to the interview and marks feedback as submitted.

#### AC2 - Feedback Is Visible to Hiring Team

**Given** feedback has been submitted,  
**when** a Recruiter or assigned Hiring Manager opens the application,  
**then** LTI displays the evaluation with evaluator, timestamp, rating, recommendation, and notes.

#### AC3 - Unassigned User Cannot Submit Feedback

**Given** an internal user is not assigned to the interview,  
**when** they attempt to submit evaluation feedback,  
**then** LTI blocks the submission.

### 7. Non-Functional Requirements

- **Security and permissions:** Assigned Interviewers submit; Recruiters and assigned Hiring Managers view; Candidates never see internal evaluations.
- **Privacy and compliance:** Evaluation data is internal hiring data and must not be exposed through candidate portal.
- **Auditability:** Feedback creation and edits must record evaluator, timestamp, and changes.
- **Performance:** Feedback submission should complete within 2 seconds.
- **Accessibility / UX:** Form controls must be labeled and usable on desktop and mobile.

### 8. Dependencies and Assumptions

**Dependencies:**

- Interview coordination and assigned interviewer access.

**Assumptions:**

- MVP allows editing feedback before final decision, with audit history.

### 9. Data and Traceability

**Affected entities:** Interview, Evaluation, User, Application, Notification  
**Events/audit trail:** feedback submitted, feedback edited, reminder sent  
**Notifications:** feedback submitted notification to Recruiter and Hiring Manager; pending feedback reminder

### 10. INVEST Evaluation

- **Independent:** Partial - depends on interview scheduling.
- **Negotiable:** Yes - rating scale and fields can be refined.
- **Valuable:** Yes - improves decision quality and collaboration.
- **Estimable:** Yes - data structure is clear.
- **Small:** Yes - limited to structured feedback.
- **Testable:** Yes - permissions and submission are verifiable.

### 11. Refinement Notes

- Confirm recommendation values, such as Strong Yes, Yes, No, Strong No, or Needs Discussion.
- Decide whether feedback edits are allowed after submission.
- Consider splitting reminders into a notification-focused task.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-018 | Implement evaluation API and permissions | Backend / Security | 3 |
| TASK-019 | Build structured feedback form | Frontend / UX | 3 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## US-007: Collaborate with Comments and Follow-Up Tasks

### 1. Summary

**Epic / Feature:** Collaboration tools

**Priority:** Should  
**Estimate:** 5  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 5.2 Manage applications in the recruitment pipeline, 5.3 Evaluate candidates collaboratively with AI assistance, 6 Data Model

### 2. User Story

**As a** Hiring Manager,  
**I want** to comment on applications, mention teammates, and assign follow-up tasks,  
**so that** the hiring team can resolve open questions without losing context.

### 3. Context and Value

Hiring teams often coordinate through scattered email threads and chats. In-context collaboration keeps discussion attached to the relevant application. The expected outcome is a shared comment and task history visible to authorized team members.

**Expected value:**

- Hiring teams discuss candidates in context.
- Recruiters reduce manual follow-up.
- Reduces overdue feedback and missing decision context.

### 4. Scope

**Includes:**

- Add comments to applications.
- Mention authorized internal users.
- Assign simple follow-up tasks with owner and due date.
- Notify mentioned or assigned users.

**Does not include:**

- Full project management task boards.
- External candidate-visible comments.
- Slack or Teams integrations.

### 5. Business Rules

- Comments are internal and never visible to candidates.
- Only users with access to the application can comment or be mentioned.
- Follow-up tasks must have an owner from the organization.
- Comments and task assignments must be attributed to a human user.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Comment Is Added to Application

**Given** an authorized Hiring Manager opens a candidate application,  
**when** they add a comment,  
**then** LTI stores the comment with author and timestamp and displays it in the application activity.

#### AC2 - Mentioned User Is Notified

**Given** a comment includes a mention of an authorized teammate,  
**when** the comment is posted,  
**then** LTI creates an internal notification for the mentioned user.

#### AC3 - Unauthorized Mention Is Blocked

**Given** a Hiring Manager tries to mention a user without access to the application,  
**when** they post the comment,  
**then** LTI blocks the mention or removes it with a clear validation message.

### 7. Non-Functional Requirements

- **Security and permissions:** Comments and tasks are restricted to authorized internal users.
- **Privacy and compliance:** Candidate-sensitive discussion remains internal and hidden from candidates.
- **Auditability:** Comment creation, edit, deletion if supported, and task assignment must be recorded.
- **Performance:** Posting comments should complete within 2 seconds; notifications may be queued.
- **Accessibility / UX:** Comment input, mentions, and task actions must be keyboard usable.

### 8. Dependencies and Assumptions

**Dependencies:**

- Application access model and notification service.

**Assumptions:**

- MVP task management is lightweight and tied to application context.

### 9. Data and Traceability

**Affected entities:** Comment, Application, User, Notification  
**Events/audit trail:** comment created, user mentioned, task assigned, task completed  
**Notifications:** mention notification, task assignment notification

### 10. INVEST Evaluation

- **Independent:** Partial - depends on candidate/application views.
- **Negotiable:** Yes - task fields can be adjusted.
- **Valuable:** Yes - improves collaboration speed.
- **Estimable:** Yes - scope is lightweight.
- **Small:** Yes - avoids full task management suite.
- **Testable:** Yes - comments, mentions, and permissions are verifiable.

### 11. Refinement Notes

- Confirm whether comment editing/deletion is in MVP.
- Decide if tasks need status beyond open/done.
- Validate notification channels.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-020 | Implement comments and simple task records | Backend / Data | 3 |
| TASK-021 | Build comments and mention UI | Frontend / UX | 5 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## US-008: Send Candidate Status Communications

### 1. Summary

**Epic / Feature:** Candidate communication

**Priority:** Should  
**Estimate:** 5  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 5.2 Manage applications in the recruitment pipeline, 5.3 Evaluate candidates collaboratively with AI assistance, 7 High-Level System Design

### 2. User Story

**As a** Recruiter,  
**I want** to draft, review, send, and track candidate status messages,  
**so that** candidates receive timely and consistent communication during the hiring process.

### 3. Context and Value

Candidate experience depends on clear and timely updates. Recruiters need basic templates for application received, interview invitation, status update, rejection, and offer follow-up. AI may draft message text, but the recruiter must review and send it.

**Expected value:**

- Candidates receive clearer process updates.
- Recruiters reduce repetitive message writing.
- Improves candidate response time and communication consistency.

### 4. Scope

**Includes:**

- Draft and send basic candidate emails.
- Track communication history on the candidate profile.
- Optional AI-assisted draft text marked as suggestion.
- Manual recruiter review before sending.

**Does not include:**

- SMS, WhatsApp, or multi-channel campaigns.
- Marketing automation.
- Automatic rejection, offer, or status messages without human review.

### 5. Business Rules

- Recruiters must explicitly send or approve candidate messages.
- AI-drafted messages must be editable and labeled as assistive drafts.
- Rejection and offer follow-up messages require authorized human action.
- Communication history must be visible to authorized internal users.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Recruiter Sends Status Message

**Given** a Recruiter opens a candidate application,  
**when** they select a status message template, edit the content, and send it,  
**then** LTI queues the email and records the message in communication history.

#### AC2 - AI Draft Requires Human Review

**Given** an AI-assisted message draft is generated,  
**when** the Recruiter views the draft,  
**then** LTI labels it as AI-generated and requires the Recruiter to review and send it manually.

#### AC3 - Unauthorized User Cannot Message Candidate

**Given** an Interviewer opens assigned candidate context,  
**when** they attempt to send a candidate status message,  
**then** LTI blocks the action.

### 7. Non-Functional Requirements

- **Security and permissions:** Recruiters and Admins can send; Hiring Managers may view if assigned; Interviewers cannot send candidate messages.
- **Privacy and compliance:** Emails must avoid exposing internal comments, evaluations, or AI notes.
- **Auditability:** Draft creation, human send action, recipient, timestamp, and delivery status must be logged.
- **Performance:** Sending should queue within 2 seconds; delivery is asynchronous.
- **Accessibility / UX:** Editor must support clear templates, validation, and readable confirmation states.

### 8. Dependencies and Assumptions

**Dependencies:**

- Candidate profile, notification/email service, AI assistance if enabled.

**Assumptions:**

- MVP email delivery is available through a basic email provider adapter.

### 9. Data and Traceability

**Affected entities:** Candidate, Application, Notification, AIInsight, User  
**Events/audit trail:** draft generated, message edited, message sent, delivery status updated  
**Notifications:** candidate email notification

### 10. INVEST Evaluation

- **Independent:** Partial - depends on application and email service.
- **Negotiable:** Yes - template wording can be refined.
- **Valuable:** Yes - improves candidate experience.
- **Estimable:** Yes - limited to basic email.
- **Small:** Yes - excludes campaigns and multi-channel messaging.
- **Testable:** Yes - send, review, and permission behavior are testable.

### 11. Refinement Notes

- Validate rejection message policy with HR/legal stakeholders.
- Confirm templates needed for MVP.
- Decide whether AI drafts are generated on demand or asynchronously.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-022 | Implement message templates and send endpoint | Backend / Notifications | 3 |
| TASK-023 | Build candidate message composer | Frontend / UX | 5 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## US-009: Enforce Human Decision Controls

### 1. Summary

**Epic / Feature:** Human decision controls, Role-based access control

**Priority:** Must  
**Estimate:** 5  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 5.2 Manage applications in the recruitment pipeline, 5.3 Evaluate candidates collaboratively with AI assistance, 10 Key MVP Assumptions and Constraints

### 2. User Story

**As an** Admin,  
**I want** final hiring decisions, rejection decisions, offer decisions, and stage changes to require authorized human action,  
**so that** LTI preserves accountability and prevents AI or automation from deciding candidate outcomes.

### 3. Context and Value

The PRD makes human accountability a core product rule. LTI can use AI and automation to reduce repetitive work, but candidate outcomes must remain controlled by authorized humans. The expected outcome is a consistent authorization and audit layer for sensitive hiring actions.

**Expected value:**

- Organizations can trust that AI does not make final hiring decisions.
- Recruiters and Hiring Managers remain accountable for outcomes.
- Reduces compliance, fairness, and reputational risk.

### 4. Scope

**Includes:**

- Permission checks for final stage changes, rejection, offer, and hired outcomes.
- Explicit confirmation for sensitive decisions.
- Audit trail with acting user and decision context.
- Guardrails preventing AIInsight or AutomationRule from directly changing final outcomes.

**Does not include:**

- Advanced legal compliance workflows.
- Bias detection or fairness analytics.
- Multi-level enterprise approval chains.

### 5. Business Rules

- AI cannot make final hiring, rejection, offer, ranking, or stage-change decisions.
- Automation cannot move candidates to Hired, Rejected, or Offer without authorized human confirmation.
- Sensitive decisions must record who acted, when, previous state, new state, and reason where applicable.
- Candidates must not see internal decision reasoning unless explicitly included in approved communication.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Final Decision Requires Authorized Human

**Given** a Recruiter or assigned Hiring Manager has permission for final decisions,  
**when** they confirm moving an application to Hired, Rejected, or Offer,  
**then** LTI saves the decision and records the audit trail.

#### AC2 - AI Cannot Change Outcome

**Given** an AIInsight suggests role-fit signals or feedback themes,  
**when** the insight is generated,  
**then** LTI stores it as assistive content and does not change the application stage or outcome.

#### AC3 - Unauthorized Final Decision Is Blocked

**Given** an Interviewer attempts to reject or hire a candidate,  
**when** they submit the action,  
**then** LTI blocks the action and keeps the application state unchanged.

### 7. Non-Functional Requirements

- **Security and permissions:** Sensitive actions require role and assignment checks.
- **Privacy and compliance:** Internal decision context is protected from candidate-facing surfaces.
- **Auditability:** All sensitive decisions must be fully auditable.
- **Performance:** Authorization checks should add no noticeable delay to decision actions.
- **Accessibility / UX:** Confirmation dialogs must clearly explain the consequence and be keyboard accessible.

### 8. Dependencies and Assumptions

**Dependencies:**

- RBAC, Application, PipelineStage, AIInsight, audit logging.

**Assumptions:**

- MVP uses a simple fixed permission matrix by role and assignment.

### 9. Data and Traceability

**Affected entities:** Role, User, Application, PipelineStage, AIInsight, AutomationRule  
**Events/audit trail:** final decision confirmed, unauthorized attempt blocked, AIInsight stored without decision action  
**Notifications:** optional internal notification after final decision

### 10. INVEST Evaluation

- **Independent:** Partial - applies across pipeline, AI, and automation stories.
- **Negotiable:** Yes - permission matrix details need stakeholder agreement.
- **Valuable:** Yes - protects product ethics and compliance posture.
- **Estimable:** Yes - fixed MVP roles constrain scope.
- **Small:** Yes - excludes advanced compliance workflows.
- **Testable:** Yes - authorization and AI guardrails are testable.

### 11. Refinement Notes

- Confirm exact role permissions for Offer and Hired stages.
- Decide how decision reasons are captured.
- Add automated regression tests for AI and automation guardrails.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-024 | Implement sensitive-action authorization checks | Backend / Security | 3 |
| TASK-025 | Add audit trail for final decisions and blocked attempts | Backend / Data | 3 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## US-010: View Basic Pipeline Metrics

### 1. Summary

**Epic / Feature:** Basic reporting

**Priority:** Should  
**Estimate:** 5  
**Status:** Draft  
**PRD Source:** LTI-LL.md sections 3 Main Product Features, 4 Lean Canvas Key Metrics, 6 Data Model, 7 High-Level System Design

### 2. User Story

**As a** Hiring Manager,  
**I want** to view basic hiring pipeline metrics,  
**so that** I can understand vacancy health, candidate flow, pending feedback, and bottlenecks.

### 3. Context and Value

Medium-sized hiring teams need operational visibility without a custom BI platform. Basic reporting helps identify stalled candidates, overdue feedback, and active workload. The expected outcome is a simple dashboard with MVP metrics derived from existing ATS data.

**Expected value:**

- Hiring Managers and Recruiters see pipeline health quickly.
- Teams can act on pending feedback and stage bottlenecks.
- Supports key metrics from the Lean Canvas.

### 4. Scope

**Includes:**

- Metrics for open vacancies, candidates by stage, time in stage, pending feedback, and pipeline health.
- Filters by vacancy and basic date range.
- Access for Admin, Recruiter, and assigned Hiring Manager.
- Aggregated operational data only.

**Does not include:**

- Custom BI dashboards.
- Predictive analytics.
- Export integrations or executive reporting packs.

### 5. Business Rules

- Users can only view metrics for organizations and vacancies they are authorized to access.
- Metrics must be based on recorded ATS events and current application state.
- AI-generated insights must not be presented as predictive hiring decisions.
- Candidate PII should be minimized in aggregate reporting views.

> For stories involving AI: remember that AI is assistive. It must not make final hiring, rejection, offer, automatic ranking, or stage-change decisions without an authorized human action.

### 6. Acceptance Criteria

#### AC1 - Dashboard Displays Core Metrics

**Given** a Hiring Manager opens the reporting page for an assigned vacancy,  
**when** applications and interviews exist,  
**then** LTI displays open vacancy status, candidates by stage, average time in stage, and pending feedback count.

#### AC2 - Metrics Respect Authorization

**Given** a Hiring Manager is not assigned to a vacancy,  
**when** they attempt to view that vacancy's reporting data,  
**then** LTI denies access to the metrics.

#### AC3 - Empty State Is Clear

**Given** a vacancy has no applications yet,  
**when** the Recruiter opens the reporting page,  
**then** LTI shows zero-state metrics and a clear empty state without errors.

### 7. Non-Functional Requirements

- **Security and permissions:** Reports enforce organization, role, and assignment access.
- **Privacy and compliance:** Aggregate views minimize candidate PII and hide internal details from unauthorized users.
- **Auditability:** Report access may be logged if required by compliance; metric calculations must be traceable to ATS records.
- **Performance:** Dashboard should load within 3 seconds for MVP data volumes.
- **Accessibility / UX:** Charts or metrics must include text equivalents and readable labels.

### 8. Dependencies and Assumptions

**Dependencies:**

- Applications, pipeline stages, interviews, evaluations, and audit timestamps.

**Assumptions:**

- MVP reporting is operational and near-real-time, not a data warehouse.

### 9. Data and Traceability

**Affected entities:** JobOpening, Application, PipelineStage, Interview, Evaluation, User  
**Events/audit trail:** optional report viewed event, metric computation from stage and feedback timestamps  
**Notifications:** none

### 10. INVEST Evaluation

- **Independent:** Partial - depends on core workflow data.
- **Negotiable:** Yes - visualization format can be refined.
- **Valuable:** Yes - improves hiring visibility.
- **Estimable:** Yes - metrics are limited and defined.
- **Small:** Yes - excludes custom analytics.
- **Testable:** Yes - calculations and permissions can be tested.

### 11. Refinement Notes

- Define exact formula for pipeline health.
- Confirm whether average time in stage uses calendar days or business days.
- Decide if dashboard is organization-wide or vacancy-first for MVP.

### 12. Potential Work Tickets

Use this section only when the story moves into technical planning.

| Ticket | Description | Area | Estimate |
| --- | --- | --- | --- |
| TASK-026 | Build reporting queries and authorization filters | Backend / Data | 5 |
| TASK-027 | Build accessible metrics dashboard | Frontend / UX | 5 |

### 13. Definition of Ready

- [ ] The story has a clear user, action, and benefit.
- [ ] The business or user value is explained.
- [ ] The acceptance criteria are written in Given/When/Then format.
- [ ] The scope and out-of-scope items are defined.
- [ ] The main dependencies are known.
- [ ] The story has been reviewed against INVEST.
- [ ] The team can estimate it with reasonable confidence.

### 14. Definition of Done

- [ ] All acceptance criteria are met.
- [ ] Business rules are implemented and verified.
- [ ] Role permissions and restrictions are validated.
- [ ] Sensitive data is handled according to the PRD privacy rules.
- [ ] Relevant events are audited.
- [ ] The functionality has been tested across the main, alternative, and error flows.
- [ ] Backlog documentation or notes have been updated if applicable.

## 4. Product Backlog

### Prioritization Method

This backlog uses simplified WSJF because the MVP has multiple foundational flows with different value, urgency, risk, and dependency profiles. For each story, priority is based on Cost of Delay divided by effort, where Cost of Delay is interpreted qualitatively through Business Value, Urgency, and Risk Reduction / Learning. This fits the LTI MVP because the team must deliver the hiring lifecycle in the right order while learning early about privacy, collaboration, and AI guardrails.

| Rank | User Story ID | Title | Epic / Feature | Persona | Priority | Estimated Story Points | Business Value | Urgency | Risk Reduction / Learning | Dependencies | Rationale |
| --- | --- | --- | --- | --- | --- | ---: | --- | --- | --- | --- | --- |
| 1 | US-001 | Create and Publish a Job Vacancy | Job vacancy management | Recruiter | Must | 8 | Very High | Very High | High | RBAC, users | Foundational object for applications, pipeline, interviews, and reporting. |
| 2 | US-002 | Submit an Application with CV and Consent | Application intake | Candidate | Must | 8 | Very High | Very High | Very High | US-001, document storage | Converts open vacancies into candidate records while validating consent and storage assumptions. |
| 3 | US-003 | Manage Applications in the Recruitment Pipeline | Recruitment pipeline | Recruiter | Must | 8 | Very High | Very High | High | US-002, pipeline stages | Core ATS workflow and source of stage metrics. |
| 4 | US-009 | Enforce Human Decision Controls | Human decision controls / RBAC | Admin | Must | 5 | Very High | High | Very High | RBAC, US-003, AIInsight | Critical ethical and product constraint that protects the AI-assisted model. |
| 5 | US-004 | Review Candidate Profile with Assistive AI Summary | Candidate profile / Assistive AI | Recruiter | Must | 8 | High | High | Very High | US-002, AI jobs, storage | Validates AI usefulness while preserving human review and candidate context. |
| 6 | US-005 | Coordinate Candidate Interviews | Interview coordination | Recruiter | Must | 5 | High | Medium | Medium | US-003, users, notifications | Enables structured interview process after screening. |
| 7 | US-006 | Submit Structured Interview Feedback | Evaluations | Interviewer | Must | 5 | High | Medium | Medium | US-005 | Turns interviews into comparable evidence for human decisions. |
| 8 | US-008 | Send Candidate Status Communications | Candidate communication | Recruiter | Should | 5 | High | Medium | Medium | US-002, notifications | Improves candidate experience and closes the loop on status changes. |
| 9 | US-007 | Collaborate with Comments and Follow-Up Tasks | Collaboration tools | Hiring Manager | Should | 5 | Medium | Medium | Medium | US-004, notifications | Reduces fragmented discussion and missing context, but depends on core profiles. |
| 10 | US-010 | View Basic Pipeline Metrics | Basic reporting | Hiring Manager | Should | 5 | Medium | Low | Medium | US-001, US-002, US-003, US-006 | Valuable once workflow data exists; should not precede the data-producing flows. |

## 5. Prompt Experiments

### Prompt 1: Direct Generation from PRD

**Full prompt:**

```text
Act as a Senior Product Manager and Business Analyst. Read the LTI ATS PRD and generate a set of MVP user stories for implementation. Include a prioritized product backlog, choose one story for technical planning, and break it into work tickets with estimates.
```

**Expected result:** A broad first draft of user stories and backlog items derived from the PRD.

**Strengths:**

- Fast way to generate initial coverage.
- Useful for discovering obvious epics and missing areas.
- Low prompt complexity.

**Limitations:**

- Too generic without explicit template requirements.
- Acceptance criteria may not consistently use BDD format.
- AI and human-decision guardrails may be under-specified.
- Traceability to PRD sections may be weak.

### Prompt 2: Generation Using Template, INVEST, and BDD

**Full prompt:**

```text
Act as a Senior Product Manager and Business Analyst for a B2B SaaS ATS. Using the LTI PRD and the provided user-story-template.md, generate 6 to 10 user stories. Use exactly the template structure for every story. Each story must include BDD acceptance criteria using Given/When/Then, Definition of Ready, Definition of Done, non-functional requirements, traceability to PRD sections, and an INVEST evaluation. Keep AI assistive only and never allow AI to make hiring, rejection, offer, ranking, or stage-change decisions.
```

**Expected result:** User stories that are ready for refinement and consistent with the course template.

**Strengths:**

- Produces better story quality and consistency.
- Forces testable acceptance criteria.
- Captures non-functional requirements and AI constraints.
- Supports engineering refinement.

**Limitations:**

- Does not by itself optimize story ordering.
- May produce long stories without clear MVP sequencing.
- Work ticket decomposition still needs a separate planning prompt.

### Prompt 3: Generation with Explicit Prioritization and MVP Focus

**Full prompt:**

```text
Act as a Senior Product Manager and Business Analyst specialized in Agile/Scrum and B2B SaaS discovery. Using ReadMe.md, LTI-LL/LTI-LL.md, supporting-theoretical-information.md, and user-story-template.md, create LTI-LL/UserStories-LL.md in English. Generate 6 to 10 MVP user stories using exactly the template structure. Cover job vacancy management, application intake, candidate profile, recruitment pipeline, collaboration, interview coordination, candidate communication, assistive AI, human decision controls, RBAC, and basic reporting. Prioritize the product backlog using simplified WSJF and explain the method. Include at least 3 prompt experiments and select the best. Choose one foundational story for technical planning and break it into 5 to 10 technical tickets with Fibonacci story points. Include security, permissions, privacy, auditability, performance, UX/accessibility, BDD acceptance criteria, INVEST, Definition of Ready, and Definition of Done. Do not invent scope outside the MVP; mark future scope as out of scope. AI must be assistive only and must never make final hiring, rejection, offer, ranking, or stage-change decisions.
```

**Expected result:** A complete single-file deliverable aligned with the exercise, the PRD, the mandatory template, and the requested backlog planning artifacts.

**Strengths:**

- Best alignment with the actual assignment.
- Explicitly combines source files, template compliance, MVP coverage, prioritization, prompt experimentation, and technical planning.
- Reduces scope creep by naming out-of-scope areas and AI guardrails.
- Produces a document that is both product-ready and engineering-refinement-ready.

**Limitations:**

- Longer prompt requires careful review to avoid producing overly verbose output.
- Still needs human validation of stakeholder priorities, legal/privacy language, and exact permission matrix.

### Best Prompt Selection

Prompt 3 produced the best result because it constrained the assistant with the mandatory files, exact output structure, MVP coverage, prioritization method, AI guardrails, and ticket estimation requirements. Prompt 1 was useful for ideation, and Prompt 2 improved story quality, but Prompt 3 best matched the assignment and reduced the risk of missing required deliverables.

## 6. Selected User Story for Technical Planning

Selected story: `US-001: Create and Publish a Job Vacancy`.

This story is selected because it is the foundation for the rest of the MVP. Without a published `JobOpening`, candidates cannot apply, applications cannot enter a pipeline, interviews cannot be coordinated, candidate communications lack context, and reporting has no vacancy-level anchor. It also validates key cross-cutting concerns early: RBAC, auditability, status transitions, notifications, and the shared Recruiter/Hiring Manager workflow.

## 7. Work Tickets

| Ticket ID | Title | Description | Area | Story Points | Priority | Assigned person | Tags | Technical notes | Acceptance criteria | Dependencies | Comments |
| --- | --- | --- | --- | ---: | --- | --- | --- | --- | --- | --- | --- |
| TASK-001 | Define JobOpening Status Model | Define MVP vacancy statuses, allowed transitions, required fields, and persistence changes for `JobOpening` and `JobTeamMember`. | Data | 3 | Must | Backend Engineer | Data, Backend, Sprint 10 | Statuses: draft, pending_approval, changes_requested, approved, open, paused, closed. Keep transition rules server-side. | Schema supports required fields; invalid transitions can be rejected; migration is reversible in non-production. | Organization, User, Role entities | Confirm naming with engineering conventions before implementation. |
| TASK-002 | Implement Vacancy Create and Edit API | Add REST endpoints for creating, reading, and updating vacancy drafts with validation and organization scoping. | Backend | 5 | Must | Backend Engineer | Backend, Security, Sprint 10 | Validate role permissions and tenant boundaries on every request. | Recruiter can create draft; missing required fields return validation errors; unauthorized users receive forbidden response. | TASK-001, auth/RBAC | Include unit tests for validation and permissions. |
| TASK-003 | Implement Approval and Publication Workflow API | Add endpoints for submit for approval, approve, request changes, and publish. | Backend | 5 | Must | Backend Engineer | Backend, Security, Sprint 10 | Publication must require approved status and required fields. | Hiring Manager can approve assigned vacancy; Recruiter can publish approved vacancy; unapproved vacancy cannot be published. | TASK-001, TASK-002 | Keep workflow fixed for MVP. |
| TASK-004 | Add Vacancy Audit Events | Record vacancy creation, edits, approval requests, approval decisions, change requests, and publication events. | Backend | 3 | Must | Backend Engineer | Backend, Audit, Sprint 10 | Store user, timestamp, event type, previous state, new state, and relevant changed fields. | Audit entries are created for every workflow transition; audit data is queryable for vacancy history. | TASK-002, TASK-003 | Use existing audit pattern if available. |
| TASK-005 | Build Vacancy Create/Edit Form | Implement responsive internal UI for vacancy details, hiring team assignment, validation errors, save draft, and submit for approval. | Frontend | 5 | Must | Frontend Engineer | Frontend, UX, Sprint 10 | Use accessible labels, field-level validation, and clear status indicators. | Recruiter can save draft; errors are shown next to invalid fields; form works with keyboard and on mobile. | TASK-002 | Keep form fields aligned with PRD MVP fields. |
| TASK-006 | Build Hiring Manager Approval View | Implement approval screen where assigned Hiring Manager can review details, approve, or request changes with a comment. | Frontend | 3 | Must | Frontend Engineer | Frontend, Security, Sprint 10 | Show read-only role details plus approve/request changes actions. | Assigned Hiring Manager can approve or request changes; unassigned user cannot access approval actions. | TASK-003 | Request changes comment should appear in vacancy history. |
| TASK-007 | Expose Published Vacancy on Candidate Page | Render published/open vacancy details on candidate-facing page and hide draft, pending, paused, or closed vacancies. | Frontend | 3 | Must | Frontend Engineer | Frontend, UX, Sprint 10 | Public page must not expose internal-only data or audit history. | Open vacancy is visible to candidates; non-open vacancy returns unavailable state; internal-only fields are hidden. | TASK-003 | Application form itself belongs to US-002. |
| TASK-008 | Queue Hiring Team Notifications | Send internal notification for approval request, changes requested, approval, and publication. | Notifications | 3 | Should | Backend Engineer | Notifications, Backend, Sprint 10 | Queue notifications asynchronously; do not block workflow actions on email delivery. | Notification record is created for each relevant event; workflow succeeds even if delivery is queued. | TASK-003, TASK-004 | Email template wording can be basic for MVP. |
| TASK-009 | Test Vacancy Workflow End to End | Cover create, validation, approval, request changes, publish, public visibility, permission denial, and audit trail. | QA | 5 | Must | QA Engineer | QA, Security, Sprint 10 | Include API tests and at least one UI flow test if test stack supports it. | All US-001 acceptance criteria pass; unauthorized actions are rejected; audit events are verified. | TASK-002 through TASK-008 | Add regression cases for status transition edge cases. |

## 8. Effort Estimation

Estimation uses Fibonacci story points: 1, 2, 3, 5, 8, 13. Points represent relative complexity, uncertainty, implementation effort, testing effort, and cross-functional coordination. The estimates assume the MVP modular monolith architecture from the PRD and an existing authentication foundation.

| Ticket ID | Area | Fibonacci Estimate | Estimation Criteria | Risks / Uncertainties |
| --- | --- | ---: | --- | --- |
| TASK-001 | Data | 3 | Small schema/status model with important workflow implications. | Existing schema conventions may require adjustment. |
| TASK-002 | Backend | 5 | Multiple endpoints, validation, permissions, and tests. | RBAC implementation detail may increase effort. |
| TASK-003 | Backend | 5 | Workflow transitions and role-specific actions. | Approval edge cases may expand if stakeholders request more states. |
| TASK-004 | Backend | 3 | Audit events for known workflow actions. | Audit infrastructure may not exist yet. |
| TASK-005 | Frontend | 5 | Form-heavy UI with validation and hiring team assignment. | Field definitions and responsive layout may need iteration. |
| TASK-006 | Frontend | 3 | Focused approval view with two actions. | Request-changes comments may need richer activity integration. |
| TASK-007 | Frontend | 3 | Public vacancy rendering and status visibility rules. | Public routing or candidate portal shell may not exist yet. |
| TASK-008 | Notifications | 3 | Queue notification records for workflow events. | Email provider integration may be incomplete. |
| TASK-009 | QA | 5 | End-to-end coverage across workflow, permissions, and audit. | Test tooling maturity may affect effort. |

**Total approximate effort:** 35 story points.

Key uncertainty: if authentication, RBAC, audit logging, or public candidate-page routing are not already available, US-001 may require enabling platform work before the feature can be completed. In that case, the team should split those foundations into separate prerequisite tickets.

## 9. Conclusions

This backlog is suitable to start the LTI MVP because it follows the real hiring lifecycle: create a vacancy, receive applications, manage candidates in the pipeline, review profiles with assistive AI, coordinate interviews, collect feedback, communicate with candidates, enforce human decision controls, collaborate internally, and inspect basic metrics.

Decisions to validate with stakeholders:

- Mandatory vacancy and application fields for MVP.
- Exact permission matrix for Recruiter, Hiring Manager, Interviewer, Admin, and Candidate.
- Consent wording, CV retention expectations, and privacy requirements.
- Candidate communication templates and rejection message policy.
- AI disclaimer wording and human review requirements for AI insights.
- Reporting metric formulas, especially time in stage and pipeline health.

Open risks:

- Privacy and compliance expectations may vary by customer region.
- AI outputs can be useful but must be carefully labeled, reviewed, and audited.
- Notification, calendar, and document storage providers may affect implementation detail.
- Reporting quality depends on consistent audit and timestamp capture from the start.

Final checklist:

- All User Stories provide clear user or business value.
- All User Stories follow the mandatory template structure.
- All User Stories include verifiable BDD acceptance criteria.
- The backlog is prioritized with simplified WSJF reasoning.
- The technical tickets are actionable and scoped to `US-001`.
- The estimation uses Fibonacci story points consistently.
- AI is assistive only and never makes final decisions.
- The document remains aligned with the LTI PRD and does not add non-MVP scope.
