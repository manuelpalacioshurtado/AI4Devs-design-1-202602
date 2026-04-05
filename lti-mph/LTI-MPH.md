# LTI
## CONTEXT
A software application for an ATS (Applicant Tracking System) focused on interview management, candidate lifecycle tracking, and hiring workflow automation.

## PROPOSED SOLUTION
A responsive web application that helps recruiters and hiring teams manage candidate interviews, assessments, data privacy, follow-up actions, contract workflow, and communication.

## MAIN FEATURES
- Interview scheduling and management
- Candidate database with strong privacy protections
- Follow-up workflow for candidate progression and status updates
- Notifications for interview reminders, status changes, and next steps
- Support for cognitive and technical assessments
- Contract generation and signing workflow
- Candidate self-service portal for profile updates and application tracking

## APPLICATION ENHANCEMENTS
- Candidate self-service portal for applicants to update profiles, upload documents, and track application status.
- Automated follow-up workflows with reminders, next-step suggestions, and task assignments for hiring managers.
- Contract automation with templates, electronic signatures, and approval routing to speed up job offer acceptance.
- Reporting dashboards for pipeline health, time-to-hire metrics, and hiring team performance.

## INNOVATIVE FEATURES
- AI-powered candidate matching and ranking based on skills, experience, assessment results, and role fit.
- Interview intelligence with recording, transcription, summary generation, competency highlights, and sentiment insights.
- Smart scheduling integration with calendars (Google, Outlook) and automatic availability matching.
- Built-in assessment platform with instant scoring, role benchmarks, and candidate comparison tools.
- Privacy-by-design features such as consent tracking, GDPR-ready data handling, and anonymized shortlists.
- Mobile-first experience with recruiter dashboards and candidate-facing mobile access.

## USE CASES
### Create Application
Recruiters or candidates can submit a new job application and track its progress inside the ATS.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor':'#0A527B','secondaryColor':'#78B0E0','tertiaryColor':'#F1F8FF','lineColor':'#0A527B','textColor':'#102A43','fontFamily':'Arial','fontSize':'14px'}}}%%
sequenceDiagram
    participant Candidate as Candidate
    participant ATS as ATS
    participant Recruiter as Recruiter

    Candidate->>ATS: Submit application
    ATS->>ATS: Validate data and create profile
    ATS->>Recruiter: Notify new application
    Recruiter-->>ATS: Review application
    ATS-->>Candidate: Confirm receipt and status
```

### Follow Up Application
Hiring staff manage application progression, schedule interviews, and record next-step actions with automated reminders.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor':'#0A527B','secondaryColor':'#78B0E0','tertiaryColor':'#F1F8FF','lineColor':'#0A527B','textColor':'#102A43','fontFamily':'Arial','fontSize':'14px'}}}%%
sequenceDiagram
    participant Recruiter as Recruiter
    participant ATS as ATS
    participant Interviewer as Interviewer
    participant Calendar as Calendar
    participant Candidate as Candidate

    Recruiter->>ATS: Update application status / request interview
    ATS->>Calendar: Check availability
    Calendar-->>ATS: Return available slots
    ATS->>Candidate: Send interview invitation
    Candidate-->>ATS: Confirm interview slot
    ATS->>Interviewer: Notify schedule and details
    ATS->>Recruiter: Send follow-up reminder
```

### Sign Contract
The ATS supports contract generation, review, electronic signature, and completion tracking.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor':'#0A527B','secondaryColor':'#78B0E0','tertiaryColor':'#F1F8FF','lineColor':'#0A527B','textColor':'#102A43','fontFamily':'Arial','fontSize':'14px'}}}%%
sequenceDiagram
    participant Recruiter as Recruiter
    participant ATS as ATS
    participant ContractService as "Contract Service"
    participant Candidate as Candidate

    Recruiter->>ATS: Generate contract from template
    ATS->>ContractService: Create contract document
    ContractService-->>ATS: Contract ready
    ATS->>Candidate: Send contract for review & signature
    Candidate-->>ContractService: Sign contract
    ContractService-->>ATS: Confirm signed contract
    ATS->>Recruiter: Notify contract completed
```

### Core Domain Model
The system is structured around core entities that represent candidates, applications, interview events, assessments, contracts, and notifications.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor':'#0A527B','secondaryColor':'#78B0E0','tertiaryColor':'#F1F8FF','lineColor':'#0A527B','textColor':'#102A43','fontFamily':'Arial','fontSize':'14px'}}}%%
classDiagram
    classDef entityFill fill:#E8F1F9,stroke:#0A527B,stroke-width:2px,color:#102A43;
    class Candidate entityFill
    class Application entityFill
    class Interview entityFill
    class Assessment entityFill
    class Contract entityFill
    class Notification entityFill

    class Candidate {
      +String id
      +String name
      +String email
      +String privacyConsent
      +updateProfile()
    }
    class Application {
      +String id
      +String role
      +String status
      +Date submittedAt
      +updateStatus()
    }
    class Interview {
      +String id
      +Date scheduledAt
      +String type
      +String outcome
      +recordFeedback()
    }
    class Assessment {
      +String id
      +String type
      +String score
      +String result
      +evaluate()
    }
    class Contract {
      +String id
      +String template
      +String status
      +Date signedAt
      +sendForSignature()
    }
    class Notification {
      +String id
      +String type
      +String message
      +send()
    }

    Candidate "1" -- "many" Application : submits
    Application "1" -- "0..*" Interview : schedules
    Application "1" -- "0..*" Assessment : includes
    Application "1" -- "0..1" Contract : leads to
    Application "1" -- "0..*" Notification : generates
    Interview "1" -- "0..*" Notification : generates
    Contract "1" -- "0..*" Notification : generates
```

