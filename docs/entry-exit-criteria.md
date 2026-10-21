# Entry and Exit Criteria (Testware #004)

## Entry criteria: must be met BEFORE test execution starts

| ID | Criterion | Notes |
|---|---|---|
| E1 | Resource availability: QA allocated; Provider App DEV build with the email fallback installed on an iPhone (iOS only); 3 working days confirmed (2026-10-21 to 2026-10-23). | Record the build version for each run. |
| E2 | Access and data: P1 and P2 log in on DEV and their registered emails go to QA inboxes (readable in the Mail app on the test iPhone); P3 with no registered email created; a phone that receives SMS on DEV. | See 'Test data requirements' in the Test Plan. |
| E3 | Email sending enabled in the test environment and checked: one email with a code received in a QA inbox. | Rule 9: the test environment was changed to send emails. |
| E4 | Test material available: PRD reviewed, answers of 2026-10-21 recorded (rules 4 to 12), Figma accessible (if any), and the build with the email fallback deployed to DEV (Jira story or release note). |  |
| E5 | Critical ambiguities answered: generic message text and cases (A1), upper case and spaces (A2), [Back] and the SMS code (A4), email code expiry after a new request (A6). | Without these answers, the expected results of the high-priority tests are unknown. |
| E6 | Initial quality level: smoke test passed. The SMS login works; the SMS code screen shows the email button; one email code arrives and logs P1 in. |  |

## Exit criteria: must be met to consider the cycle COMPLETE

| ID | Criterion | Notes |
|---|---|---|
| X1 | 100% of high priority test conditions (risk score 15 or higher: R1, R2, R3) executed. |  |
| X2 | All rules of the 'Email check' decision table validated on iOS, including P3 (no email), and the same generic message confirmed for every invalid case. |  |
| X3 | Code state transitions validated: SMS code expired after [Get Started]; older email code expired after a new request; email code accepted before and rejected after 10 minutes. |  |
| X4 | Screens validated: button shown at once only on the SMS code screen and absent from the email code screen; [Back] returns to the SMS code screen; login with the email code opens the right account. |  |
| X5 | Zero open defects of Critical or High severity related to the email check, code delivery, code expiry or login completion. |  |
| X6 | Defect density within the level agreed with the stakeholders. |  |
| X7 | All planned tests executed, deferred with a recorded reason, or blocked with a documented cause. |  |
| X8 | Minimal SMS login check executed with no new failures. |  |
| X9 | Missing attempt limits, lockout and rate limit recorded as a future improvement (change request) and acknowledged by the stakeholders. |  |
| X10 | Test Completion Report written and reviewed by the stakeholders (Engineering, Backend and Frontend leads). |  |

Entry criteria follow CTFL 4.0 section 5.1.3: resources available, test material available, and an initial quality level (smoke test). Exit criteria combine measures of thoroughness (coverage, open defects, defect density) and completion criteria (planned tests executed, report reviewed).
