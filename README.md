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

Use the yellow "Mockup controls" bar at the top to switch between roles (Counselor/leaver, SM, TL, new counselor) and to fast-forward the clock through the 8 PM transfer job. After the transfer, switch to the new counselor's or TL's view and open the **Tasks & Performance** tab to see the "Connect with Reassigned Leads" task (styled after the counselor CRM's own Tasks & Performance tab, shown here as static mock data).
