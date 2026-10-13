# Leap Lead Reassign — Handover your Leads

A clickable mockup and product spec for the "Handover your Leads" flow inside Leap's counselor CRM — a resigning counselor's leads getting reassigned to new counsellors.

## What's here

- [`index.html`](index.html) — a self-contained, static HTML/JS mockup of the flow (no build step, no dependencies). Open it directly in a browser, or deploy it as a static site.
- [`docs/prd-counselor-handover.md`](docs/prd-counselor-handover.md) — the PRD: the revised flow, handover tasks, calling rules, edge cases and open questions.
- [`docs/brd-counselor-handover.md`](docs/brd-counselor-handover.md) — the original business requirements doc (written before the revised flow).
- [`docs/lead-stage-values.csv`](docs/lead-stage-values.csv) — the CRM's "Bofu Status" export used to define the lead-stage enum referenced in the PRD.

## Running the mockup locally

It's a single static HTML file — no install needed:

```bash
open index.html
```

or serve it with any static file server, e.g.:

```bash
npx serve .
```

## The flow in one paragraph

The counsellor has no part in this: the **SM raises a request** for them with their last working day (LWD) and **approves it**; only then does the lead list open. The **TL (or POD leader if the counsellor has no TL, or the SM)** picks leads, picks a new counsellor and clicks **Approve Assignment**. After an "Are you sure? This action is irreversible" confirm the leads move **immediately**. There is no schedule. Each reassigned lead becomes one **handover task** for the new counsellor: join the student's WhatsApp group (every lead) and call the student (post-STI leads excluding visa approved, and every locked-in student). The old counsellor leaves the group when the new counsellor joins. At 8 PM on the LWD the old counsellor's login is deactivated, but only if every lead has been reassigned.

## Every view, by role

Sample counsellors are seeded in each state, so every view exists on load.

| State | Seeded as | SM (Counsellor Last Working Day) | TL / POD (Counsellor Last Working Day) | New counsellor (Rahul) |
|---|---|---|---|---|
| No request yet | Priya (live walk-through), Kabir | **+ Raise request** | — | — |
| Request raised, waiting for approval | Mohit | **Approve**, calendar, delete | — | — |
| Assigning leads, nothing moved | Vivek (640 leads, lazy load), Neha | Calendar, delete | To reassign | — |
| Assigning leads, partly moved | Karan (30 / 92), Zara (12 / 42, no TL so POD Rohit) | Calendar only (delete hidden once leads move) | To reassign + Track block | Tasks for the moved leads |
| All leads reassigned, some still to join | Rhea | Track handover: warning + Remind all | Tracking handover | Tasks, "Join WA Group" |
| All leads reassigned, everyone joined | Tanvi | Track handover: all joined | Tracking handover | Joined |
| Completed (left), 2 never joined | Imran | Past tab | Completed | — |
| Deleted | Dev (sample row) | Past tab | — | — |

## Trying the flow

Use the yellow "Mockup controls" bar to switch roles and move the clock.

1. **Vikram (SM):** Tasks & Performance → Important Business Tasks → the **Counsellor Last Working Day** bar under the 8 tiles (one bar for requests and lead assignment). **+ Raise request**: pick Priya and her LWD, see the leads affected (counts only; the list stays hidden). Her row becomes *Pending approval*. **Approve**: a red confirm lists what happens next. The calendar icon changes the LWD until reassignment is complete; delete is only offered while no lead has moved.
2. **Ankit (TL):** the **Counsellor Last Working Day** bar under the Important Business Tasks tiles → open Priya → click rows (or the header checkbox) → choose a counsellor → **Approve Assignment**. The "irreversible" confirm shows the leads, their pre-STI / post-STI / visa split. On confirm the leads move at once and the rows lock. Columns: student (name with user ID below), stage, servicing type, lock-in details, intake, country, UG / PG. Filters: group stage, servicing type, lock in, lead score, intake and country (multi-select), UG / PG (single). Leads load 500 at a time; Group stage narrows the whole book, the other filters apply to the loaded leads only.
3. **Rahul (new counsellor):** Tasks & Performance → **Connect with Reassigned Leads**. Each lead is one task with two sub-tasks: **① Join WA Group** and **② Call Student** (post-STI leads excluding visa approved, and every locked-in student). Tasks are ordered deposit done, premium / lock-in, STI done, STI not done, visa in process. Joining removes the old counsellor and sends the LGC message and email. **View assigned leads** lists reassignments counsellor-wise (old counsellor, old TL, number of leads). Three unanswered calls close the task. Students with no WhatsApp group show "introduce yourself directly".
4. **Sameer (RM):** Tasks & Performance, laid out like the production RM page (Boost STI, Boost Revenue, Boost Loan, Potential Escalation, Quality Score, Own Tasks) with the **Connect with Reassigned Leads** bar on top. Students reassigned to counsellors mapped to this RM show with **Join group** and **View student**; the task closes when the RM joins the group. The call stays with the new counsellor.
5. **Demo bar:**
   - **New counsellors join groups** — everyone joins; the old counsellor is removed.
   - **Jump to last working day − 1, 11 PM** — fires the alert email to Debasish Sahoo, the SM and the reporting manager if leads are still unassigned or a new counsellor has joined none of their groups.
   - **Jump to last working day, 8 PM** — deactivates Priya if every lead is reassigned; if not, deactivation is **blocked** and the SM and TL are alerted. Reassign the rest and she deactivates automatically.
