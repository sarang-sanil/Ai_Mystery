# AI Mystery Room — SPEC.md

**Version:** 1.0.0  
**Status:** Implementation Specification  
**Project Type:** AI-powered mystery investigation web application  
**Architecture:** MERN Stack  
**AI Provider:** Google Gemini API  
**Primary Coding Assistant:** OpenAI Codex  
**Deployment Goal:** A publicly accessible website with a complete playable mystery-solving experience.

---

# 1. Project Overview

## 1.1 Project Description

AI Mystery Room is an AI-powered web application that allows users to create and solve fictional mysteries. Users provide a short premise, select a difficulty level, and receive an interactive investigation generated using Google's Gemini API.

Each mystery contains a story, suspects, clues, and hints. Players investigate the available evidence, discover clues, request hints when needed, and accuse the suspect they believe is responsible.

After submitting an accusation, the application reveals whether the player was correct, identifies the actual culprit, explains the solution, and displays the final score.

The application must provide a complete end-to-end experience, from mystery creation to final verdict, on the deployed website.

## 1.2 Problem Statement

Traditional mystery games usually offer predefined stories with limited replayability. Creating a large collection of unique mysteries manually requires significant time and effort.

AI Mystery Room addresses this problem by using generative AI to create customized, playable investigations from user-provided scenarios.

However, AI-generated mysteries can contain contradictory evidence, inconsistent timelines, or solutions that cannot be logically deduced. Therefore, the application must validate each mystery before allowing users to play it.

## 1.3 Project Objectives

- Generate unique mysteries from user-provided premises.
- Provide three difficulty levels: Easy, Medium, and Hard.
- Generate the correct number of suspects, clues, and hints for each difficulty.
- Allow users to investigate and discover clues.
- Provide hints without revealing the solution prematurely.
- Evaluate accusations and calculate scores on the backend.
- Reveal the culprit and explanation only after accusation.
- Persist mystery data and player progress in MongoDB.
- Deploy the application so that another person can complete the entire game online.

## 1.4 Target Users

- Students learning full-stack development and AI integration.
- Mystery and detective-game enthusiasts.
- Users interested in interactive storytelling.
- Users who want to create and solve personalized fictional cases.

## 1.5 Core User Experience

The user should feel like a detective working through a structured investigation.

The interface must clearly communicate:

- What happened.
- Who the suspects are.
- What clues are available.
- Which clues have been discovered.
- How many hints remain.
- How the investigation is progressing.
- When the player is ready to accuse a suspect.

The experience must be intuitive, immersive, responsive, and playable without technical knowledge.

---

# 2. Scope and Features

## 2.1 Mandatory MVP Features

1. Home page with project introduction.
2. Mystery creation form.
3. Difficulty selection.
4. Gemini-powered mystery generation.
5. Structured suspect and clue generation.
6. Investigation room.
7. Clue discovery and progress tracking.
8. Hint system.
9. Suspect accusation.
10. Backend-controlled scoring.
11. Results and solution explanation.
12. MongoDB persistence.
13. Error handling and loading states.
14. Responsive design.
15. Deployment and live demonstration.

## 2.2 Optional Enhancements

These features must not delay completion of the core MVP:

- AI-generated evidence photographs.
- Fictional chat-message evidence.
- CCTV-style evidence and transcripts.
- Animated investigation boards.
- Sound effects and cinematic transitions.
- User accounts and case history.
- Downloadable case reports.
- Daily mystery challenges.
- Leaderboards.
- Multiplayer investigations.

The MVP must remain fully playable even if image or video generation is not implemented.

## 2.3 Project Limitations

- Mysteries are fictional and intended for entertainment.
- Gemini responses may occasionally be invalid or inconsistent; validation and bounded retries are required.
- Actual image or video generation requires a compatible media-generation provider. Gemini text output must not be represented as an actual video file.
- The application must not depend on an AI model to determine the final score.
- The application must not reveal the hidden solution through frontend state or public APIs.
- The project does not perform real-world criminal investigations.

---

