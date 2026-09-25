# Counselor Handover — v0 Plan

## Objective
A self-serve resignation and lead handover flow built inside the existing counselor CRM. A resigning counselor submits their last working day, the SM approves it, the TL assigns the leads, and on the last working day the CRM moves every lead, messages every student, and deactivates the leaver's login on its own. It replaces today's process of exporting leads, sending them to the SM pod for reassignment, and emailing students by hand.

## The Problem
Today when a counselor resigns:
- The counselor is asked to export all their leads and hand the file over.
- The SM pod reassigns the leads by hand.
- The pod or the SM emails students one by one to tell them who their new counselor is.

This depends on the pod and the SM being available and having access. With 3 to 5 resignations a month and 200 to 500 leads per leaver, that is roughly 600 to 2,500 leads a month moved by hand. Leads get missed, follow-ups stay with someone who has left, students are not told who to contact, and there is no single record of who got what.

## Who Uses This
All users log in to the existing CRM with their own account. No new login is needed. What each person sees depends on their existing CRM role.

- **Counselor (leaver).** Submits their resignation with a last working day and tracks progress. Uses it once.
- **TL (Team Lead) of the leaver.** Assigns the leaver's leads to counselors. Uses it 3 to 5 times a month across the org, less for any single TL.
- **SM (Senior Manager).** Approves, rejects, changes the date, or cancels requests. Watches message delivery. Can also assign leads (when the TL is away, or when a TL resigns).
- **New counselor (receiver).** Does not use the flow. Gets a bell alert when leads arrive and a CRM task to post in the student's Leap group chat.
- **Students.** Not CRM users. Receive an email and a WhatsApp message.