## TECHNOLOGY, INTEGRATIONS, AND ARCHITECTURE
- Responsive web app compatible with desktop, tablet, and mobile devices
- Integration with video conferencing tools for interview recording and transcription
- Integration with email and notification systems for candidate and recruiter communication
- Calendar integration for interview scheduling and availability management
- Electronic signature support for contract approval and signing

The LTI ATS is composed of a user-facing web application, backend services, external integrations, and shared persistence to support hiring workflows end to end.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor':'#166088','secondaryColor':'#DDECFB','tertiaryColor':'#F3F8FF','lineColor':'#0A527B','textColor':'#102A43','fontFamily':'Arial','fontSize':'14px'}}}%%
C4Context
    title LTI ATS System Context
    Person(candidate, "Candidate", "Job seeker using the self-service portal")
    Person(recruiter, "Recruiter", "HR or talent acquisition user")
    Person(interviewer, "Interviewer", "Staff member conducting interviews")
    System(ats, "LTI ATS", "Applicant tracking system for interview and hiring workflows")
    System_Ext(videoPlatform, "Video Conference Service", "External recording and transcription provider")
    System_Ext(emailService, "Email / Notification Service", "External communication provider")
    System_Ext(signatureService, "E-signature Service", "External contract signing provider")

    Rel(candidate, ats, "Submit applications, review status, and sign contracts")
    Rel(recruiter, ats, "Manage candidates, schedules, interviews, and contracts")
    Rel(interviewer, ats, "Access interview schedules and submit feedback")
    Rel(ats, videoPlatform, "Integrate for interview recording and transcription")
    Rel(ats, emailService, "Send notifications and messages")
    Rel(ats, signatureService, "Generate and manage contract signing")
```

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor':'#166088','secondaryColor':'#DDECFB','tertiaryColor':'#F3F8FF','lineColor':'#0A527B','textColor':'#102A43','fontFamily':'Arial','fontSize':'14px'}}}%%
C4Container
    title LTI ATS Container Diagram
    Person(candidate, "Candidate", "Job seeker using the self-service portal")
    Person(recruiter, "Recruiter", "HR or talent acquisition user")
    Person(interviewer, "Interviewer", "Staff member conducting interviews")

    System_Boundary(ats, "LTI ATS") {
        Container(webApp, "Web Application", "React / SPA", "Candidate and recruiter user interface")
        Container(apiService, "API Service", "Node.js / Express", "Business logic, workflows, and orchestration")
        Container(db, "Database", "PostgreSQL", "Stores candidates, applications, interviews, assessments, and contracts")
        Container(authService, "Authentication Service", "OAuth / JWT", "Handles authentication and authorization")
        Container(notificationService, "Notification Service", "Messaging and alerts")
        Container(videoIntegration, "Video Integration", "3rd-party API", "Interview recording and transcription")
        Container(signatureIntegration, "E-signature Integration", "3rd-party API", "Contract creation and signing")
    }

    Rel(candidate, webApp, "Apply, track status, and sign contracts")
    Rel(recruiter, webApp, "Manage applications, interviews, and contracts")
    Rel(interviewer, webApp, "View interview details and submit feedback")
    Rel(webApp, apiService, "Calls API for all actions")
    Rel(apiService, db, "Reads/writes hiring data")
    Rel(apiService, authService, "Validates authentication and authorization")
    Rel(apiService, notificationService, "Sends notifications and reminders")
    Rel(apiService, videoIntegration, "Schedules and records interviews")
    Rel(apiService, signatureIntegration, "Creates and tracks contract signing")
```

## LEAN CANVAS
### Problem
- Recruiters struggle to manage multiple candidates and interview stages efficiently.
- Manual scheduling and coordination between candidates and interviewers is time-consuming.
- Lack of centralized candidate data with privacy compliance (e.g., GDPR).
- Limited visibility into pipeline progress and hiring performance metrics.
- Contract generation and onboarding processes are often fragmented across tools.

### Customer Segments
- Primary customers: HR teams and recruiters, hiring managers, talent acquisition departments
- Secondary users: job candidates (through the self-service portal), interviewers within organizations
- Early adopters: small–medium companies scaling hiring, recruitment agencies managing multiple clients, tech companies hiring high volumes of candidates

### Unique Value Proposition
“An intelligent interview and hiring platform that automates recruitment workflows while ensuring privacy, speed, and data-driven hiring decisions.”