# 3. Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend framework | React | Interactive user interface |
| Build tool | Vite | Development server and production build |
| Styling | Tailwind CSS | Responsive styling |
| Routing | React Router | Page navigation |
| Backend runtime | Node.js | Server-side execution |
| Backend framework | Express.js | REST API |
| Database | MongoDB | Persistent mystery and progress storage |
| ODM | Mongoose | Database models and validation |
| AI | Google Gemini API | Mystery generation |
| AI SDK | Official Google Gen AI JavaScript SDK | Gemini integration |
| Request validation | Zod | API and AI response validation |
| HTTP communication | Fetch or Axios | Frontend API calls |
| Testing | Vitest, Supertest | Unit and API integration tests |
| Deployment | Compatible frontend and backend hosting | Public website and API |

Use stable, mutually compatible package versions and commit the dependency lockfile.

## 3.1 Architecture

```text
React + Vite + Tailwind CSS
             |
             | REST API
             v
       Node.js + Express
             |
      +------+------+
      |             |
      v             v
 Gemini Service   Game Engine
      |             |
      v             +-- Clue Discovery
 Gemini API        +-- Hint Management
                    +-- Accusation
                    +-- Score Calculation
                           |
                           v
                     MongoDB
```

### Architecture rules

- The frontend must never call Gemini directly.
- The Gemini API key must remain on the backend.
- All scores must be calculated on the backend.
- The correct culprit and solution must remain private until the accusation is submitted.
- The frontend must not be trusted to report discovered clues or calculate scores.
- All generated mysteries must be validated before they become playable.

---

# 4. Complete User Flow

## Step 1: Home

The user visits the website and sees the product introduction, feature overview, and a prominent **Start a Mystery** button.

Clicking the button navigates to the Create Mystery page.

## Step 2: Create Mystery

The user enters a premise, for example:

"The owner of a private art gallery discovers that a valuable painting has disappeared overnight. Four people had access to the gallery."

The user selects Easy, Medium, or Hard.

The frontend validates the form and submits the request to the backend.

## Step 3: AI Generation

The backend sends a structured request to Gemini.

Gemini generates:

- Mystery title.
- Story and incident.
- Suspects.
- Clues.
- Hints.
- Hidden culprit.
- Canonical timeline.
- Explanation of the solution.

The backend validates the generated mystery and saves it in MongoDB.

The interface displays a loading screen until the case is ready.

## Step 4: Investigation Room

The player enters the investigation room and reviews the story and suspects.

Clues are initially undiscovered. The player selects clues to investigate them and reveal their contents.

The backend records each discovery and updates the player's progress.

## Step 5: Request Hints

The player may request hints according to the selected difficulty.

Hints should gradually guide the player toward relevant evidence without directly naming the culprit.

Every hint request is recorded by the backend and incurs a 10-point deduction.

## Step 6: Accusation

The player selects a suspect and submits the accusation.

The backend verifies the selected suspect, checks the investigation session, and calculates the final score using the official scoring rules.

## Step 7: Results

The results page reveals:

- Correct or incorrect accusation.
- Selected suspect.
- Actual culprit.
- Final score.
- Solution explanation.
- Important clues supporting the conclusion.
- The sequence of events.

## Step 8: Play Again

The user clicks **Create Another Mystery** to return to the creation form and begin a new investigation.

Each new mystery must have its own unique case ID and independent ground truth.

---

# 5. UI and Page Specifications

## 5.1 Home Page

**Route:** `/`

Purpose: Introduce the project and encourage the user to start investigating.

Required elements:

- Logo and navigation.
- Hero heading: "Every Story Hides a Secret."
- Short product description.
- Primary CTA: "Start a Mystery".
- How It Works section.
- Feature cards.
- Sample mystery preview.
- Footer.

Behavior:

- CTA opens the mystery creation page.
- Navigation links work.
- Page adapts to desktop, tablet, and mobile.
- Include loading and error states only where asynchronous content requires them.

## 5.2 Create Mystery Page

**Route:** `/create`

Purpose: Collect the user's premise and difficulty settings.

Required elements:

- Scenario textarea.
- Difficulty selector.
- Difficulty descriptions.
- Character count.
- Generate Mystery button.
- Validation messages.
- Loading state.

### Form validation

| Field | Rule |
|---|---|
| Premise | Required, 50–2,000 characters |
| Difficulty | Required; Easy, Medium, or Hard |
| Submission | Disabled while the same request is in progress |