## High Level Flow
1. Counselor opens Learning & Development → sees a **Resignation** card below IMP Sheet and Training Modules.
2. Counselor picks their last working day → sees their lead counts by stage → clicks **Submit** → sees a confirm screen → clicks **Confirm**.
3. Request status becomes **Pending SM approval**. The SM gets a bell alert.
4. SM opens the request from the bell or the **Resignations** card → approves, rejects with a reason, or changes the last working day and approves.
5. On approval: status becomes **Approved, TL assigning**. New leads stop being allocated to the leaver. The leaver's TL gets a bell alert.
6. TL opens the assignment screen → filters by stage, servicing type and program → selects leads in bulk → assigns them to one counselor at a time → can change any assignment until the transfer runs.
7. At 8 PM on the last working day the transfer runs:
   - Any lead still unassigned is split evenly across the leaver's TL team.
   - Every lead moves to its new counselor, with open tasks, follow-ups, and past calls and notes.
   - Every student gets an email (sent from the leaver's account) and a WhatsApp message (from the company WhatsApp Business number).
   - Each new counselor gets one bell alert and one CRM task per student to post an intro in the student's Leap group chat.
   - The leaver's CRM login is deactivated.
8. Status becomes **Transferred**. The SM's card shows sent, failed and pending message counts. Failed messages are retried 3 times over one hour, then marked **Failed**.

---

## Features

### 1. Resignation submission (counselor)
- Shown as a card on the Learning & Development page, below IMP Sheet and Training Modules, with the same card style (icon, title, subtitle, expand arrow).
- Card title: "Resignation". Subtitle: "Submit your last working day and hand over your leads".
- Visible to every user with the Counselor or TL role. Not shown to SMs.
- One field only: Last working day.
- Shows a read-only summary of lead counts before submitting: one small box per BOFU stage group (**Pre STI / Open App**, **STI**, **Deposit Done**, **Visa**, **Pre ISL Drop Off**, **Post ISL Drop Off** — see [Reference: Field Values](#reference-field-values) and [Reference: Stage Groups](#reference-stage-groups)) with its total, laid out in a compact row that wraps rather than one long list — this view is read-only for the counselor, so there's nothing to click into. A group with 0 leads for this leaver is left out entirely. The per-stage breakdown behind each group is only shown (and only actionable) on Screen 6.
- Confirm screen before the request is created.
- Once submitted, the counselor cannot withdraw. Only the SM can cancel.
- A user can have only one request that is Pending SM approval or Approved, TL assigning at a time.

### 2. SM approval
- SM gets a bell alert for each new request.
- SM sees all requests from counselors and TLs under them on a **Resignations** card.
- SM actions on a Pending request: **Approve**, **Change date and approve**, **Reject** (reason required).
- SM action on an Approved request: **Cancel** (any time before the 8 PM transfer). Cancelling drops all TL assignments.
- SM action on a Rejected request: **Reopen for counselor**. This lets the counselor submit a new request.

### 3. Lead assignment (TL, or SM)
- Opens as soon as the SM approves.
- Done by the leaver's current TL. The SM can also open the same screen for any request under them.
- If the leaver is a TL, only the SM assigns.
- Every lead the leaver has ever owned is listed, in whatever stage it is currently in — including stages that mean the lead is no longer active, such as Dead Lead or Lead Drop Off.
- Every lead is always listed in one flat, paginated table — there's no separate grouped/collapsed layout to switch into.
- Bulk select with four independent filters — **Stage**, **Group**, **Servicing type**, and **Program** — then assign the selected leads to one counselor.
- **Stage** filter: a multi-select checklist of every individual value from the CRM's Bofu Status field (see [Reference: Field Values](#reference-field-values)); any number can be checked, or none for "All stages".
- **Group** filter: a multi-select checklist of the 6 BOFU stage groups (see [Reference: Stage Groups](#reference-stage-groups)) — **Pre STI / Open App**, **STI**, **Deposit Done**, **Visa**, **Pre ISL Drop Off**, **Post ISL Drop Off**, plus **Other** for anything outside those 6. Checking a group shows every lead whose stage falls in it, without needing to know or pick the individual stage codes — this is the fast way to work a whole funnel stage (e.g. "just Visa") at once. Stage and Group both apply at the same time (AND) when both have selections.
- **Servicing type** filter: `FREE_SERVICE`, `PAID_SERVICE`, or "All".
- **Program** filter: `MASTERS`, `UNDER_GRADUATION`, or "All".
- The counselor dropdown lists every active counselor under the same SM, across all TLs, excluding the leaver.
- Assignments can be changed until the transfer runs.

### 4. Stop new leads to the leaver
- Once the SM approves, the CRM's lead allocation skips the leaver.
- If the SM cancels, the leaver is added back to allocation.

### 5. Automatic transfer at 8 PM on the last working day
- Auto split for unassigned leads, then ownership change, then messages, then tasks and alerts, then login deactivation. Order is set out under Logic and Rules.
- Moves with each lead: owner, all open tasks and follow-ups, and access to past calls and notes.

### 6. Student messages
- **Email**
  - From: the leaver's email account, sent through the CRM's existing email connection for that counselor.
  - To: the student's email on the lead.
  - Reply-To: the new counselor's email.
  - CC: new counselor, TL/POD, SM, debasish.sahoo@leapfinance.com.
  - Body: one fixed template (below) with names and contact details filled in.
- **WhatsApp DM**
  - From: the company WhatsApp Business number.
  - To: the student's phone on the lead.
  - Uses one Meta-approved template (below) with new counselor, TL and SM names and phone numbers.
- **Leap group chat (LGC)**
  - v0 does not post automatically.
  - Each new counselor gets one CRM task per student: "Join [Student name]'s Leap group chat and post your intro". The task holds a ready message to copy.
- All students get messages, including students whose lead is in a closed-out stage (Dead Lead, Lead Drop Off, Admit Declined, and so on).
- Changing the wording needs a developer. There is no template editor in v0.

### 7. Delivery tracking and retries
- Each student has a delivery status per channel: Pending, Sent, Failed. LGC tracks the task instead: Open or Done.
- A failed email or WhatsApp is retried 3 times: 20, 40 and 60 minutes after the first attempt.
- After the 3rd failed retry it is marked Failed and shown on the SM's card.

### 8. Action history
- Every action on a request is recorded with time, the person who did it, and details.
- Visible to the SM and the leaver's TL on the request detail screen.

### 9. Notifications (CRM bell only)
- SM: new request submitted.
- TL: request approved, assign leads now.
- TL: a counselor you assigned leads to is no longer active, and those leads are unassigned again.
- Counselor (leaver): request approved (with final last working day), request rejected (with reason), request cancelled.
- New counselor: "You received [N] leads from [Leaver name]" at transfer.

---

## Screens

### Screen 1: Resignation card — Not submitted (counselor or TL)
**Purpose:** Let the user submit their resignation.

**What the user sees:**
- A card below Training Modules on Learning & Development. Title "Resignation", subtitle "Submit your last working day and hand over your leads", and an expand arrow.
- When expanded:
  - Text: "Your leads will move to new counselors automatically at 8 PM on your last working day. Students will be informed by email and WhatsApp."
  - A **Your leads** summary: one small box per stage group that has at least one lead, plus a Total box, wrapping onto as many lines as needed rather than stacking one group per row. For example, in a single compact row: "Pre STI / Open App 142 · STI 135 · Visa 8 · Pre ISL Drop Off 25 · Post ISL Drop Off 15 · Total 325" (Deposit Done is left out here because this leaver has 0 leads in it). This summary is not clickable — it's read-only information for the counselor. The per-individual-stage breakdown behind each group (see [Reference: Stage Groups](#reference-stage-groups)) is only shown, and only actionable, on Screen 6, where the TL/SM is the one deciding what to do with those leads.
  - A **Last working day** date picker.
  - A **Submit** button, disabled until a valid date is picked.

**What the user can do:**
- Pick a date.
- Click **Submit** to open the confirm screen (Screen 2).

**Empty state:** If the user owns 0 leads, the summary shows "You have no leads. Nothing will be transferred." Submit still works.
**Error state:** If lead counts fail to load, the summary shows "Couldn't load your lead counts. Refresh the page." Submit stays disabled.

### Screen 2: Confirm resignation (modal)
**Purpose:** Stop accidental submissions.

**What the user sees:**
- Title: "Confirm your resignation".
- Text: "Last working day: [DD MMM YYYY]. [Total] leads will be handed over ([group breakdown, e.g. 142 Pre STI / Open App, 135 STI, 8 Visa, 25 Pre ISL Drop Off, 15 Post ISL Drop Off]). You can't withdraw this after submitting. Only your SM can cancel it."
- Buttons: **Go back** and **Confirm**.

**What the user can do:**
- Click **Go back** to close the modal and return to Screen 1 with the date kept.
- Click **Confirm** to create the request. The card changes to Screen 3 and the SM gets a bell alert.

**Empty state:** Not applicable.
**Error state:** If saving fails, show red text in the modal: "Couldn't submit. Check your connection and try again." The modal stays open.

### Screen 3: Resignation card — Status tracker (counselor or TL)
**Purpose:** Show the leaver where their request is without asking anyone.

**What the user sees:**
- Last working day (the SM-changed date if changed, with the note "Changed by [SM name]").
- A 4-step tracker: **Submitted** → **SM approved** → **TL assigning** → **Leads transferred**. Done steps are ticked, the current step is highlighted, and each done step shows its date and time.
- After approval: "New leads are no longer assigned to you."
- If rejected: a red badge "Rejected", the SM's reason, and the text "Please speak to your SM." No buttons.
- If cancelled: a grey badge "Cancelled by [SM name]" with the date and time. The card goes back to Screen 1 so a new request can be submitted.
- After transfer: "All [N] leads were transferred on [date]." The user's login is deactivated shortly after, so they will rarely see this.

**What the user can do:** Nothing. The card is read only.

**Empty state:** Not applicable.
**Error state:** "Couldn't load your request. Refresh the page."

### Screen 4: Resignations card (SM)
**Purpose:** One place for the SM to see and act on every request under them.

**What the user sees:**
- A card on the SM's Learning & Development page titled "Resignations", with the count of pending requests as a badge.
- A table, newest first. Columns: Name, Role (Counselor / TL), TL, Last working day, Status, Leads (total), Assigned (for example "180 / 325"), Email (Sent / Failed / Pending), WhatsApp (Sent / Failed / Pending), LGC tasks (Done / Open).
- Status values shown as coloured badges: Pending SM approval (amber), Approved, TL assigning (blue), Transferred (green), Rejected (red), Cancelled (grey).
- A status filter dropdown: All, Pending SM approval, Approved TL assigning, Transferred, Rejected, Cancelled. Default: All.

**What the user can do:**
- Click a row to open Screen 5.
- Change the status filter.

**Empty state:** "No resignations yet."
**Filter returns nothing:** "No requests with this status."
**Error state:** "Couldn't load resignations. Refresh the page."

### Screen 5: Request detail (SM, and TL read-only for approval actions)
**Purpose:** Approve, reject, change the date, cancel, reach the assignment screen, see failed messages and history.

**What the user sees:**
- Header: leaver name, role, TL, SM, submitted date, last working day, status badge.
- Lead counts by stage group, the same compact read-only boxes as Screen 1.
- Action buttons depend on status (SM only):
  - Pending SM approval: **Approve**, **Change date & approve**, **Reject**.
  - Approved, TL assigning: an assignment button (opens Screen 6) plus **Cancel resignation**. The button's label and an accompanying note change with progress, so the SM never sees a plain "Assign leads" button when work has already been done (see below).
  - Rejected: **Reopen for counselor**.
  - Transferred, Cancelled: no buttons.
- The TL sees only the assignment button (when Approved) and no approval buttons.
- **Assignment progress note and button**, shown whenever the status is Approved, TL assigning (to both the SM here and the TL on their own Learning & Development card):
  - 0 leads assigned yet: no note, button reads **Assign leads** (primary/blue).
  - Some but not all assigned: a note "[Assigned] of [Total] leads assigned so far.", button reads **Continue assigning** (primary/blue).
  - All leads assigned: a green note "All [Total] leads have already been assigned. Open below to review or change any of them before the 8 PM transfer.", button reads **Review assignments** (secondary/outline, not primary — so it doesn't read as a fresh, un-done task).
  - This is what stops the TL and the SM from duplicating each other's work: whoever looks second sees the true state instead of a button that looks identical whether 0 or 325 leads are done.
- **Failed messages** section, shown after transfer when there are any. A table with Student name, Lead ID, Channel (Email / WhatsApp), Reason (the provider's error text), and Last tried (time).
- **History** section: a timeline of every action, newest first. Examples: "Submitted by Priya R · 12 Oct 10:42", "Last working day changed from 30 Oct to 25 Oct by Ankit S".

**What the user can do:**
- **Approve**: sets the status to Approved, TL assigning, stops new leads, and sends bells to the TL and the leaver.
- **Change date & approve**: opens a date picker (tomorrow or later). **Save & approve** does the same as Approve with the new date.
- **Reject**: opens a textbox "Reason" (required, max 500 characters). **Reject** sets the status to Rejected and sends a bell to the leaver.
- **Cancel resignation**: opens a confirm modal: "Cancel [Name]'s resignation? All lead assignments will be cleared and they will start getting new leads again." Buttons **Keep it** and **Cancel resignation**.
- **Reopen for counselor**: the counselor's card returns to Screen 1. The rejected request stays in the list.
- **Assign leads / Continue assigning / Review assignments**: opens Screen 6, whichever label is currently showing.

**Empty state:** History always has at least the "Submitted" entry.
**Error state:** If an action fails: red text under the buttons, "Couldn't save. Try again." Status does not change.

### Screen 6: Assign leads (TL, or SM)
**Purpose:** Choose the new counselor for each of the leaver's leads.

**What the user sees:**
- Header: "Assign [Leaver name]'s leads · Transfer at 8 PM on [date]". Progress: "[Assigned] of [Total] assigned".
- One flat, paginated table (50 rows per page) — every lead the leaver owns, always shown; there's no separate grouped/collapsed layout to switch into.
- Four filters, all applying together (AND):
  - **Stage** — a multi-select dropdown: a button (labelled "All stages", the one selected stage's name, or "[N] stages selected") opens a checklist panel listing every individual value in the CRM's Bofu Status field (see [Reference: Field Values](#reference-field-values)), each with its own checkbox, plus a **Clear** action that resets to "All stages". Any number of stages can be checked at once (OR within this filter — a lead matching any checked stage is shown).
  - **Group** — the same kind of multi-select checklist, sitting right next to Stage, but listing the 6 BOFU stage groups (see [Reference: Stage Groups](#reference-stage-groups)) instead of individual stages: **Pre STI / Open App**, **STI**, **Deposit Done**, **Visa**, **Pre ISL Drop Off**, **Post ISL Drop Off**, plus **Other**. Checking "Visa", for example, shows every lead in any Visa-related stage without having to know or check each individual Visa stage code one by one.
  - **Servicing type** — single-select: `Free Service` (`FREE_SERVICE`), `Paid Service` (`PAID_SERVICE`), or All.
  - **Program** — single-select: `Masters` (`MASTERS`), `Under Graduation` (`UNDER_GRADUATION`), or All.
- A **Show** toggle: All / Unassigned / Assigned.
- Table columns: checkbox, Student name, Lead ID, Stage, Servicing type, Program, Last contacted (date), Next follow-up (date), Assigned to (counselor name or "Unassigned").
- Default sort: Next follow-up, soonest first, with empty dates last. Columns can be sorted by clicking the header.
- A **Select all [N] shown** checkbox in the table's header, and a **Select all [N] shown** button above the table — both select every row matching the current filters, across all pages. This is the way to "select all leads of a stage/group at once": check the stage(s) or group(s) wanted in the Stage/Group filters, then Select all shown.
- A bottom bar that appears when rows are selected: "[N] selected", an **Assign to** counselor dropdown, and an **Assign** button.
- Text under the header changes with progress: "Anything left unassigned at 8 PM on [date] will be split evenly across [TL name]'s team." while incomplete, or a green "All [Total] leads are assigned. They'll transfer automatically at 8 PM on [date] — you can still change any assignment until then." once every lead has an assignee.
- A **Back to Learning & Development** button below the table, always present (not just the small breadcrumb-style back link above the header) — so there is always an obvious, reachable way to leave this screen after selecting or assigning. Once every lead is assigned, it reads **Done — back to Learning & Development** instead, as a clear closing confirmation.

**What the user can do:**
- Filter by any combination of Stage (any number checked), Group (any number checked), Servicing type and Program, and switch Show.
- Sort and page.
- Tick rows, or Select all shown (spans every page, not just the one on screen).
- Pick a counselor and click **Assign**. The selected rows update to that counselor, the progress count updates, and the selection clears. A confirming toast names the counselor and count ("[N] leads assigned to [Name]"). Assigning rows that already have a counselor replaces the old choice.
- Search the counselor dropdown by name. It lists active counselors under the same SM, excluding the leaver, as "Name · TL name · current open leads".
- Click **Back to Learning & Development** (or **Done — back to Learning & Development** once complete) at any time — nothing is lost, since every assignment saves immediately when made.

**Empty state:** If the leaver has 0 leads: "No leads to assign."
**Filter returns nothing:** "No leads match these filters."
**Error state:** If Assign fails: a red toast, "Couldn't assign. Try again." Rows are unchanged.
**Locked state:** After transfer, or if cancelled: the table is read only with the banner "Transfer complete" or "Resignation cancelled". The bottom bar is hidden. Filters still work for browsing.

---

## Forms

### Resignation form (Screen 1)

| Field | Type | Required | Notes / Validation |
|-------|------|----------|--------------------|
| Last working day | Date picker | Yes | Must be tomorrow or later. Dates before that are greyed out. |

**On submit:** Opens the confirm modal (Screen 2). On **Confirm**: a request is created with status "Pending SM approval", a bell goes to the SM, and the card shows Screen 3.
**Validation errors:** No date → Submit stays disabled. If the date is no longer valid at submit (for example, the page was left open overnight): red text under the field, "Pick a date from tomorrow onwards."

### Change date & approve (Screen 5)

| Field | Type | Required | Notes / Validation |
|-------|------|----------|--------------------|
| Last working day | Date picker | Yes | Tomorrow or later. Pre-filled with the counselor's date. |

**On submit:** Saves the new date, approves the request, and records "Last working day changed from X to Y" in History.
**Validation errors:** Red text "Pick a date from tomorrow onwards."

### Reject (Screen 5)

| Field | Type | Required | Notes / Validation |
|-------|------|----------|--------------------|
| Reason | Textarea | Yes | 1 to 500 characters. |

**On submit:** Status becomes Rejected, a bell goes to the leaver with the reason, and the action is recorded in History.
**Validation errors:** Red text "Add a reason." The form does not submit.

### Assign (Screen 6 bottom bar)

| Field | Type | Required | Notes / Validation |
|-------|------|----------|--------------------|
| Assign to | Searchable dropdown | Yes | Active counselors under the same SM, leaver excluded. |

**On submit:** Saves the chosen counselor on every selected lead's pending assignment. Nothing moves until 8 PM on the last working day.
**Validation errors:** The Assign button stays disabled until a counselor is chosen and at least 1 row is selected.

### Filters (Screen 6, not a submitted form — applied live)

| Field | Type | Required | Notes / Validation |
|-------|------|----------|--------------------|
| Stage | Multi-select checklist (button opens a panel) | No (defaults to no stages checked, i.e. "All stages") | Options: every Bofu Status value — see [Reference: Field Values](#reference-field-values). Any number can be checked at once (OR); a **Clear** action in the panel resets to "All stages". |
| Group | Multi-select checklist (button opens a panel) | No (defaults to no groups checked, i.e. "All groups") | Options: the 6 BOFU stage groups — see [Reference: Stage Groups](#reference-stage-groups). Any number can be checked at once (OR); a **Clear** action resets to "All groups". Applies together with Stage (AND) when both have selections. |
| Servicing type | Dropdown (single-select) | No (defaults to All) | Options: All, Free Service (`FREE_SERVICE`), Paid Service (`PAID_SERVICE`). |
| Program | Dropdown (single-select) | No (defaults to All) | Options: All, Masters (`MASTERS`), Under Graduation (`UNDER_GRADUATION`). |

---

## Message Templates (fixed in v0)

**Email**
- Subject: `Your new Leap counselor: {{new_counselor_name}}`
- Body:
```
Hi {{student_first_name}},

I'm moving on from Leap, and my last day is {{last_working_day}}. It's been great working with you.

From now on, {{new_counselor_name}} will be your counselor and will take things forward from where we left off.

Your new counselor: {{new_counselor_name}} · {{new_counselor_phone}} · {{new_counselor_email}}
Team Lead: {{tl_name}} · {{tl_phone}} · {{tl_email}}
Senior Manager: {{sm_name}} · {{sm_phone}} · {{sm_email}}

Just reply to this email to reach {{new_counselor_name}} directly.

All the best,
{{leaver_name}}
```

**WhatsApp (to be submitted to Meta for approval)**
```
Hi {{1}}, your Leap counselor {{2}} has moved on. Your new counselor is {{3}} ({{4}}). You can also reach Team Lead {{5}} ({{6}}) or Senior Manager {{7}} ({{8}}). Reply here anytime.
```
The variables in order are: student first name, leaver name, new counselor name, new counselor phone, TL name, TL phone, SM name, SM phone.

**LGC task (CRM task for the new counselor)**
- Title: `Post intro in {{student_name}}'s Leap group chat`
- Due: the next day at 12 PM
- Ready message: `Hi {{student_first_name}}, I'm {{new_counselor_name}}, your new counselor at Leap, taking over from {{leaver_name}}. Looking forward to helping you. Message me here anytime.`

---

## Logic and Rules

**Request statuses**
- Pending SM approval → Approved, TL assigning (SM approves)
- Pending SM approval → Rejected (SM rejects)
- Approved, TL assigning → Cancelled (SM cancels, before 8 PM on the last working day)
- Approved, TL assigning → Transferred (the 8 PM job completes)
- Rejected → the counselor can submit a new request only after the SM clicks Reopen for counselor.
- Transferred, Rejected and Cancelled are final. Statuses never go backwards.

**Who sees the Resignation card**
- If the role is Counselor or TL → show the card.
- If the role is SM or higher → show the Resignations card (Screen 4) instead.

**Who assigns**
- If the leaver is a Counselor → their current TL and their SM can use Screen 6.
- If the leaver is a TL → only their SM can use Screen 6.

**Who can be assigned leads**
- If a counselor is active, reports under the same SM, and is not the leaver → they appear in the dropdown.
- Otherwise → they do not appear.

**Stop new lead allocation**
- If the request becomes Approved, TL assigning → exclude the leaver from lead allocation.
- If the request becomes Cancelled → include the leaver again.

**Assigned counselor becomes inactive before transfer**
- If a counselor holding pending assignments is deactivated or submits their own approved resignation → set their pending assignments back to Unassigned, send a bell to the TL ("[N] leads assigned to [Name] are unassigned again because they are no longer active"), and record it in History.

**Stage, servicing type and program filters (Screen 6)**
- The three filters apply together (AND, not OR). For example Stage = College Shortlisted + Servicing type = Paid Service + Program = Masters shows only leads matching all three.
- A lead always has exactly one Stage, one Servicing type, and one Program value — these come from the lead's existing fields in the CRM, not something this tool sets.
- The transfer, message sending and Leap group chat tasks apply to every lead regardless of Stage, Servicing type or Program — the filters only affect what the TL/SM sees while assigning, never who ends up receiving a lead.

**Stage groups (Screen 1, Screen 5, Screen 6)**
- The CRM's Bofu Status stages roll up into 6 official BOFU groups, confirmed by the team that owns the funnel. Funnel order is groups 1–4; groups 5–6 (the two drop-off groups) sit outside the linear funnel, not after Visa. The full mapping is in [Reference: Stage Groups](#reference-stage-groups):
  1. **Pre STI / Open App**
  2. **STI**
  3. **Deposit Done**
  4. **Visa**
  5. **Pre ISL Drop Off**
  6. **Post ISL Drop Off**
- A counselor is only ever assigned a lead starting at `LS_PAYMENT_DONE` — that is the start of BOFU and the start of group 1, even though it isn't the lead's very first stage overall. Stages before it (Lead Captured, Webinar Scheduled/Attended/Cancelled, Counseling Call Done, Agent Change Call Scheduled) belong to an earlier, pre-sales part of the funnel that this tool never needs to touch, because a resigning counselor cannot own a lead that hasn't reached Payment Done yet.
- If a lead's Stage is `LS_USER_ACQUIRED` or `LS_LEAD_CLOSED_AFTER_SALES_CALL` → it does not belong to any of the 6 groups (in practice this should be rare-to-never on a counselor's book). It is not hidden — it still appears normally in the Stage filter and the table, and matches "Other" in the Group filter — it is simply left out of the 6 named groups and their totals on Screens 1 and 5.
- **Pre ISL Drop Off vs Post ISL Drop Off**: a lead whose current Stage is Dead Lead or Lead Drop Off is classified using its last non-drop status, read from `calculated_lead_status_change_log`:
  - **Pre ISL Drop Off** if that last status was `LS_PAYMENT_DONE` or earlier (it went cold before ever being shortlisted).
  - **Post ISL Drop Off** if that last status was `LS_COLLEGE_SHORTLISTED` or later (it had already been shortlisted, or further along, before going cold).
  - The current Stage code alone (`LS_DEAD_LEAD` / `LS_LEAD_DROP_OFF`) cannot answer this — it always requires the status change log.
- On Screen 6, Group is an ordinary filter, not a display mode: checking a group (or several) narrows the same flat table the same way Stage, Servicing type or Program do. It does not change how rows are laid out, and it combines with every other filter (AND). The 6-group breakdown on Screens 1 and 5 is read-only summary information; Screen 6's Group filter is the only place a group selection actually changes what's shown or selectable.

**8 PM transfer job (runs daily at 8 PM for every request whose last working day is today and status is Approved, TL assigning)**
1. Auto split unassigned leads:
   - If the leaver is a Counselor → pool = active counselors in the leaver's TL team, excluding the leaver.
   - If that pool is empty, or the leaver is a TL → pool = active counselors under the same SM, excluding the leaver.
   - Sort unassigned leads by Lead ID. Deal them one at a time to the pool in round-robin order, starting with the counselor with the fewest open leads.
   - If the pool is still empty → do not transfer, keep the status as Approved, TL assigning, send a bell to the SM ("No active counselors to receive [Name]'s leads"), and retry at 8 PM the next day.
2. For each lead: change the owner to the assigned counselor, move all open tasks and follow-ups to the new owner, and keep all past calls and notes on the lead so the new owner can see them. Record the old owner on the lead as "Previous counselor".
3. Queue one email and one WhatsApp per lead:
   - If the lead has no email → mark Email "Failed: no email on lead" (no retry).
   - If the lead has no phone → mark WhatsApp "Failed: no phone on lead" (no retry).
4. Create one LGC task per lead for the new counselor.
5. Send one bell per new counselor: "You received [N] leads from [Leaver name]."
6. Set the request status to Transferred.
7. Deactivate the leaver's CRM login once every email's first send attempt has finished, whether it succeeded or failed.

**Send retries**
- If the email or WhatsApp provider returns an error → retry 20, 40 and 60 minutes after the first attempt.
- If the 3rd retry fails → mark Failed with the provider's error text and show it on Screen 4 and Screen 5.
- If any attempt succeeds → mark Sent.

**Cancellation**
- If the SM cancels before the 8 PM job starts → clear all pending assignments, set the status to Cancelled, restore lead allocation, and send a bell to the leaver and TL.
- If the job has already started → the Cancel button is hidden.

**Editing assignments**
- If the status is Approved, TL assigning → the TL or SM can assign and reassign freely.
- If the status is anything else → Screen 6 is read only.

---

## Edge Cases

| Situation | What the app does |
|-----------|-------------------|
| Counselor submits with no date | Submit stays disabled. |
| Counselor opens the page again after submitting | Shows the status tracker (Screen 3), not the form. |
| Counselor tries to submit a second request while one is active | Not possible, because the form is replaced by the tracker. |
| TL and SM assign the same lead at the same time | Last save wins. Both actions are recorded in History. |
| Filters on Screen 6 match nothing (e.g. Stage = Visa Granted + Program = Under Graduation and the leaver has no such lead) | "No leads match these filters." |
| Connection drops while assigning | Red toast "Couldn't assign. Try again." Nothing partial is saved for that click. |
| TL hasn't assigned everything by 8 PM | Leftover leads are auto split across the TL's team (see rules). |
| Assigned counselor leaves before transfer | Their rows go back to Unassigned and the TL gets a bell. |
| TL has already assigned all (or some) leads, and the SM opens the request afterwards | The SM sees the true progress ("[N] of [Total] assigned" and, if complete, a green "already assigned" note) and a **Continue assigning** or **Review assignments** button instead of a plain **Assign leads** button — so the SM doesn't start reassigning leads the TL has already placed. Same the other way if the SM assigns first and the TL opens it later. |
| SM changes the last working day to earlier than today's date | Not allowed. The picker starts from tomorrow. |
| Student has no email or no phone | That channel is marked Failed with the reason, and the other channel still sends. |
| Email or WhatsApp provider is down at 8 PM | Retried 3 times over 1 hour, then marked Failed on the SM's card. |
| Leaver has 0 leads | Flow still works. The transfer only deactivates the login. |
| A lead is edited by the leaver after SM approval (including its Stage, Servicing type or Program) | Allowed. The transfer at 8 PM uses the lead's state at that moment. |
| A lead's Stage is a value not in the CRM's current Bofu Status list (data cleanup, one-off) | It still shows in the table with its raw value and can still be assigned; it just won't match any of the named Stage filter options, only "All". |
| A lead's Stage is `LS_USER_ACQUIRED` or `LS_LEAD_CLOSED_AFTER_SALES_CALL` | Shows normally in the Stage filter and table on Screen 6, and matches "Other" in the Group filter. Left out of the 6 named group totals on Screens 1 and 5 — see Logic and Rules. |
| A lead's Stage is a pre-payment stage (Lead Captured, Webinar *, Counseling Call Done, Agent Change Call Scheduled) | Should not occur on a counselor's book in practice, since ownership starts at Payment Done. If it ever does (data cleanup, one-off), it behaves like User Acquired above: visible in the flat filter, left out of the 6 groups. |
| `calculated_lead_status_change_log` has no earlier status for a dead/dropped-off lead (edge case in the data) | Treat it as Pre ISL Drop Off (it never reached College Shortlisted, by definition, if there's no earlier status at all). |
| Resignations already in progress at launch | Finished the old manual way. The tool is used for new resignations only. |
| TL resigns | Same flow. The SM approves and the SM assigns. |
| App opened on a mobile browser | Cards stack to one column. Screen 6 table scrolls sideways. All actions still work. |

---

## Admin and Settings
No admin screens in v0. Message wording is fixed in code, and the transfer time (8 PM) and retry timings are set in config by a developer. The Stage, Servicing type and Program value lists are read from the CRM's existing lead fields, not configured inside this tool — if the CRM adds a new Bofu Status value, it appears automatically in the Stage filter with no code change here.

---

## FAQs

**What if the TL is on leave and nobody assigns the leads?**
The SM can open the same assignment screen. If nobody assigns anything, every lead is split evenly across the TL's team at 8 PM on the last working day.

**What if the counselor changes their mind after submitting?**
They can't withdraw it themselves. They talk to their SM, who can cancel any time before 8 PM on the last working day. All assignments are cleared and they start getting new leads again.

**What if the SM rejects by mistake?**
The SM clicks Reopen for counselor, and the counselor submits again.

**Why email from the leaver's account when their login is deactivated right after?**
The email is a personal goodbye, so it comes from them. Reply-To is set to the new counselor, so any reply goes to the right person. The CRM login is deactivated only after the first send attempt, and retries use the CRM's stored email connection. Whether the mailbox itself is closed is up to HR or IT, not this tool.

**Will messaging students in a closed-out stage (Dead Lead, Lead Drop Off, Admit Declined, and so on) cause problems?**
You chose to message everyone regardless of Stage. Sending WhatsApp messages to students who haven't engaged recently can lead to blocks or reports and lower the quality rating of the company number. Watch the WhatsApp number's quality rating in the first month. If it drops, limit messages to leads in active stages.

**What about the Leap group chat?**
In v0, each new counselor gets a task per student with a ready intro message to post. Auto-joining the group with a system message is planned for v1.

**Why did the old "Program" filter (MS in US, MBA, UG in Canada, and so on) go away?**
Those were placeholder values from the first draft of this PRD, not real CRM fields. The CRM actually holds two separate fields on each lead — Servicing type (Free Service or Paid Service) and Program (Masters or Under Graduation) — so Screen 6 now filters on those two real fields instead of one made-up one.

---

## What v0 Does NOT Include
- **Auto-joining the new counselor to the Leap group chat with a system message.** This depends on whether the group chat tool has an API. It is planned for v1, and v0 uses a CRM task instead.
- **Template editor for email or WhatsApp.** Wording rarely changes, and WhatsApp changes need Meta approval anyway.
- **CSV export of transfers.** Removing exports is part of the goal. The CRM already shows each lead's owner and previous counselor.
- **Automatic rule-based reassignment (by capacity, stage, servicing type or program).** TLs assign by hand. The only automatic rule is the even split of leftover leads at 8 PM.
- **Editing a lead's Stage, Servicing type or Program from inside this tool.** Those fields are owned by the rest of the CRM; this tool only reads and filters on them.
- **Email or WhatsApp alerts to staff.** All staff alerts are CRM bell notifications.
- **Counselor withdrawal or resubmission without the SM.** The SM controls cancel and reopen.
- **Backfilling resignations already in progress at launch.** These finish the old way.
- **HR or payroll integration.** Resignation acceptance and the notice period stay with HR.

---

## How You'll Know It's Working
- Zero leads are owned by deactivated counselors. Check with one CRM query each week.
- The SM pod gets no export or reassignment requests for resignations after launch.
- Each resignation's SM card shows email and WhatsApp as Sent for almost every student on the last working day, with Failed rows limited to missing contact details.
- Leavers and TLs stop asking the SM "where is my resignation?", because the tracker answers it.
- TLs say the Stage, Group, Servicing type and Program filters make it faster to hand similar leads to the counselor best suited for them (for example, keeping Masters leads with a counselor who already works Masters applications, or working through the Visa group in one pass).

---

## Reference: Field Values

### Reference: Stage Groups
The 6 official BOFU groups shown on Screens 1, 5 and 6, and which Bofu Status stages fall into each one. This is the authoritative bucketing confirmed by the team that owns the Bofu Status funnel. Groups 1–4 are funnel order; groups 5–6 sit outside the linear funnel (a lead can drop off at any point, not only at the end).

**1. Pre STI / Open App** — everything before the application reaches the institute. Starts at Payment Done because that's the first stage a counselor is ever assigned a lead at (see Logic and Rules), not because it's the lead's first stage overall:
`LS_PAYMENT_DONE`, `LS_COLLEGE_SHORTLISTED`, `LS_COLLEGE_FINALIZED`, `LS_APPLICATION_PROCESS_STARTED`, `LS_APPLICATION_PROCESS_ON_HOLD`, `LS_APPLICATION_IN_PROCESS`, `LS_APPLICATION_SUBMITTED_TO_AGGREGATOR`, `LS_APPLICATION_ON_HOLD_BY_AGGREGATOR`, `LS_APPLICATION_REJECTED_BY_AGGREGATOR`.

**2. STI** — the application has reached the institute; every outcome from there, admit or reject, lives here (no separate "Admit" group in v0):
`LS_APPLICATION_SUBMITTED_TO_INSTITUTE`, `LS_APPLICATION_ON_HOLD_BY_INSTITUTE`, `LS_ADMIT_REJECTED_BY_INSTITUTE`, `LS_CONDITIONAL_ADMIT_RECEIVED`, `LS_UNCONDITIONAL_ADMIT_RECEIVED`, `LS_ADMIT_RECEIVED`, `LS_ADMIT_ACCEPTED`, `LS_ADMIT_DECLINED`, `LS_OFFER_REVOKED_BY_UNIVERSITY`.

**3. Deposit Done**:
`LS_DEPOSIT_PAID`, `LS_TUITION_FEE_PAID`.

**4. Visa** — every visa status regardless of outcome:
`LS_VISA_FILING_STARTED`, `LS_VISA_WIP`, `LS_VISA_APPLIED`, `LS_VISA_PROCESS_ON_HOLD`, `LS_VISA_GRANTED`, `LS_VISA_REJECTED`, `LS_VISA_DROPPED`.

**5. Pre ISL Drop Off** and **6. Post ISL Drop Off** — the same two stage codes, split per lead by history, not by stage code (see Logic and Rules):
`LS_LEAD_DROP_OFF`, `LS_DEAD_LEAD`.

**Not in any of the 6 groups** — either a pre-payment stage (never on a counselor's book, since ownership starts at Payment Done) or a code the funnel bucketing doesn't cover:
`LS_LEAD_CAPTURED`, `LS_WEBINAR_SCHEDULED`, `LS_WEBINAR_ATTENDED`, `LS_WEBINAR_CANCELLED`, `LS_COUNSELING_CALL_DONE`, `LS_AGENT_CHANGE_CALL_SCHEDULED`, `LS_USER_ACQUIRED`, `LS_LEAD_CLOSED_AFTER_SALES_CALL`.

### Lead stage (CRM field: Bofu Status)
The full set of values this field can hold today, taken from a CRM export (`docs/lead-stage-values.csv`). All of these are selectable in the Screen 6 Stage filter; only the ones a given leaver actually has leads in are shown as rows on Screens 1 and 5.

| Value stored in the CRM | Shown in the UI as |
|---|---|
| `LS_LEAD_CAPTURED` | Lead Captured |
| `LS_WEBINAR_SCHEDULED` | Webinar Scheduled |
| `LS_WEBINAR_ATTENDED` | Webinar Attended |
| `LS_WEBINAR_CANCELLED` | Webinar Cancelled |
| `LS_COUNSELING_CALL_DONE` | Counseling Call Done |
| `LS_AGENT_CHANGE_CALL_SCHEDULED` | Agent Change Call Scheduled |
| `LS_COLLEGE_SHORTLISTED` | College Shortlisted |
| `LS_COLLEGE_FINALIZED` | College Finalized |
| `LS_APPLICATION_PROCESS_STARTED` | Application Process Started |
| `LS_APPLICATION_IN_PROCESS` | Application In Process |
| `LS_APPLICATION_PROCESS_ON_HOLD` | Application Process On Hold |
| `LS_APPLICATION_SUBMITTED_TO_AGGREGATOR` | Application Submitted To Aggregator |
| `LS_APPLICATION_SUBMITTED_TO_INSTITUTE` | Application Submitted To Institute |
| `LS_APPLICATION_ON_HOLD_BY_AGGREGATOR` | Application On Hold By Aggregator |
| `LS_APPLICATION_ON_HOLD_BY_INSTITUTE` | Application On Hold By Institute |
| `LS_APPLICATION_REJECTED_BY_AGGREGATOR` | Application Rejected By Aggregator |
| `LS_CONDITIONAL_ADMIT_RECEIVED` | Conditional Admit Received |
| `LS_UNCONDITIONAL_ADMIT_RECEIVED` | Unconditional Admit Received |
| `LS_ADMIT_RECEIVED` | Admit Received |
| `LS_ADMIT_ACCEPTED` | Admit Accepted |
| `LS_ADMIT_DECLINED` | Admit Declined |
| `LS_ADMIT_REJECTED_BY_INSTITUTE` | Admit Rejected By Institute |
| `LS_OFFER_REVOKED_BY_UNIVERSITY` | Offer Revoked By University |
| `LS_DEPOSIT_PAID` | Deposit Paid |
| `LS_PAYMENT_DONE` | Payment Done |
| `LS_TUITION_FEE_PAID` | Tuition Fee Paid |
| `LS_VISA_FILING_STARTED` | Visa Filing Started |
| `LS_VISA_APPLIED` | Visa Applied |
| `LS_VISA_WIP` | Visa WIP |
| `LS_VISA_PROCESS_ON_HOLD` | Visa Process On Hold |
| `LS_VISA_GRANTED` | Visa Granted |
| `LS_VISA_REJECTED` | Visa Rejected |
| `LS_VISA_DROPPED` | Visa Dropped |
| `LS_USER_ACQUIRED` | User Acquired |
| `LS_LEAD_CLOSED_AFTER_SALES_CALL` | Lead Closed After Sales Call |
| `LS_LEAD_DROP_OFF` | Lead Drop Off |
| `LS_DEAD_LEAD` | Dead Lead |

A lead with a blank/missing Bofu Status (498 such leads org-wide in the export used to build this list) is treated the same as any other lead — it still appears in the table and can still be assigned — but it will not match any named Stage filter, only "All".

### Servicing type (new field)
| Value stored in the CRM | Shown in the UI as |
|---|---|
| `FREE_SERVICE` | Free Service |
| `PAID_SERVICE` | Paid Service |

### Program (new field, replaces the placeholder program list from the first draft)
| Value stored in the CRM | Shown in the UI as |
|---|---|
| `MASTERS` | Masters |
| `UNDER_GRADUATION` | Under Graduation |

---

## Open Questions for the Build
1. Does the CRM already store a sending connection (for example Gmail or Outlook) for each counselor's email? If not, "from the leaver's account" needs one set up per counselor before their last day.
2. Which WhatsApp Business provider does the company number use (for example Gupshup, Interakt or the Meta Cloud API)? The template must be submitted there for approval.
3. Where do the Leap group chats live, and do they have an API? This decides whether v1 auto-join is possible.
4. Confirm the CRM role field values for Counselor, TL and SM, and how "same SM" and "TL team" are stored.
5. What phone number and email should be shown for the TL and SM in student messages: their work numbers from the CRM profile?
6. Confirm that CC'ing debasish.sahoo@leapfinance.com on every student email is intended. At 3 to 5 exits a month with 200 to 500 leads each, that is roughly 600 to 2,500 emails a month.
7. Confirm the exact field names in the CRM's database/API for Bofu Status, Servicing type and Program (the names above are the values seen in exports and product conversations, not confirmed schema/column names).
8. Are Servicing type and Program always set on every lead, or can they be blank? If they can be blank, the Stage filter's "blank still shows under All" behaviour (see Reference section) should apply to them too.
9. Is the CSV export used to build the Stage reference list (`docs/lead-stage-values.csv`) the full, current list of Bofu Status values, or does the CRM's schema allow values that hadn't been used yet at export time?
10. ~~Does the CRM keep a history of stage changes per lead?~~ **Resolved during PRD review:** yes — `calculated_lead_status_change_log` is the source for a lead's last non-drop status, which is what decides Pre ISL Drop Off vs Post ISL Drop Off (see Logic and Rules). Confirm with engineering exactly how this log is queried (table/API) before build.
11. ~~Confirm the stage groups and their membership~~ **Resolved during PRD review:** the 6-group bucketing in [Reference: Stage Groups](#reference-stage-groups) (Pre STI / Open App, STI, Deposit Done, Visa, Pre ISL Drop Off, Post ISL Drop Off) is confirmed authoritative by the team that owns the Bofu Status funnel, including Application Rejected By Aggregator sitting under Pre STI / Open App (not a drop-off) and Payment Done opening that same group as the start of BOFU.
12. The 8 Bofu Status codes outside the 6 official groups (`LS_LEAD_CAPTURED`, the three `LS_WEBINAR_*` codes, `LS_COUNSELING_CALL_DONE`, `LS_AGENT_CHANGE_CALL_SCHEDULED`, `LS_USER_ACQUIRED`, `LS_LEAD_CLOSED_AFTER_SALES_CALL`) should never appear on a counselor's book in practice, since ownership starts at Payment Done — confirm this is actually enforced by the CRM (i.e. a lead is only ever assigned to a counselor once it reaches Payment Done), rather than something this tool needs to defend against with its own logic.
