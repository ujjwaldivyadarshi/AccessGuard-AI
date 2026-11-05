# ACCESSGUARD AI – OFFICIAL MASTER PROJECT PROMPT

## PROJECT CONTEXT

We are a team of three B.Tech Computer Science students participating in **HACK-DAY 2026**, a 24-hour hackathon.

**Project Name:** AccessGuard AI

**Problem Statement #09:** Real-Time Website Accessibility Testing Platform

**Project Title:** AI-Powered Real-Time Website Accessibility Testing Platform

**Objective:** Build a functional web application that automatically identifies accessibility problems on websites, explains their impact on users with disabilities, and recommends practical corrections.

Our goal is to build a technically impressive, reliable, demonstrable product within 24 hours.

All three members are beginners, so provide step-by-step instructions and explain important code and technical concepts.

---

## 1. TEAM MEMBERS AND RESPONSIBILITIES

**Member 1 – Ujjwal Divyadarshi (Backend and Scanner Lead)**

Responsible for:
- Python Flask backend
- REST API development
- Playwright browser automation
- axe-core accessibility scanning
- URL validation and security
- Extracting accessibility violations
- Processing results and returning JSON
- Scanner error handling

Files owned:
- `backend/app.py`
- `backend/scanner.py`
- `backend/requirements.txt`

**Member 2 – Shivam Kumar (Frontend Lead)**

Responsible for:
- HTML, CSS and JavaScript
- Professional responsive dashboard
- URL input and Scan Website button
- Loading states and error messages
- Accessibility score visualization
- Violation cards and severity filters
- Displaying AI recommendations
- Downloadable report interface
- Integration with Flask API

Files owned:
- `frontend/index.html`
- `frontend/style.css`
- `frontend/script.js`

**Member 3 – Krishnanand Krishn (AI and Testing Lead)**

Responsible for:
- AI explanation and recommendation engine
- Gemini or OpenAI API integration
- Structured AI output
- Suggested HTML/CSS corrections
- AI error handling and fallback
- Test cases and sample data
- Testing scanner output
- Checking technical accuracy of AI recommendations

Files owned:
- `backend/ai_helper.py`
- `testing/test_cases.md`
- `testing/sample_results.json`

No member should independently rewrite another member's module.

---

## 2. FIXED TECHNOLOGY STACK

Frontend:
- HTML5
- CSS3
- Vanilla JavaScript
- Fetch API

Backend:
- Python 3
- Flask
- Flask-CORS

Scanning:
- Playwright for Python
- Chromium browser
- axe-core JavaScript accessibility engine

AI:
- Gemini API or OpenAI API (final provider to be agreed by the team)
- AI credentials stored in environment variables, never committed to GitHub

Collaboration:
- Git
- GitHub

Data:
- JSON

Database:
- No database required for the initial MVP.

Do not introduce React, Next.js, Django, MongoDB or other major frameworks without agreement from the entire team.

---

## 3. PROJECT ARCHITECTURE

The application follows this flow:

1. User enters a public website URL in the frontend.
2. JavaScript sends a POST request to the Flask `/scan` API.
3. Flask validates the URL.
4. Playwright launches Chromium and loads the webpage.
5. axe-core runs accessibility checks inside the webpage.
6. Python collects and normalizes the findings.
7. The backend calculates a custom accessibility score.
8. The optional AI helper explains findings and suggests corrections.
9. Flask returns structured JSON.
10. JavaScript displays results in the dashboard.
11. Users can download a report.

**Critical requirement:** The scanner must work even if the AI API fails or is unavailable.

For the hackathon MVP, scan one webpage per request, not an entire website.

---

## 4. COMMON FOLDER STRUCTURE

```text
AccessGuard-AI/
├── backend/
│   ├── app.py
│   ├── scanner.py
│   ├── ai_helper.py
│   └── requirements.txt
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── testing/
│   ├── test_cases.md
│   └── sample_results.json
├── PROJECT_SPEC.md
├── .gitignore
└── README.md
```

The backend may use a locally installed copy of axe-core from npm or a pinned JavaScript asset. The scanner developer must document how the file is installed and loaded.

---

## 5. FIXED API CONTRACT – VERSION 1

Endpoint:

`POST /scan`

Request body:

```json
{
  "url": "https://example.com"
}
```

Successful response format:

```json
{
  "url": "https://example.com",
  "status": "success",
  "total_violations": 1,
  "score": 85,
  "score_method": "custom_weighted_violations_v1",
  "summary": {
    "critical": 1,
    "serious": 0,
    "moderate": 0,
    "minor": 0
  },
  "violations": [
    {
      "id": "image-alt",
      "impact": "critical",
      "description": "Images must have alternative text",
      "help": "Images must have alternative text",
      "help_url": "https://dequeuniversity.com/rules/axe/4.10/image-alt",
      "affected_elements": [
        {
          "html": "<img src=\"logo.png\">",
          "target": ["img"]
        }
      ],
      "ai_explanation": null,
      "suggested_fix": null
    }
  ],
  "ai_status": "unavailable"
}
```