Behavior:

- Reject empty or excessively long premises.
- Prevent repeated submissions while generation is running.
- Display understandable API errors.
- Preserve the entered premise if generation fails.
- Provide a retry action.

## 5.3 Investigation Room

**Route:** `/cases/:id`

Purpose: Allow the player to investigate the case.

Required elements:

- Case title.
- Story summary.
- Difficulty badge.
- Suspect cards.
- Clue inventory.
- Discovered-clue indicators.
- Hint button.
- Progress indicator.
- Accusation button.
- Loading and error states.

### Suspect cards

Each suspect includes:

- Name.
- Fictional profile.
- Relationship to the incident.
- Publicly available motive or background.
- Statement or alibi when relevant.

Suspect cards must not contain hidden culprit flags or internal solution notes.

### Clue cards

Each clue includes:

- Clue title.
- Short preview.
- Discovery status.
- Clue content after discovery.

The initial clue inventory may display titles or short descriptions, but hidden clue content must not be returned to the frontend before discovery.

### Progress

Display:

- Discovered clues / total clues.
- Hints used / available hints.
- Investigation status.

The backend is authoritative for progress.

## 5.4 Accusation Interface

**Route:** `/cases/:id/accuse`

Purpose: Collect the final suspect selection.

Required elements:

- List of suspects.
- Selectable suspect cards.
- Confirmation dialog.
- Submit Accusation button.
- Cancel action.

Behavior:

- A suspect must be selected before submission.
- The backend validates that the suspect belongs to the case.
- The application must warn the user that submission ends the investigation.
- The accusation must be recorded exactly once.
- Returning to the accusation page after completion must not allow the player to change the verdict.

## 5.5 Results Page

**Route:** `/cases/:id/result`

Purpose: Reveal the verdict and explain the solution.

Required elements:

- Verdict status.
- Final score.
- Accused suspect.
- Correct culprit.
- Full solution explanation.
- Decisive clues.
- Final timeline.
- Create Another Mystery button.

Behavior:

- Results are available only after the accusation has been submitted.
- The backend returns the stored verdict and ground truth.
- Refreshing the page must not change the result.
- The results page must work on the deployed website.

## 5.6 Navigation and Responsive Design

Desktop:

- Use a multi-column investigation layout.
- Keep suspect and clue navigation visible.
- Give the evidence content sufficient reading space.

Mobile:

- Use stacked sections or tabs.
- Keep buttons touch-friendly.
- Prevent horizontal overflow.
- Make suspect cards, clues, and hints readable.

All pages must include appropriate empty, loading, success, and error states.

---

# 6. Difficulty Configuration

Difficulty determines the exact number of suspects, clues, and hints.

| Difficulty | Suspects | Clues | Available hints | Base score for correct accusation |
|---|---:|---:|---:|---:|
| Easy | 3 | 5 | 2 | 100 |
| Medium | 4 | 7 | 3 | 150 |
| Hard | 5 | 9 | 3 | 200 |

These values are mandatory.

The backend must reject or repair a generated case that does not satisfy the selected difficulty configuration.

A hint is considered used only when the backend successfully returns and records it.

A clue is considered discovered only after the backend confirms the discovery.

---

# 7. AI Behavior and Mystery Generation

## 7.1 Generation Principles

Gemini must generate a complete mystery, not independent pieces of unrelated content.

The recommended process is:

1. Generate the incident and hidden solution.
2. Establish the culprit's identity and motive.
3. Create the suspect profiles.
4. Establish the canonical event timeline.
5. Generate the required clues.
6. Generate the required hints.
7. Explain how the clues support the solution.
8. Validate structural and logical consistency.
9. Save the validated case.
10. Mark the case as playable.

The solution must be established before clues and hints are finalized.

## 7.2 Required AI Output

The generated case must contain:

- Title.
- Story.
- Difficulty.
- Suspects.
- Clues.
- Hints.
- Hidden culprit ID.
- Actual timeline.
- Final solution explanation.
- Decisive clue IDs.
- Supporting clue IDs.
- Red-herring explanations, where applicable.

## 7.3 Example JSON Schema Shape

The following is a conceptual output structure. Implement equivalent strict Zod and Mongoose schemas.

