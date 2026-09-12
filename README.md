# SOC engagement portfolio: Cloudora

## What this engagement was

Simulated SOC trainee queue for Cloudora. Two shifts so far: Shift 1 covered CLD-0201 to CLD-0204, Shift 2 covered CLD-0205 to CLD-0208. Took each alert from first look to a closed verdict, same as a real queue.

## How I worked

Same structure every ticket: alert, hypotheses, evidence, reasoning, verdict, key takeaways, evidence used. Always wrote a malicious hypothesis and a benign hypothesis before looking at evidence, so I wasn't just confirming a first guess. Re-scored severity at the end using impact x confidence instead of trusting the auto-rating; that's why CLD-0203 went from Low to S3, and why CLD-0205 went from Medium to S3.

## What I found — Shift 1

**CLD-0201** — Failed sign-ins on a new privileged account. Looked like brute force. Turned out to be a new hire stuck on Caps Lock with a temp password, confirmed by a helpdesk ticket. False positive. [Full writeup](SOC_Shift_1_Ticket_Investigation.md#cld-0201-investigation)

**CLD-0202** — PUA alert from a bundled installer that came with a free PDF converter. Alert only fired because it's a remediation rule, not a detection rule — AV had already quarantined it and the rescan came back clean. False positive. [Full writeup](SOC_Shift_1_Ticket_Investigation.md#cld-0202-investigation)

**CLD-0203** — Guest account's first after-hours sign-in tripped a baseline rule. 3 of 4 attributes matched normal behavior, and the "unusual" hour fit a pattern of logins getting later each time. Event itself looked benign, but the account has no MFA and touches live payroll data during an active payroll phishing push. Escalated on the exposure, not the event. [Full writeup](SOC_Shift_1_Ticket_Investigation.md#cld-0203-investigation)

**CLD-0204** — Port sweep outside the normal scan window. Matched an approved change (CHG-2101) that moved the weekly scanner earlier for the week. Alert rule just hadn't caught up to the new schedule. False positive. [Full writeup](SOC_Shift_1_Ticket_Investigation.md#cld-0204-investigation)

## What I found — Shift 2

**CLD-0205** — Reported phishing email spoofing DocuSign, timed to a live Bexley integration project. SPF, DKIM, and DMARC all failed, and the link pointed to a bare IP credential page. True positive, rerated Medium to High (S3). [Full writeup](SOC_Shift_2_Ticket_Investigation.md#cld-0205-investigation)

**CLD-0206** — Impossible-travel alert for a sign-in from Lisbon. Matched a pre-approved annual leave ticket and used a registered device with MFA satisfied. False positive. [Full writeup](SOC_Shift_2_Ticket_Investigation.md#cld-0206-investigation)

**CLD-0207** — Outbound mail surge from the marketing account. Matched a scheduled newsletter send logged in the helpdesk, same recipient count and connector. False positive. [Full writeup](SOC_Shift_2_Ticket_Investigation.md#cld-0207-investigation)

**CLD-0208** — Automated sandbox re-scored the same phishing URL from CLD-0205 as malicious post-delivery. Same sender, recipient, and timestamp — closed as a duplicate, no new action needed. [Full writeup](SOC_Shift_2_Ticket_Investigation.md#cld-0208-investigation)

## What I would do differently

Should have flagged the no-MFA gap on guest accounts as its own issue instead of burying it in CLD-0203's closing notes. Also want to check handover notes first, before I start forming a theory; a couple times I checked evidence in the wrong order and had to backtrack.

## Contents

- `SOC_Shift_1_Ticket_Walkthrough.md`: full writeups for CLD-0201 to CLD-0204, screenshots embedded
- `SOC_Shift_2_Ticket_Walkthrough.md`: full writeups for CLD-0205 to CLD-0208, screenshots embedded
- `images/`: screenshots referenced in both reports

All data in this engagement is synthetic (reserved IP ranges, reserved domains); the verdicts and writing are mine.
