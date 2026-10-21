# Test Plan (Testware #004)

Feature: Provider App login - login code via email as a fallback when the SMS code does not arrive.  
Improvement of the existing login (phone number + 6-digit code by SMS): on the SMS code screen, the provider can ask for the code by email.  
Sources: PRD 'Allow providers to receive login code via email as a fallback when SMS doesn't work' and answers from the clarification session (2026-10-21).

Status legend: **Confirmed** = agreed; **To confirm** = waiting for an answer from the team; **Pendent** = not reviewed yet.

## 1. CONTEXT OF TESTING

### Scope

> WHAT is tested and what is NOT. Writing the 'out of scope' list avoids discussions later about tests nobody planned (CTFL 5.1.1 / ISO 29119-3 test plan: context of testing).

Only the email fallback added to the Provider App login, on iOS (Provider App DEV build):  
(a) the button [Didn't get the code? Login with email instead] on the SMS code screen;  
(b) the email screen: input with placeholder 'Email Address', [Get Started] and [Back];  
(c) the check that the typed email is the one registered and linked to the account of the phone number entered at login, and the generic error message;  
(d) delivery of the 6-digit code to that email inbox;  
(e) the email code screen (similar to the SMS screen, without the email button): 6-digit code, 10-minute validity, previous codes expired;  
(f) login completed with the email code;  
(g) minimal check of the SMS login, only because it is the mandatory starting point of the improvement.  
OUT OF SCOPE: attempt limits, lockout and rate limit (they do not exist: recorded as a future improvement); full regression of the SMS login; Android; Customer App and website; creating or changing a provider's email; email template design beyond the code; any other login or account feature.

**Status:** Confirmed · **Notes:** Platform confirmed: iOS only. Limits recorded as future improvement (R14).

### Test objectives

> WHY we test. Each objective must be checkable at the end of the cycle; the exit criteria are built from them.

Ensure that:  
(1) the email option is shown at once on the SMS code screen, and only there (not before the SMS request and not on the email code screen);  
(2) a code is sent only when the typed email is the one registered and linked to the account of the phone entered; any other input (another provider's email, an unregistered email, a malformed or empty value, a provider with no email) gets the generic error message and no email is sent;  
(3) the email arrives on time with a 6-digit code, and that code logs the provider in;  
(4) only the newest code works: the SMS code (and any older email code) expires when a new code is generated, and the email code expires after 10 minutes;  
(5) [Back] returns to the SMS code screen with no side effects;  
(6) the SMS login still works (minimal check).

**Status:** Confirmed

### Constraints

> Limits that shape HOW we can test (time, tools, data, environment).

- 3 working days: 2026-10-21 to 2026-10-23  
- Manual execution only: the Provider App is a native iOS app, and Playwright tests web pages, not native apps, so no automation in this cycle  
- Only fictional test providers whose registered emails go to inboxes controlled by QA (no real person may receive a code)  
- Expiry tests need real waiting time (10 minutes): run them in parallel with other tests  
- No attempt limits exist, so repeated requests during testing are possible but must be kept reasonable

**Status:** Confirmed · **Notes:** Deadline confirmed (2026-10-21).

### Test basis

> The documents the tests are derived from. If a rule is not in the test basis, it becomes an assumption to confirm.

- PRD 'Allow providers to receive login code via email as a fallback when SMS doesn't work' (English summary + Portuguese description of the screens and buttons)  
- Answers from the clarification session (2026-10-21): rules 4 to 10 below  
- Figma design: to be added  
- Jira story: to be added

**Status:** Confirmed · **Notes:** The PRD is short: the confirmed rules and open assumptions below define the expected results.

## 2. ASSUMPTIONS AND PROJECT CONSTRAINTS

### Confirmed rules

> Rules already stated by the team. Expected results of the test cases come from here.

(1) The email option is offered only after the code was requested by SMS, on the code input screen.  
(2) The button [Didn't get the code? Login with email instead] is shown immediately when the SMS code screen opens.  
(3) A tap opens a new screen with one input (placeholder 'Email Address'). [Get Started] confirms the request; [Back] cancels it.  
(4) The code is sent only to the email registered and linked to the provider account.  
(5) If the typed email does not match, the app shows a generic error message.  
(6) After [Get Started], a code screen similar to the SMS one opens, without the email button. The code has 6 digits.  
(7) The email code is valid for 10 minutes. A new code is generated and the previous code expires.  
(8) [Back] returns to the previous screen (SMS code screen).  
(9) On the test environment, the email is really sent (the environment was changed for that).  
(10) There are no attempt limits, lockout or rate limit. This is recorded as a future improvement, not tested.  
(11) A provider with no registered email does not exist yet: QA will create one to test it.  
(12) The SMS login gets a minimal check only.

**Status:** Confirmed · **Notes:** Rules 1 to 3: PRD. Rules 4 to 12: answers from the clarification session (2026-10-21).

### Assumptions to confirm

> Anything we BELIEVE but nobody confirmed. Each one becomes a question to the team; the answer changes the expected result of some test cases.

(A1) The generic error message is the SAME text, in the same place, for every invalid case: another provider's email, unregistered email, malformed email, empty field, provider with no email. Exact text to be provided.  
(A2) The email check ignores upper/lower case and leading/trailing spaces (the iOS keyboard capitalizes the first letter and may add a space after a suggestion).  
(A3) With an empty field, [Get Started] is disabled or shows the generic error; there is a basic format check (an '@' and a domain) in the app.  
(A4) [Back] does not generate a code and does not expire the SMS code: only [Get Started] does.  
(A5) The email code screen has no 'resend' and no 'back to SMS' option (the PRD describes none).  
(A6) Rule 7 also applies email to email: each new [Get Started] expires the previous email code.  
(A7) The 10 minutes count from the moment the code is generated and are checked by the server.  
(A8) Login with the email code opens the same screen and session as the SMS login.  
(A9) The email contains the 6-digit code only (no login link), in English.

**Status:** To confirm · **Notes:** A1, A2, A4 and A6 block the entry criteria (E5): send them first.

## 3. STAKEHOLDERS

### QA Analyst

> Who is involved and why. Knowing who answers each kind of question saves time.

QA Analyst and tester (author of this plan): plans, designs, executes and reports the tests.

**Status:** Confirmed

### Engineering Lead

technical approval; clarifies business rules (open assumptions) and the future improvement (limits).

**Status:** Confirmed · **Notes:** Same stakeholders in every Testware (confirmed 2026-10-21).

### Backend Lead

email-to-account check, code generation and expiry (10 minutes, previous codes expired), email sending in the test environment, logs, the test provider with no registered email.

**Status:** Confirmed

### Frontend Lead

Provider App screens and states (SMS code screen button, email screen, email code screen, [Back], [Get Started]), generic error message text, keyboard behavior.

**Status:** Confirmed

## 4. RISK REGISTER (SUMMARY)

### Reference

> Risk-based testing: the higher the risk score, the earlier and deeper we test that area (CTFL 5.2).

See the 'Risk Register' tab for the full list with probability, impact, score and mitigation.

**Status:** Confirmed

### Highest-severity risks (summary)

R2: a valid registered email is rejected because of upper case or spaces (the iOS keyboard capitalizes the first letter): the fallback fails exactly when the provider needs it.  
R1: the app checks only that the email EXISTS, not that it belongs to the account of the phone entered: the code goes to another provider or logs into the wrong account.  
R3: the email does not arrive, arrives late or goes to spam, or the code in it does not work.  
R14 (accepted): no attempt limits: unlimited requests and code guesses. Recorded as a future improvement.

**Status:** Confirmed

## 5. TEST APPROACH

### Test levels

> Level = how big the piece under test is. Here: the whole system and its integrations, not single functions of the code.

(1) System testing of the login flow in the Provider App (iOS, DEV build);  
(2) System integration testing of the code delivery (Backend > email service > inbox);  
(3) Minimal regression (smoke) of the SMS login.

**Status:** Confirmed

### Test types

> Type = which quality characteristic we check (function, data, time, usability, robustness).

(i) Functional (button, email screen, check of the email, code screen, login);  
(ii) Security-related functional checks (email linked to the right account, previous codes expired, error message that does not reveal which emails exist);  
(iii) Time-based (10-minute validity, email delivery time);  
(iv) UI and usability (button visible on small iPhones with the keyboard open, placeholder, iOS keyboard, Mail app switch and return).

**Status:** Confirmed

### Test techniques

> Techniques = methods to choose test cases with the best coverage using few tests (CTFL chapter 4).

(a) Equivalence partitioning and boundary value analysis: email input (registered email of this provider; same email in upper case or with spaces; another provider's email; unregistered valid email; malformed; empty; provider with no email); code (6 correct digits; 5 and 7 digits; letters; wrong 6 digits); expiry (just before and just after 10 minutes);  
(b) Decision table 'Email check': format valid? x email registered? x linked to the phone's account? > error or email sent;  
(c) State transition: SMS code screen > email screen > (Back > SMS code screen) or (Get Started > email code screen > logged in); code states: SMS code active > expired after Get Started; email code 1 > expired after a new request; email code > expired after 10 minutes;  
(d) Error guessing: double tap on [Get Started], leave the app to read the Mail app and come back, iOS keyboard autocapitalization and autofill 'From Mail'.

**Status:** Confirmed

### Test deliverables

> The work products this cycle will produce (ISO 29119-3).

(1) Test Plan; (2) Risk Register; (3) Entry and exit criteria; (4) Schedule; (5) Test conditions, test cases, charters and coverage items; (6) Test sets, procedures and test data; (7) Execution records; (8) Defect reports; (9) Test completion report.

**Status:** Confirmed · **Notes:** This file covers deliverables 1 to 4.

### Entry and exit criteria

> Entry = what must be true to START executing. Exit = what must be true to say we are DONE.

See the 'Entry-Exit Criteria' tab.

**Status:** Confirmed

### Test independence

> An independent tester finds different defects than the author of the code.

Tests are run by QA, independent from the team that built the feature, which reduces confirmation bias.

**Status:** Confirmed

### Metrics to be collected

> The numbers that show progress and quality during and at the end of the cycle.

(1) Cases executed vs planned; (2) cases passed, failed and blocked; (3) defects by severity; (4) open vs closed defects; (5) coverage of high-priority conditions; (6) 'Email check' decision table rules validated; (7) code state transitions validated; (8) email delivery time (from [Get Started] to the email in the inbox), measured in each run.

**Status:** Confirmed

### Test data requirements

> The data that must exist BEFORE the tests run. Missing data is a common reason for blocked tests.

(a) Fictional test providers on DEV with Provider App login: P1 with a registered email in a QA inbox (main provider); P2 another provider with a different QA inbox (to test 'another provider's email'); P3 with NO registered email (to be created);  
(b) Email variants for P1: exact; first letter upper case; all upper case; leading and trailing spaces; unregistered valid email; malformed (no '@', no domain); empty;  
(c) Code variants: correct, wrong 6 digits, 5 digits, 7 digits, letters, expired SMS code, expired email code;  
(d) A phone number that receives SMS on DEV, for the minimal SMS check.

**Status:** Pendent · **Notes:** P3 must be created ('insert to test'): ask Backend if the account accepts an empty email.

### Test environment requirements

> Tools, devices and environments needed. Record versions: a defect is only useful if it can be reproduced in the same configuration.

(1) Provider App DEV build with the email fallback installed on an iPhone (iOS only; version recorded for each run), and a small-screen iPhone or simulator for the button layout; (2) Mail app (or webmail) on the same iPhone with the QA inboxes, to test the app switch; (3) a stopwatch for the 10-minute tests; (4) Backend logs (code generation, expiry, email sent) when available.

**Status:** Confirmed

### Deviations from policy or strategy

No Playwright automation in this cycle (native iOS app). Otherwise none: Testware #003 is the format baseline.

**Status:** Confirmed

## 6. BUDGET AND SCHEDULE

### Budget

Not applicable (internal effort, no extra tool cost).

**Status:** Confirmed

### Schedule

3 working days: 2026-10-21 to 2026-10-23. See the 'Schedule' tab.

**Status:** Confirmed
