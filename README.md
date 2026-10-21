# QA Portfolio: Email Login Fallback (Testware #004)

An anonymized showcase of how I planned the tests for a login improvement in a provider mobile app of a health & aesthetics services marketplace. It follows ISO/IEC/IEEE 29119 and ISTQB CTFL 4.0. This cycle covers the **test planning** work products: test plan, schedule, risk register, and entry and exit criteria. No internal company data, names or links are included.

---

## What this is

Providers log in with their phone number and a 6-digit code sent by SMS. Sometimes the SMS never arrives. The improvement adds a fallback on the SMS code screen: a button **"Didn't get the code? Login with email instead"** opens a screen where the provider types an email address and taps **Get Started**; the code is then sent to the email registered on the account.

It is a small feature with a large blast radius: it is part of **authentication**. A mistake here either locks providers out exactly when they need help, or sends a login code to the wrong person.

The scope was kept strictly to the described improvement. All company, product, person and environment names were removed. Any test data mentioned is fictional.

## Rules under test (summary)

| # | Rule |
|---|------|
| 1 | The email option appears **immediately** on the SMS code screen, and only there |
| 2 | The code is sent **only** to the email registered and linked to the account of the phone entered |
| 3 | Any non-matching input gets a **generic** error message |
| 4 | After Get Started, a similar code screen opens **without** the email button; the code has 6 digits |
| 5 | The email code is valid for **10 minutes**; generating a new code **expires the previous one** |
| 6 | **Back** returns to the SMS code screen |
| 7 | There are **no attempt limits** (recorded as a future improvement, not tested) |

The full list (12 confirmed rules and 9 open assumptions) is in [`docs/test-plan.md`](docs/test-plan.md).

## Highlights from the risk analysis

- **R2 (High): the iOS keyboard can break the fallback.** iOS capitalizes the first letter of a text field and the suggestion bar can add a trailing space. If the email comparison is case- or space-sensitive, a provider typing their correct email is rejected, so the fallback fails exactly when it is needed.
- **R1 (High): "the email exists" is not "the email belongs to this account".** The provider types the email instead of the app using the stored one. If the check only confirms that the email exists, a code can be sent to another provider or open the wrong account.
- **R3 (High): delivery.** The whole feature depends on an email arriving well inside the 10-minute window, outside the spam folder, with a working code.
- **R14 (Accepted): no attempt limits.** Unlimited requests and code guesses are a known gap. Instead of testing behavior that does not exist, the plan records it as a change request and makes stakeholder acknowledgement an exit criterion.

## Repository map

| Path | Content |
|------|---------|
| `docs/test-plan.md` | Scope (in and out), objectives, constraints, test basis, confirmed rules, open assumptions, stakeholders, approach, metrics, test data and environment |
| `docs/schedule.md` | 7 steps of the ISO/IEC/IEEE 29119-2 test process over 3 working days |
| `test-design/risk-register.md` | 19 product and project risks scored by probability x impact, with mitigation |
| `docs/entry-exit-criteria.md` | 6 entry criteria and 10 exit criteria |
| `docs/test-planning-workbook.xlsx` | The original planning workbook (4 tabs, with formulas, status lists and explanatory tooltips) |

## Test design techniques planned

- **Equivalence partitioning:** email input classes (registered email of this provider; same email in upper case or with spaces; another provider's email; unregistered valid email; malformed; empty; provider with no email).
- **Boundary value analysis:** code length (5, 6, 7 digits) and the 10-minute expiry (just before and just after).
- **Decision table "Email check":** format valid? x email registered? x linked to this phone's account? > error or email sent.
- **State transition testing:** screens (SMS code > email > back, or > email code > logged in) and code states (SMS code expired after Get Started; older email code expired after a new request; email code expired after 10 minutes).
- **Error guessing and charters:** double tap on Get Started, leaving the app to read the Mail app and coming back, iOS autofill "From Mail".

## Why no automation in this cycle

The target is a **native iOS app**. Playwright drives web pages, not native apps, so this cycle is manual by design. The plan says so explicitly, as a recorded deviation from the usual strategy.

## Skills shown

Security-minded functional testing of an authentication flow, tight scope control, risk-based prioritization, turning a missing requirement into a change request instead of a test, and entry and exit criteria tied to measurable objectives.

## Status

Planning is complete. Execution results are not included because the target application is private.
