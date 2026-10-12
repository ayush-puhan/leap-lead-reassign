# Leap Lead Reassign — Handover your Leads

A clickable mockup and product spec for the "Handover your Leads" flow inside Leap's counselor CRM — a resigning counselor's leads getting reassigned to a new counselor.

## What's here

- [`index.html`](index.html) — a self-contained, static HTML/JS mockup of the flow (no build step, no dependencies). Open it directly in a browser, or deploy it as a static site.
- [`docs/prd-counselor-handover.md`](docs/prd-counselor-handover.md) — the v0 PRD: screens, forms, message templates, logic and edge cases.
- [`docs/brd-counselor-handover.md`](docs/brd-counselor-handover.md) — the business requirements doc.
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

## Trying the flow

The SM drives this flow; the leaving counselor has no control. Use the yellow "Mockup controls" bar to switch roles and move the clock:

- **Vikram (SM):** Tasks & Performance → Important Business Tasks → **Counsellor Last Working Day** opens a full-page modal. **+ Add last working day**: pick a counselor and a date, see the leads affected (Open App / Post STI / Post Deposit / Drop off), confirm. Fresh lead assignment stops and the TL gets a task. **Reassign leads** opens scheduling; **Cancel** handles retention.
- **Ankit (TL):** the **Schedule Reassigned Leads** tile opens a full-page modal with one accordion per leaver. Click rows, choose a counselor and **Schedule assignment**.
- **Run 8 PM transfer** moves the leads on T-7 (7 days before the last working day). If T-7 has already passed, leads move the moment they are scheduled.
- **Skip ahead 1 day (LGC joins):** new counselors join each student's Leap group chat (Step 0); the LGC message and email go to the student on join.
- **Jump to last working day:** at 8 PM the old counselor and their TL are removed from the Leap group chats.
- **Rahul (new counselor):** reassigned students show in Boost STI / Boost Deposit with a Reassigned badge and Step 0, and **View assigned leads** (top bar) lists every reassigned lead in any stage.