```json
{
  "title": "The Missing Painting",
  "story": "A valuable painting disappears overnight.",
  "difficulty": "Easy",
  "suspects": [
    {
      "id": "suspect_01",
      "name": "Alex Morgan",
      "profile": "The gallery manager.",
      "statement": "I left before closing."
    }
  ],
  "clues": [
    {
      "id": "clue_01",
      "title": "The Access Log",
      "preview": "An unusual entry appears in the records.",
      "content": "Fictional access log details.",
      "relatedSuspectIds": ["suspect_01"],
      "discoveryOrder": 1
    }
  ],
  "hints": [
    {
      "level": 1,
      "text": "Compare the recorded times with the suspects' statements."
    }
  ],
  "solution": {
    "culpritId": "suspect_01",
    "motive": "A fictional motive.",
    "actualTimeline": [],
    "decisiveClueIds": ["clue_01"],
    "explanation": "The timeline and access records establish the solution."
  }
}
```

This is a structural example only, not a hardcoded mystery. Actual generation must satisfy the selected difficulty's exact counts.

## 7.4 Stable IDs

Every suspect and clue must have a unique ID within its case.

Rules:

- Suspect IDs must resolve to actual suspects.
- Clue IDs must resolve to actual clues.
- Decisive clue references must exist.
- Hint levels must be valid for the selected difficulty.
- IDs must not be reused between unrelated cases if doing so would create ambiguity in API or session handling.

## 7.5 Logical Consistency

Before saving a mystery, validate that:

- Exactly one culprit exists.
- The culprit ID refers to an existing suspect.
- The number of suspects and clues matches the selected difficulty.
- The number of hints matches the selected difficulty.
- The culprit's motive and opportunity are consistent with the timeline.
- Decisive clues support the final solution.
- Alternative suspects can be eliminated using available evidence.
- The explanation matches the generated story and clues.
- Hints do not reveal the culprit directly before accusation.
- The solution is inferable from evidence the player can discover.

A case that fails validation must not be playable.

## 7.6 Retry Policy

If Gemini returns invalid or inconsistent output:

1. Validate the response.
2. Identify the validation errors.
3. Attempt a targeted repair or regeneration.
4. Validate the new output.
5. Stop after a configurable number of attempts.
6. Mark the generation as failed if all attempts fail.

Use three repair/regeneration attempts after the initial generation as the default.

Never retry indefinitely or publish invalid output simply to complete a request.

---

# 8. Scoring Rules

All scoring must be calculated on the backend.

## 8.1 Formula

Let:

- `B` = base score for the selected difficulty.
- `D` = number of unique clues discovered.
- `H` = number of hints successfully used.
- `C` = 1 if the accusation is correct, otherwise 0.

Then:

`Final Score = max(0, (C × B) + (D × 5) − (H × 10))`

Base scores:

- Easy: 100.
- Medium: 150.
- Hard: 200.

## 8.2 Examples

| Difficulty | Accusation | Clues discovered | Hints used | Final score |
|---|---|---:|---:|---:|
| Easy | Correct | 5 | 0 | 125 |
| Easy | Correct | 4 | 2 | 100 |
| Medium | Correct | 7 | 1 | 175 |
| Medium | Incorrect | 5 | 2 | 5 |
| Hard | Correct | 9 | 3 | 215 |
| Hard | Incorrect | 0 | 3 | 0 |

## 8.3 Scoring Restrictions

- Clues count once per player per case.
- Repeated clue-discovery requests must not award additional points.
- Hints count only after successful server-side delivery and persistence.
- The frontend must not submit a score value.
- The backend must derive clue and hint counts from persisted progress.
- The accusation result must be calculated against the stored culprit ID.
- Scores cannot be changed by manipulating browser state or API payloads.
- The final score must be persisted with the completed result.

---

# 9. Backend API Specification

Base path: `/api`

All request and response bodies use JSON unless otherwise specified.

## 9.1 Create a Mystery

**Endpoint:** `POST /api/cases`

Request:

```json
{
  "premise": "A priceless painting disappears from a locked gallery.",
  "difficulty": "Easy"
}
```

Validation:

- `premise` is a string containing 50–2,000 characters.
- `difficulty` is exactly `Easy`, `Medium`, or `Hard`.
- Unknown fields must be rejected or safely stripped.
- Requests must be rate-limited.

