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

The counselor enters their date on the **View Profile** page (avatar menu, top right). The SM and TL act on it from **Tasks & Performance → Important Business Tasks**; the Learning & Development tab is unchanged. Use the yellow "Mockup controls" bar to switch roles and move the clock:

- **Priya (counselor):** enter a last working day (LWD). A red confirm shows who will transfer the leads and when. Then an order-style tracker shows every step up to the transfer date.
- **Vikram (SM):** on **Tasks & Performance**, the **Last Working Day Approval** tile opens a full-page modal with the requests (pending-name pills + table). **Review** opens the modal with Approve / Change last working day / Reject; approval has a confirm step. Deactivated records drop off.
- **Ankit (TL):** the **Schedule Reassigned Leads** tile opens a full-page modal with one accordion per leaver. Open one, click rows, choose a counselor and **Schedule assignment**.
- **Run 8 PM transfer** moves Priya's leads 7 days before her LWD (or the day after approval on short notice). She stays in the Leap group chats until **Jump to last working day**, when her login is deactivated.
- **Rahul (new counselor):** reassigned students show inside the Boost STI / Boost Deposit pipelines on **Tasks & Performance**, with a "Reassigned" badge and filter.
