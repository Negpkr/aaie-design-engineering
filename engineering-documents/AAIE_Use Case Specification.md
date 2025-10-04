# AAIE (Artificial Assessment Intelligence for Educator) Platform

## Use Case Specification

**Version:** 1.2  
**Scope:** This document defines the core use cases for the AAIE platform, focused on educator and system admin interactions for prompt feedback, AI usage detection, rubric alignment, and reporting.

---

## Table of Contents
1. [Actor Descriptions – AAIE Platform](#actor-descriptions--aaie-platform)  
2. [Phase 1: Educator Interaction](#phase-1-educator-interaction)  
   - [Use Case 1: Submit Student Work for Evaluation](#use-case-1-submit-student-work-for-evaluation)  
   - [Use Case 2: View Results](#use-case-2-view-results)  
   - [Use Case Document – Educator Interaction (Phase 1)](#use-case-document--educator-interaction-phase-1)
   - [Related Artifacts](#related-artifacts)
3. [Phase 2](#phase-2)  
   - [Student Interaction with AAIE Platform](#student-interaction-with-aaie-platform)  
   - [System Notes & Decisions](#system-notes--decisions)  
4. [Future Scope Use Cases](#future-scope-use-cases)  

---
## Actor Descriptions – AAIE Platform

| Phase       | Actor Name                                | Type              | Description |
|-------------|-------------------------------------------|-------------------|-------------|
| **Phase 1** | **Educator**                              | Primary User      | An instructor or academic staff member who:<br>• Uploads only student submissions (no prompts, no chat logs in current scope).<br>• Initiates AI evaluation.<br>• Reviews:<br>&nbsp;&nbsp;- AI classification (Human, AI, Hybrid)<br>&nbsp;&nbsp;- Four rubric-based scores: Conceptual Understanding, Real-World Application, Critical Evaluation, Academic Writing.<br>&nbsp;&nbsp;- LLM short feedback.<br>• Adds teacher feedback manually to complement AI feedback.<br>• Downloads structured report.<br>_They do not have access to model configuration or synthetic data generation tools._ |
| **Phase 1** | **System Admin / Developer**              | Technical Actor   | A backend administrator or developer responsible for managing the system’s infrastructure, deploying model APIs, configuring workflows, and generating synthetic datasets. They ensure system functionality, integrate model updates, and manage GitHub workflows.<br>**In the Current Scope:**<br>• Manages platform setup and API integration with the AI Classifier.<br>• Monitors system health and ensures evaluation process works.<br>• No direct role in day-to-day educator workflow. |
| **Phase 1** | **LLM Model (External System)**           | Support System    | The connected language model (e.g., GPT or Claude) that receives prompt and response data and returns feedback, rubric alignment, similarity scores, and hallucination detection. Operates via API and is monitored by the development team.<br>**In the Current Scope:**<br>• Receives student submission text.<br>• Returns:<br>&nbsp;&nbsp;- Classification: Human / AI / Hybrid.<br>&nbsp;&nbsp;- Four rubric scores (qualitative, e.g., Good, Average).<br>&nbsp;&nbsp;- Short written feedback from LLM. |
| **Phase 2** | **Student (Reference Only)**              | Future User       | While not implemented in the current sprint, this actor is considered for design purposes. A student would eventually interact with the platform to submit responses, receive feedback, and track improvements via OnTrack. Currently, all student responses are uploaded by educators or simulated via GenAI. |
| **Phase 2**    | **Role-Based Access Control (RBAC)**      | Support System    | **Authentication Layer:** RBAC is enforced right after login.<br>**Frontend Restrictions:** Users see only UI components allowed by their role.<br>**Backend Gatekeeping:** API endpoints enforce RBAC rules to block unauthorized actions (e.g., student accessing educator tools).<br>**Audit Logging:** All role-based access attempts are logged. |
| Optional    | **Prompt Owner / Researcher**             | Optional          | May be a stakeholder analyzing evaluation results or conducting academic integrity analysis. Overlaps with educator but may include permissions for accessing analytics or summary reports. |
| Phase 2        | **OnTrack (System Component)**            | Internal System   | The current platform used to monitor student progress. Supports assignment submission and feedback. Future plans include deeper student interactivity, learning analytics, viewing AI/LLM-generated feedback, and assigning rubric scores. |
| Both        | **GitHub Workflow / Repository**          | Passive Actor     | The GitHub repository and pull request workflows manage documentation updates, diagram reviews, and team contributions. While not a person, it acts as a system actor in the technical flow. |

---

## Phase 1: Educator Interaction

---

### Use Case 1: Submit Student Work for Evaluation

**Actor:** Educator  
**Goal:** Upload only the student submission for analysis.

**Main Flow:**
- Educator logs in.
- Uploads student submission text.
- Submits for evaluation.

**System Response:**
- Passes submission to AI Classifier.
- Receives:
  - Classification: Human / AI / Hybrid.
  - Feedback scores (4 criteria only — Conceptual Understanding, Real-World Application, Critical Evaluation, Academic Writing).
  - LLM feedback (short text).
- Allows educator to add teacher feedback (human-written).

**Postcondition:** Results are stored and available for review.

---

### Use Case 2: View Results

**Actor:** Educator  

**Main Flow:**
1. Navigate to **"My Submissions"**.
2. View:
   - AI classification.
   - Scores (Good / Average / etc.).
   - LLM feedback.
   - Teacher feedback.
3. View structured report.

---

### Transferred to Future Scope
- Student prompt submission.
- Prompt similarity %, plagiarism scoring, AI content scoring scale.
- Student direct interaction with system (Phase 2).
- Multi-criteria numeric scoring & future scope analytics.

---

## Use Case Document – Educator Interaction (Phase 1)

![Educator Use Case Diagram – Phase 1](../product-diagram/UseCaseDiagram_Educator(Phase1).png)

### Purpose
This document outlines the functional interactions between the **Educator** and the **LLM Engine (System Actor)** within the AAIE system for Phase 1.

In the current scope, AAIE enables educators to:
- Submit student work for automated classification and basic feedback (four rubric areas)
- Add their own comments
- Produce a final evaluation report

---

### Actors

1. **Educator (Academic Staff)**  
   The main human user. Responsible for:
   - Uploading student work
   - Initiating AI classification and feedback processes
   - Reviewing AI results
   - Adding teacher-written feedback to complement AI output
   - Downloading the final report  

   **Notes:**
   - Triggers most use cases.
   - Must be authenticated (via login).

2. **LLM Engine (System Actor)**  
   Represents the backend AI component responsible for:
   - Detecting AI-generated content and classifying as Human, AI, or Hybrid
   - Generating short, formative feedback in four rubric categories:  
     1. Conceptual Understanding  
     2. Real-World Application  
     3. Critical Evaluation  
     4. Academic Writing  
   - Returning all results to the educator interface.

---

### Use Cases (Educator Perspective)

| Use Case | Description |
|----------|-------------|
| **Register Account** | If the educator is new, they must create an account before using the system. |
| **Login to Account** | Authenticates educator before granting platform access. |
| **Upload Student Work** | Allows educator to submit student assignments for evaluation (no prompt or rubric upload in current scope). |
| **Detect AI Usage** | System starts analysis to detect AI-generated content. |
| **Classify AI vs Human** | System assigns category: Human / AI / Hybrid. |
| **Generate Feedback** | LLM generates short feedback for each rubric category. |
| **Approve Comparative Feedback** | Educator reviews AI feedback, adds teacher comments, and confirms the final evaluation. |
| **View Feedback Report** | Educator views combined AI + teacher feedback. |

---

### Notes
- This use case diagram is **Educator-focused (Phase 1)**.  
  Future phases (e.g., Student or Admin views) will have additional actors and interactions such as **Enforce Role-Based Access Control (RBAC)**.
- Many originally planned actions (e.g., rubric upload, prompt similarity analysis, plagiarism scoring) have been moved to **Future Scope**.
- All actions are dependent on the user's role and prior steps (e.g., login must precede all actions).
- The **LLM Engine** is a passive system actor; it performs logic based on triggers from the educator interface.

---

### Key Roles and Their Permissions

| Role | Permissions & Access |
|------|----------------------|
| **Educator (Academic Staff)** | - Upload and evaluate student submissions.<br>- Trigger AI classification (Human / AI / Hybrid).<br>- View AI-generated rubric scores and feedback.<br>- Add teacher-written feedback to the AI output.<br>- Download the combined AI + teacher feedback report. |
| **Admin / System Developer** | - Manage educator accounts and platform settings.<br>- Maintain and update API connections to the AI Classifier.<br>- Monitor system health, logs, and error reports.<br>- Support troubleshooting for educators.<br>- Deploy minor feature updates (within current scope). |
| **LLM Engine (System Actor)** | - Process student submissions sent from the platform.<br>- Perform AI content detection and classification.<br>- Generate short feedback for four rubric categories.<br>- Return results to the educator interface for review. |

---

## Related Artifacts
- System Design and Context Diagram
- Requirement Analysis and Documentation
- Prompt Evaluation and Response Enhancement
- Backend Logic Design and Class Diagram
- Use Case and Persona Documentation (Future Scope)
- API & Technical Alignment (Define Core API Calls)
- Frontend Flow and Activity Diagram
- Database Design (Domain Model)
- Identify Datatypes, Input Methods, and Validation Documentation

---



## Phase 2:

### Student Interaction with AAIE Platform

![Student Use Case Diagram – Phase 2](../product-diagram/UseCase_diagram_Student_Future_Scope.png)

**Purpose**  
This use case illustrates how students engage in academic work using GenAI tools and OnTrack, and how the backend AAIE system supports analysis, feedback, and evaluation. It emphasizes system automation, AI detection, and student transparency, without providing students direct access to the AAIE platform.

---

### Actors

| Actor | Description |
|-------|-------------|
| **Student** | Uploads responses via OnTrack, uses GenAI to assist with responses, and downloads feedback. They do not interact directly with the AAIE backend. |
| **OnTrack** | The university’s learning management platform through which students upload assignments and view feedback. Acts as the bridge between students and the AAIE platform. |
| **LLM Model (System Actor)** | Responsible for analyzing submissions, classifying AI-generated content, detecting usage, and scoring rubric alignment inside the AAIE system. Operates entirely behind the scenes. |
| **+ Phase 1 and Educator Actors** | All actors defined in Phase 1 also apply where relevant. |

---

### Main Use Cases (Student Perspective)

1. **Login to Account**  
   - If not logged in, redirect to authentication.  
   - Students access their GenAI or OnTrack account to start the academic task workflow.

2. **Upload Prompts Used in GenAI**  
   - Students disclose the prompts they used to generate AI responses.  
   - Promotes transparency and allows the system to later verify AI usage and integrity.

3. **Upload AI-Generated Response**  
   - Students submit their GenAI-assisted work.  
   - Submissions are shared to OnTrack and sent into the AAIE system for analysis.

4. **Send Student Assignment (via OnTrack)**  
   - OnTrack sends the student submission to the AAIE backend.  
   - Acts as the integration point that moves submissions from front-facing platforms into the backend analysis flow.

5. **Analyze Submission (System Use Case)**  
   - LLM model analyzes the input using multiple layers:  
     - **Classify AI vs Human:** Identifies whether the response is likely written by a human or AI.  
     - **Detect AI Usage and Rubric Score:** Compares structure, clarity, and relevance against rubric criteria.  
     - **Flag Areas for Review:** Marks segments that need human or educator review.

6. **Download / View Feedback**  
   - Students receive evaluated feedback from AAIE, routed back via OnTrack.

7. **Resubmit (If Needed)**  
   - In case feedback requires revision, students may revise and resubmit the updated work for reevaluation.

---

### Educator Use Case Diagram

![Educator Use Case Diagram – Phase 2](../product-diagram/UseCaseDiagram_Educator(phase2).png)

---

### Use Cases (Educator Perspective)

| Use Case | Description |
|----------|-------------|
| **Register Account** | If the educator is new to the system, they must register. |
| **Login to Account** | Authenticates user before allowing access to any platform functionality. |
| **Upload Rubric** | Educator uploads marking rubric to align system feedback. |
| **Approve Rubric Deployment (LLM)** | Backend reviews the rubric and sets it as active for evaluations. |
| **Upload Student Work** | Allows educators to upload student assignments for evaluation. |
| **Detect AI Usage** | Starts an analysis for AI-generated content detection. |
| **Analyze Submission (LLM)** | The system scans the file, segments content, and checks against AI patterns. |
| **Classify AI vs Human (LLM)** | System generates a confidence score comparing human and AI features. |
| **Generate Feedback (LLM)** | The LLM generates formative feedback for the submission. |
| **Align with Rubric (LLM)** | Ensures generated feedback corresponds to educator-provided rubric. |
| **Flag Areas for Review (LLM)** | Highlights segments that may need manual checking. |
| **Approve Comparative Feedback** | Educator reviews and confirms the system's feedback. |
| **Download Feedback Report** | Educator can download the structured report for archival or sharing. |
| **View Revision/Prompt History** | Access logs of past submissions, prompts, and revision actions. |
| **Enforce Role-Based Access Control (RBAC) (System)** | System checks role before granting access or triggering logic. |

---

### System Notes & Decisions

- **RBAC Not Required Here:** Because students do not directly access AAIE, RBAC for students is not implemented in this flow.  
- **Student Privacy Maintained:** Students interact only with OnTrack, keeping backend detection hidden to avoid anxiety and maintain academic integrity.  
- **Use of GenAI:** Students are allowed to use GenAI, but must declare prompts and submit a final human-edited version.  
- **System Advice (from PO):** System should not just evaluate for AI usage, but also include:  
  - Prompt quality  
  - Rubric alignment  
  - Authenticity scoring  

---

### Design Considerations

- **Two-Tier Evaluation:** The platform uses GenAI detection and rubric-based scoring for multidimensional feedback.  
- **Feedback Report:** Includes breakdowns by rubric area and AI-use likelihood.  
- **No Student Access to AAIE:** Ensures academic integrity and enforces platform security.

---

## Future Scope Use Cases

### Deploy/Update LLM Model
**Actor:** System Admin / Developer  
**Goal:** Update the backend model used for prompt evaluation.

**Main Flow:**
1. Admin logs into system dashboard.
2. Selects **"Model Deployment"** section.
3. Uploads or configures API endpoints.
4. Runs test prompt and evaluates response.
5. Finalizes deployment.

**System Response:**
- Validates endpoint.
- Integrates model into feedback loop.
- Begins tracking hallucination and bias logs.

**Postcondition:** New model is now serving API requests.

---

### Generate Synthetic Prompts and Responses
**Actor:** System Admin / Developer  
**Goal:** Populate platform with synthetic data using GenAI.

**Main Flow:**
1. Admin navigates to **"Data Simulation"**.
2. Enters desired scenario (e.g., argumentative prompt).
3. System generates synthetic student response.
4. Pairs response with rubric.
5. Submits automatically to pipeline.

**System Response:**
- Processes as normal student submission.
- Flags as **"Synthetic"** for later analysis.

**Postcondition:** Platform contains testable data for rubric alignment and detection.

---

### Monitor and Evaluate Model Output
**Actor:** LLM / Model Team  
**Goal:** Track feedback quality, hallucination rates, and model performance.

**Main Flow:**
1. Team accesses system logs or dashboards.
2. Reviews flagged responses (bias/hallucination).
3. Reviews rubric match scores.
4. Marks bad outputs for improvement.
5. Refines prompt format or LLM endpoint.

**Postcondition:** Model outputs iteratively improved over time.

---

### Security and Access Notes
- **Role-based access (RBAC)** ensures only authorized users can submit or configure models.
- Educators cannot access model configuration panels.
- Data flagged as synthetic is excluded from final reports.

---

### Access Control Logic
RBAC is enforced at multiple interaction points:
- If the user is not logged in, no data submission or access is allowed.
- Students and educators have different interface views and API permissions.
- The system internally verifies access rights before exposing detection results or feedback.