Recommended response: `202 Accepted`.

```json
{
  "success": true,
  "data": {
    "caseId": "generated_case_id",
    "status": "generating"
  }
}
```

The backend creates the case record, starts generation, and updates the status when generation completes.

Possible statuses:

- `generating`
- `ready`
- `failed`
- `in_progress`
- `completed`

## 9.2 Retrieve a Mystery

**Endpoint:** `GET /api/cases/:id`

Returns the public case information:

- Case ID.
- Title.
- Story.
- Difficulty.
- Suspects.
- Clue titles and previews.
- Clue discovery state, where applicable.
- Progress and available actions.

The endpoint must not return the culprit ID, hidden solution, private clue contents, or decisive clue annotations before accusation.

If clue details are needed, retrieve them through the discovery endpoint.

## 9.3 Discover a Clue

**Endpoint:** `POST /api/cases/:id/clues/:clueId/discover`

Request:

```json
{}
```

Behavior:

1. Validate the case ID and clue ID.
2. Verify the case is playable.
3. Verify the clue belongs to the case.
4. Verify the investigation is not completed.
5. Record the discovery if it has not already occurred.
6. Return the clue content and updated progress.

Example response:

```json
{
  "success": true,
  "data": {
    "clueId": "clue_01",
    "discovered": true,
    "content": "Fictional access log details.",
    "discoveredClueCount": 1,
    "totalClues": 5
  }
}
```

Repeated requests may return the same clue content but must not award duplicate points.

## 9.4 Request a Hint

**Endpoint:** `POST /api/cases/:id/hints`

Request:

```json
{}
```

Behavior:

- Verify that the case is active.
- Verify that a hint remains available.
- Select the next unused hint level.
- Return and persist the hint.
- Increment the hint count exactly once.
- Never return the hidden solution.

Example response:

```json
{
  "success": true,
  "data": {
    "hintLevel": 1,
    "hint": "Compare the recorded times with the suspects' statements.",
    "hintsUsed": 1,
    "hintsRemaining": 1
  }
}
```

The backend must not allow clients to choose an arbitrary hint level or skip limits.

## 9.5 Submit an Accusation

**Endpoint:** `POST /api/cases/:id/accuse`

Request:

```json
{
  "suspectId": "suspect_01"
}
```

Behavior:

1. Validate the suspect ID.
2. Verify the suspect belongs to the case.
3. Verify the case is active.
4. Determine correctness from the stored culprit ID.
5. Calculate the score using the official formula.
6. Persist the accusation and result.
7. Mark the case completed.
8. Return the result.

The backend must not accept `isCorrect`, `culpritId`, `score`, or other authoritative values from the client.

Example response:

```json
{
  "success": true,
  "data": {
    "correct": true,
    "score": 125,
    "resultUrl": "/cases/generated_case_id/result"
  }
}
```

This response does not need to reveal the culprit immediately; the results endpoint can return the complete solution after completion.

## 9.6 Retrieve Results

**Endpoint:** `GET /api/cases/:id/result`

Behavior:

- Verify the case is completed.
- Retrieve the persisted verdict and solution.
- Return the actual culprit, explanation, timeline, score, and decisive clues.
- Reject attempts to retrieve results before accusation.

Example response:

```json
{
  "success": true,
  "data": {
    "correct": true,
    "score": 125,
    "accusedSuspectId": "suspect_01",
    "culpritId": "suspect_01",
    "explanation": "The access records contradict the suspect's statement.",
    "decisiveClueIds": ["clue_01"]
  }
}
```

## 9.7 Generation Status

**Endpoint:** `GET /api/cases/:id/status`

Returns the current generation status and, when ready, the public case ID and navigation information.

The client may poll this endpoint at a reasonable interval until generation completes or fails.

## 9.8 Error Format