Key differentiators:
- AI candidate matching and ranking
- Integrated interview intelligence (recording, transcription, summaries)
- Privacy-by-design candidate database
- Smart scheduling and automated hiring workflows

### Solution
- Interview scheduling with calendar integration
- Centralized candidate database with privacy protections
- Automated hiring workflows and follow-ups
- AI candidate ranking and matching
- Built-in technical and cognitive assessments
- Contract generation and e-signature workflow
- Candidate self-service portal
- Hiring analytics dashboards

### Channels
- Direct sales to HR departments
- SaaS website with free trial
- HR tech marketplaces
- LinkedIn and professional HR networks
- Partnerships with recruitment agencies
- Integrations with existing HR tools

### Revenue Streams
- SaaS subscription model (monthly/annual)
- Tiered pricing by company size or number of recruiters
- Premium features (AI analytics, interview intelligence)
- Enterprise licensing
- Add-ons for advanced reporting or assessments

### Cost Structure
- Software development and maintenance
- Cloud infrastructure and hosting
- AI/ML model development
- Third-party integrations (video, e-signature, calendars)
- Sales, marketing, customer support, and onboarding

### Key Metrics
- Time-to-hire reduction
- Number of active recruiters or companies
- Candidate pipeline conversion rates
- Interview scheduling completion rate
- Customer retention and churn
- AI matching accuracy / recruiter usage rate

### Unfair Advantage
- AI-powered candidate ranking using multiple signals (skills, assessments, experience)
- Interview intelligence with automatic summaries and insights
- Privacy-by-design architecture for GDPR compliance
- Fully integrated workflow from application → interview → contract signing

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor':'#375a9f','secondaryColor':'#f3f6ff','tertiaryColor':'#ffffff','lineColor':'#375a9f','textColor':'#102a5c','fontFamily':'Arial','fontSize':'13px'}}}%%
flowchart TB
  classDef section fill:#f3f6ff,stroke:#375a9f,stroke-width:2px,font-weight:bold,color:#102a5c;
  classDef detail fill:#ffffff,stroke:#9bb0da,stroke-width:1px,color:#102a5c,font-size:12px;
  classDef early fill:#fff1d6,stroke:#cc7a00,stroke-width:2px,color:#663a08;
  classDef uvp fill:#e9f9f0,stroke:#1f7d4a,stroke-width:2px,color:#114a2c;
  classDef metrics fill:#f7eef8,stroke:#834d9b,stroke-width:2px,color:#4b2c57;

  P["Problem:<br/>Recruiters struggle to manage multiple candidates and interview stages efficiently.<br/>Manual scheduling and coordination is time-consuming.<br/>Lack of centralized GDPR-compliant candidate data.<br/>Limited pipeline visibility and fragmented contract/onboarding workflows."]
  CS["Customer Segments:<br/>HR teams and recruiters<br/>Hiring managers<br/>Talent acquisition departments<br/>Job candidates (self-service portal)<br/>Interviewers within organizations"]
  EA["Early Adopters:<br/>SMBs scaling hiring<br/>Recruitment agencies managing multiple clients<br/>Tech companies hiring high volumes"]
  UVP["Unique Value Proposition:<br/>An intelligent interview and hiring platform that automates recruitment workflows while ensuring privacy, speed, and data-driven hiring decisions."]
  S["Solution:<br/>Interview scheduling with calendar integration<br/>Centralized candidate database with privacy protections<br/>Automated hiring workflows and follow-ups<br/>AI candidate ranking and matching<br/>Built-in technical and cognitive assessments<br/>Contract generation and e-signature workflow<br/>Candidate self-service portal<br/>Hiring analytics dashboards"]
  CH["Channels:<br/>Direct sales to HR departments<br/>SaaS website with free trial<br/>HR tech marketplaces<br/>LinkedIn and HR networks<br/>Partnerships with recruitment agencies<br/>Integrations with HR tools"]
  R["Revenue Streams:<br/>SaaS subscription model (monthly/annual)<br/>Tiered pricing by company size or recruiter count<br/>Premium features for AI analytics and interview intelligence<br/>Enterprise licensing and add-ons"]
  C["Cost Structure:<br/>Software development and maintenance<br/>Cloud infrastructure and hosting<br/>AI/ML model development<br/>Third-party integrations<br/>Sales, marketing, support, and onboarding"]
  M["Key Metrics:<br/>Time-to-hire reduction<br/>Active recruiters/companies<br/>Pipeline conversion rates<br/>Scheduling completion rate<br/>Retention and churn<br/>AI matching accuracy / recruiter usage rate"]
  A["Unfair Advantage:<br/>AI-powered candidate ranking using multiple signals<br/>Interview intelligence with automatic summaries and insights<br/>Privacy-by-design architecture for GDPR compliance<br/>Fully integrated workflow from application to contract signing"]

  class P,CS,S,CH,R,C section;
  class UVP uvp;
  class M metrics;
  class EA early;
  class A detail;

  P --> CS
  P --> UVP
  UVP --> S
  S --> CH
  CS --> R
  R --> C
  M --> R
  A --> UVP
  EA --> CS
```

