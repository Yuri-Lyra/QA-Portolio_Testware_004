# Test Schedule (Testware #004)

Total planned time: 3 working days (2026-10-21 to 2026-10-23).

| Step | Phase (ISO/IEC/IEEE 29119-2) | Activity | Start | End | Working days |
|---|---|---|---|---|---|
| 1 | Test Planning | Draft the Test Plan, Schedule, Risk Register and Entry/Exit Criteria. Send the open assumptions (A1 to A9, A1, A2, A4 and A6 first) to the stakeholders. Record the missing limits as a future improvement. | 2026-10-21 | 2026-10-21 | 1 |
| 2 | Test Analysis | Derive test conditions from the PRD and the confirmed rules. Build the 'Email check' decision table, the state transition model (screens and code states), the email and code partitions and the 10-minute boundaries. Prioritize by risk, coverage and requirement. | 2026-10-21 | 2026-10-21 | 1 |
| 3 | Test Design | Write test cases, charters (Mail app switch, iOS keyboard), coverage items, test data requirements and environment requirements. | 2026-10-21 | 2026-10-22 | 2 |
| 4 | Test Implementation | Build test sets and procedures. Prepare P1, P2 (QA inboxes) and create P3 (no email). Install and record the DEV build. Run the entry-criteria smoke test (SMS screen shows the button; one email code arrives and logs in). | 2026-10-22 | 2026-10-22 | 1 |
| 5 | Test Execution | Run the high priority sets: email check (decision table, upper case and spaces, another provider's email), email delivery and code login, previous codes expired, 10-minute expiry (run the waits in parallel). | 2026-10-22 | 2026-10-22 | 1 |
| 6 | Test Execution (cont.) | Run the medium and low priority sets: generic error message, P3 with no email, button visibility and layout, [Back], double tap on [Get Started], Mail app switch, code input variants, minimal SMS check, charters. Retest fixed defects. | 2026-10-23 | 2026-10-23 | 1 |
| 7 | Test Completion | Consolidate metrics, write the Test Completion Report, lessons learned and improvement actions (including the missing limits). Validate the exit criteria. | 2026-10-23 | 2026-10-23 | 1 |