This JSON is **illustrative mock data**, not the actual result of scanning example.com. The score is also illustrative.

### API contract rules

- Keep all agreed JSON field names unchanged.
- `score` is an integer from 0 to 100, calculated by a documented custom formula.
- `score` is not an official WCAG compliance score.
- `impact` follows axe-core severity values: critical, serious, moderate and minor.
- `affected_elements` contains relevant HTML snippets and selectors.
- `ai_explanation` and `suggested_fix` may be null.
- `ai_status` should indicate whether AI output is available, unavailable or failed.
- `total_violations` counts violation rules, not necessarily individual affected elements.
- A scan with zero automated violations must not be described as fully accessible.
- Failed scans return a clear error JSON with an appropriate HTTP status.
- If the team needs to change the API schema, all three members must approve and update `PROJECT_SPEC.md`.

### AI module interface

The AI helper should expose a function similar to:

`explain_violations(violations)`

Input: normalized accessibility violations.

Output: structured explanations and suggested fixes mapped to their original violations.

The AI helper must not directly control Playwright or modify the frontend.

---

## 6. REQUIRED MVP FEATURES

Priority 1 – Essential:
- URL input
- Real webpage scanning
- Actual axe-core violation detection
- Severity classification
- Affected HTML elements
- Results dashboard
- Clear error handling

Priority 2 – Important:
- Custom accessibility score
- AI explanations and suggested corrections
- Downloadable report
- Severity filtering

Priority 3 – Optional:
- Before/after comparison
- Visual highlighting of affected elements
- Historical scans
- Multi-page scanning
- Advanced analytics

Do not sacrifice a working scanner to implement optional features.

---

## 7. SECURITY AND RELIABILITY

The backend must:
- Accept only HTTP and HTTPS URLs.
- Reject localhost, loopback, private and internal network destinations.
- Validate DNS-resolved addresses and redirects.
- Prevent the browser from reaching private/internal network resources, including through subresource requests.
- Use reasonable page-loading and scanning timeouts.
- Limit resource usage and concurrent scans.
- Handle browser crashes and network failures.
- Avoid exposing API keys or internal error details.

For a public deployment, URL validation alone is not sufficient. Use network-level isolation or egress restrictions. If these safeguards cannot be implemented during the hackathon, restrict the demonstration to approved public test URLs and avoid exposing an unrestricted public scanning endpoint.

---

## 8. ACCESSIBILITY ACCURACY RULES

- Use actual axe-core results, not invented findings.
- Automated checks cannot detect all accessibility issues.
- Do not claim complete WCAG compliance.
- Do not claim that a score of 100 means a website is fully accessible.
- AI recommendations must be treated as suggestions requiring review.
- Do not invent meaningful alt text without understanding an image's context.
- Clearly identify issues that require manual accessibility testing.

---

## 9. GITHUB COLLABORATION

Use one repository named:

`AccessGuard-AI`

Branches:
- Ujjwal: `backend`
- Shivam: `frontend`
- Krishnanand: `ai-testing`

Each member must:
1. Clone the shared repository.
2. Work on their assigned branch.
3. Commit and push completed changes.
4. Explain any required integration changes.
5. Avoid modifying another member's files without coordination.
6. Test their changes before merging.

The GitHub repository and `PROJECT_SPEC.md` are the shared source of truth.

ChatGPT sessions do not automatically synchronize. Each member must share updated code, errors and approved specification changes with their own ChatGPT.

---

## 10. DEVELOPMENT PRIORITIES FOR 24 HOURS

Phase 1: Environment setup and shared API agreement.

Phase 2: Build frontend, scanner and AI prototype independently.

Phase 3: Integrate the real scanner with the frontend.

Phase 4: Add AI explanations and scoring.

Phase 5: Add reporting, test edge cases and improve presentation.

Phase 6: Prepare and rehearse the live demo.

**Highest priority:** A real, functioning end-to-end scan.

---

## 11. INSTRUCTIONS TO CHATGPT

You are the technical mentor for one member of this team.

Follow these rules:

1. First ask which member you are assisting: Ujjwal, Shivam or Krishnanand.
2. Focus on that member's assigned responsibilities.
3. Follow the fixed project architecture and API contract.
4. Teach beginner-friendly concepts step by step.
5. Explain why each technology and code section is necessary.
6. Give exact file names, terminal commands and code placement.
7. Do not dump the entire project code at once unless explicitly requested.
8. Help debug errors using actual error messages.
9. Do not invent successful test results or claim untested code works.
10. Do not change another member's interface without team approval.
11. Clearly distinguish completed work, mock data and planned functionality.
12. Prioritize integration and working functionality.
13. If something is uncertain, say so and explain how to verify it.
14. Keep the solution realistic for three beginners and a 24-hour hackathon.

**STARTING INSTRUCTION:**

Ask me which team member I am, then help me build my assigned module from the beginning, one working milestone at a time.
