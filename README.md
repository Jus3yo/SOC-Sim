# SOC engagement portfolio: Cloudora

## What this engagement was

Simulated SOC trainee queue for Cloudora. Four tickets, CLD-0201 to CLD-0204, one shift. Took each alert from first look to a closed verdict, same as a real queue.

## How I worked

Same structure every ticket: alert, hypotheses, evidence, reasoning, verdict, key takeaways, evidence used. Always wrote a malicious hypothesis and a benign hypothesis before looking at evidence, so I wasn't just confirming a first guess. Re-scored severity at the end using impact x confidence instead of trusting the auto-rating — that's why CLD-0203 went from Low to S3.

## What I found

**CLD-0201** — Failed sign-ins on a new privileged account. Looked like brute force. Turned out to be a new hire stuck on Caps Lock with a temp password, confirmed by a helpdesk ticket. False positive. [Full writeup](SOC_Shift_1_Ticket_Investigations.md#cld-0201-investigation)

**CLD-0202** — PUA alert from a bundled installer that came with a free PDF converter. Alert only fired because it's a remediation rule, not a detection rule — AV had already quarantined it and the rescan came back clean. False positive. [Full writeup](SOC_Shift_1_Ticket_Investigations.md#cld-0202-investigation)

**CLD-0203** — Guest account's first after-hours sign-in tripped a baseline rule. 3 of 4 attributes matched normal behavior, and the "unusual" hour fit a pattern of logins getting later each time. Event itself looked benign, but the account has no MFA and touches live payroll data during an active payroll phishing push. Escalated on the exposure, not the event. [Full writeup](SOC_Shift_1_Ticket_Investigations.md#cld-0203-investigation)

**CLD-0204** — Port sweep outside the normal scan window. Matched an approved change (CHG-2101) that moved the weekly scanner earlier for the week. Alert rule just hadn't caught up to the new schedule. False positive. [Full writeup](SOC_Shift_1_Ticket_Investigations.md#cld-0204-investigation)

## What I would do differently

Should have flagged the no-MFA gap on guest accounts as its own issue instead of burying it in CLD-0203's closing notes. Also want to check handover notes first, before I start forming a theory — a couple times I checked evidence in the wrong order and had to backtrack.

## Contents

- `SOC_Shift_1_Ticket_Investigations.md`: full writeups for all four tickets, screenshots embedded
- `images/`: screenshots referenced in the report

All data in this engagement is synthetic (reserved IP ranges, reserved domains); the verdicts and writing are mine.