Use a consistent response structure:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The selected difficulty is invalid."
  }
}
```

Recommended status codes:

| Status | Meaning |
|---|---|
| 400 | Invalid request |
| 401 | Authentication required, if accounts are enabled |
| 404 | Case or clue not found |
| 409 | Invalid case state or duplicate final submission |
| 422 | Generated case failed validation |
| 429 | Rate limit exceeded |
| 500 | Unexpected server error |
| 502 | AI provider failure |
| 503 | Required service temporarily unavailable |

Do not expose stack traces, API keys, raw provider errors, or private solution data.

---

# 10. MongoDB Data Model

For the initial MVP, cases and their case-specific data may be stored as embedded documents within a `cases` collection. This simplifies consistent retrieval of the case and its ground truth.

## 10.1 Case Document

```js
{
  _id: ObjectId,

  premise: String,
  title: String,
  story: String,
  difficulty: String,

  status: String,

  suspects: [
    {
      id: String,
      name: String,
      profile: String,
      statement: String
    }
  ],

  clues: [
    {
      id: String,
      title: String,
      preview: String,
      content: String,
      relatedSuspectIds: [String],
      discoveryOrder: Number
    }
  ],

  hints: [
    {
      level: Number,
      text: String
    }
  ],

  solution: {
    culpritId: String,
    motive: String,
    actualTimeline: Array,
    decisiveClueIds: [String],
    explanation: String
  },

  playerProgress: {
    discoveredClueIds: [String],
    hintsUsed: Number,
    accusedSuspectId: String,
    finalScore: Number,
    isCorrect: Boolean,
    completedAt: Date
  },

  generation: {
    model: String,
    attempts: Number,
    generatedAt: Date
  },

  createdAt: Date,
  updatedAt: Date
}
```

This is a conceptual structure; implement concrete nested Mongoose schemas rather than unrestricted generic objects.

## 10.2 Data Rules

- Difficulty must be an enum.
- Status must be an enum.
- Suspect and clue IDs must be unique within a case.
- `solution` must be stored separately from the public response representation.
- `discoveredClueIds` must contain only valid clue IDs.
- `hintsUsed` must never exceed the allowed number of hints.
- `finalScore` must be calculated by the backend.
- `completedAt` must be populated when the accusation is accepted.
- A completed case must not accept additional discoveries or hints.

## 10.3 Progress Persistence

The player's progress must survive a page refresh.

For the initial MVP, one investigation session per case is sufficient.

If the application later supports accounts, multiple players, or replaying a case, separate the player progress into a `gameSessions` collection with `ownerId`, `caseId`, `discoveredClueIds`, `hintsUsed`, `accusation`, and `finalScore`.

---

# 11. Security Requirements

## 11.1 Culprit Confidentiality

This is a mandatory security requirement.

Before accusation:

- The frontend must not receive the culprit ID.
- The frontend must not receive the final explanation.
- The frontend must not receive private clue annotations that reveal the answer.
- The results endpoint must reject requests for incomplete cases.
- Public case serialization must use an explicit allowlist.

Hiding a field in React is not sufficient. The field must never be returned by the backend.

## 11.2 Input Validation

Validate all requests using Zod or equivalent validation.

Validate:

- Premise length.
- Difficulty values.
- Case IDs.
- Clue IDs.
- Suspect IDs.
- Case status.
- Hint limits.
- Accusation state.

Never trust client-submitted scores or progress counts.

## 11.3 AI Security

- Treat the user premise as untrusted input.
- Keep system instructions separate from user content.
- Require structured output.
- Validate all Gemini responses.
- Do not execute AI-generated code.
- Do not trust arbitrary generated URLs.
- Do not expose hidden solution data in application logs.

## 11.4 API Protection

- Apply request rate limits.
- Configure CORS for the deployed frontend origin.
- Use secure HTTP headers.
- Set request-size limits.
- Use safe error messages.
- Keep secrets in environment variables.
- Use HTTPS in production.

If accounts are introduced, every private case and session endpoint must enforce ownership checks.

---

# 12. Error Handling and Reliability

## 12.1 Gemini Failures

If Gemini is unavailable, rate-limited, or returns invalid output:

- Do not save an invalid playable case.
- Display a clear error message.
- Preserve the user's premise.
- Provide a retry option.
- Apply bounded retries with appropriate backoff for transient failures.

## 12.2 Database Failures

If MongoDB is unavailable:

- Do not claim that a case has been saved.
- Return an appropriate error.
- Log the failure without logging secrets.
- Allow retry when safe.

## 12.3 Invalid Operations

Reject:

- Discovering nonexistent clues.
- Requesting more hints than allowed.
- Accusing a nonexistent suspect.
- Accusing a suspect from another case.
- Changing a completed result.
- Requesting a result before accusation.
- Repeated requests that would modify completed-case state.

## 12.4 Frontend States

Every asynchronous operation must include:

- Loading state.
- Success state.
- Error state.
- Retry or recovery action where appropriate.

---

# 13. Testing Requirements

## 13.1 Unit Tests

Test:

- Difficulty configuration.
- Exact suspect, clue, and hint counts.
- AI output validation.
- Unique ID validation.
- Ground-truth consistency.
- Clue discovery idempotency.
- Hint limits.
- Scoring formula.
- Correct and incorrect accusations.
- Results access restrictions.

## 13.2 API Integration Tests

Test the full API journey:

1. Create a mystery using a mocked Gemini response.
2. Check generation status.
3. Retrieve the public case.
4. Confirm the solution is hidden.
5. Discover a clue.
6. Request a hint.
7. Submit an accusation.
8. Retrieve the result.
9. Confirm the score and explanation are correct.

Also test invalid requests, provider failures, and duplicate submissions.

## 13.3 Security Tests

Verify that:

- The solution is absent from pre-accusation responses.
- A user cannot change their score through an API payload.
- A user cannot discover a clue from another case.
- Repeated discovery does not award additional points.
- Hint limits cannot be bypassed.
- Results cannot be retrieved before completion.
- Completed cases cannot be modified through investigation endpoints.

## 13.4 Automated Testing Rules

Ordinary tests must use mocked Gemini responses rather than requiring a live API key.

A separate optional smoke test may verify real Gemini connectivity.

---

# 14. Deployment Specification

## 14.1 Deployment Goal

The application must be accessible through a public URL and support a complete mystery-solving session without requiring a developer to run the code locally.

## 14.2 Deployment Components

Deploy:

- React frontend as a static production build.
- Node.js and Express backend as a running web service.
- MongoDB through a hosted database or another appropriately accessible MongoDB deployment.
- Gemini integration using a backend-only API key.

Select hosting services that support the required runtime and environment configuration.

## 14.3 Production Configuration

Configure:

- Frontend API base URL.
- Backend port and environment.
- MongoDB connection string.
- Gemini API key.
- Gemini model setting.
- Allowed frontend origins.
- Production HTTPS.
- Health endpoint.
- Error logging.

Do not commit production credentials to Git.

## 14.4 Deployment Verification

After deployment:

1. Open the public website.
2. Confirm that the home page loads.
3. Create an Easy mystery.
4. Verify three suspects, five clues, and two available hints.
5. Discover clues and request a hint.
6. Submit an accusation.
7. Verify the correct score.
8. Confirm that the result reveals the culprit and explanation.
9. Repeat the journey at Medium and Hard difficulty.
10. Confirm that invalid or failed generation produces a usable error state.

A successful local build is not sufficient. The live website must pass the complete user journey.

---

# 15. Acceptance Criteria

The project is considered complete only when all mandatory requirements pass.

### Mystery generation

- [ ] Users can submit a premise.
- [ ] Users can select Easy, Medium, or Hard.
- [ ] Gemini generates the mystery through the backend.
- [ ] The generated case contains exactly the required suspect, clue, and hint counts.
- [ ] The mystery has one predetermined culprit.
- [ ] The generated solution is consistent with the clues.
- [ ] Invalid output is rejected or repaired before play.

### Investigation

- [ ] The story and suspects are displayed correctly.
- [ ] Players can discover clues.
- [ ] Clue discovery is persisted.
- [ ] Repeated discoveries do not award duplicate points.
- [ ] Players can request hints within their difficulty limit.
- [ ] Hint usage is persisted.
- [ ] The interface displays investigation progress.

### Accusation and results

- [ ] Players can accuse a suspect.
- [ ] The backend determines correctness.
- [ ] The backend calculates the score using the specified formula.
- [ ] The minimum score is zero.
- [ ] The culprit remains hidden before accusation.
- [ ] Results reveal the correct culprit and explanation.
- [ ] Results persist after refresh.
- [ ] A completed case cannot be accused again to change the result.

### UI and deployment

- [ ] All five required screens are implemented.
- [ ] Responsive behavior works on desktop and mobile.
- [ ] Loading, error, and empty states are implemented.
- [ ] MongoDB persistence works in production.
- [ ] Gemini works using server-side credentials.
- [ ] The application is accessible through a public URL.
- [ ] The full mystery-solving journey works on the live website.

---

# 16. Implementation Phases

## Phase 1: Project Foundation

- Set up the repository.
- Configure React and Vite.
- Configure Tailwind CSS.
- Configure Node.js and Express.
- Connect MongoDB.
- Add environment validation.
- Add a health endpoint.
- Implement global error handling.

**Exit condition:** The frontend and backend run, and the database connection is verified.

## Phase 2: User Interface

- Build the home page.
- Build the creation form.
- Build the investigation room.
- Build the accusation interface.
- Build the results page.
- Implement responsive layouts and loading states.

**Exit condition:** The complete navigation flow is implemented.

## Phase 3: Gemini and Case Generation

- Implement the Gemini service.
- Create structured generation prompts.
- Add strict schemas.
- Implement validation and bounded retries.
- Persist validated mysteries.
- Implement generation status.

**Exit condition:** A valid mystery can be generated and stored for each difficulty.

## Phase 4: Investigation Engine

- Implement case retrieval.
- Implement clue discovery.
- Implement hint management.
- Persist player progress.
- Implement the accusation engine.
- Implement deterministic scoring.
- Implement the results endpoint.

**Exit condition:** The complete gameplay journey works against the backend.

## Phase 5: Testing and Security

- Write unit tests.
- Write API integration tests.
- Verify solution confidentiality.
- Test invalid requests.
- Test repeated requests.
- Verify all difficulty configurations.

**Exit condition:** All mandatory tests pass.

## Phase 6: Deployment

- Configure production environment variables.
- Deploy the frontend.
- Deploy the backend.
- Connect production MongoDB.
- Configure Gemini credentials.
- Run live smoke tests.
- Complete a full mystery on the deployed website.

**Exit condition:** The public website successfully passes the acceptance criteria.

---

# 17. Codex Implementation Rules

1. Read `SPEC.md` before making changes.
2. Inspect the existing repository before creating or overwriting files.
3. Implement the project incrementally by phase.
4. Preserve the required MERN stack and Gemini integration.
5. Do not replace real functionality with hardcoded frontend demonstrations.
6. Never hardcode a single culprit for all mysteries.
7. Never reveal the solution through a public API before accusation.
8. Keep business logic in backend services rather than React components.
9. Validate all AI output before persistence.
10. Add tests alongside core backend functionality.
11. Keep `.env` and credentials out of version control.
12. Do not claim tests passed unless they were actually executed.
13. Document configuration and commands in `README.md`.
14. If a requirement conflicts with the current repository, explain the conflict and propose a minimal compatible solution.
15. Update this specification and its tests together whenever a material requirement changes.

---

# 18. Final Deliverables

The completed project must contain:

1. `SPEC.md` — authoritative product specification.
2. `README.md` — setup, configuration, development, and deployment instructions.
3. React frontend.
4. Express backend.
5. MongoDB/Mongoose models.
6. Gemini generation service.
7. Case validation and investigation engine.
8. Deterministic scoring implementation.
9. Automated tests.
10. `.env.example` with placeholder configuration.
11. Production deployment configuration.
12. Public website URL.
13. Demonstration of a complete mystery-solving session.

---

# 19. Definition of Done

AI Mystery Room is complete when a user can open the live website, enter a custom premise, select a difficulty, receive a valid AI-generated mystery, investigate suspects and clues, request hints, accuse a suspect, and see a correct verdict explanation and score.

The application must satisfy the exact difficulty configuration and scoring formula defined in this document.

The backend must remain the authority for mystery validity, clue discovery, hint usage, culprit identity, verdict correctness, and final score.

**Final instruction to Codex:** Implement this specification as a complete, tested, deployable MERN application powered by Gemini. Prioritize a fair and logically solvable mystery, secure separation of public evidence and hidden ground truth, deterministic scoring, and a working live demonstration. Do not stop at a landing page, static mockup, or partially connected prototype.
