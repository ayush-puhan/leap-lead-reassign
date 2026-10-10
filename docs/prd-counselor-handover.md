# Lead Handover — PRD (final)

Mirrors the Word/Google Doc PRD: an Overview tab and a Detailed tab.

---

# Overview


## Lead Handover — PRD


[Vercel Link](https://leap-lead-reassign.vercel.app/)


### Goal


Create a self-serve handover flow in the Counsellor CRM so that when a counsellor resigns, every lead moves to a new counsellor, every student is told who their new counsellor is, and nothing is done by hand.


### Objective


Zero leads left with a counsellor who has left, and every student told who their new counsellor is 7 days before the old counsellor leaves.


### Principles

- No lead should ever be left without an owner
- The leaver, TL and SM should always know where the request stands without asking anyone
- The TL stays in control of who gets which lead
- The old and new counsellor overlap for at least 7 days before the old counsellor leaves

## Requirements


#### Counsellor — Last Working Day (View Profile)

- As a counsellor or TL, I open View Profile from my avatar (top right) and see a Last Working Day card below my profile
- I only enter my last working day (tomorrow or later) — the rest happens automatically
  - I see my total leads (e.g. 325) with the stage-group breakdown in small text; clicking it shows the actual leads, read only
  - Before submitting I see when my leads will transfer: 8 PM, 7 days before my last working day
- On Submit I see a red confirm: my name, last working day, total leads and "[TL] will transfer your leads by [date], 8 PM"
- I cannot withdraw after submitting — only my SM can cancel
- After submitting, the card shows the transfer date and a tracker: LWD submitted → SM approval → TL schedules leads → Leads transfer → Last working day
  - If rejected, I see the SM's reason. Once the SM reopens it, I can pick a new date and apply again — the reason stays visible

#### SM — Last Working Day Approval (Tasks & Performance)

- As an SM, I see a Last Working Day Approval tile in Important Business Tasks with the pending count
- Clicking it opens a full-page modal: names of pending counsellors as pills, and a table of every request (latest per leaver)
  - Columns: Counsellor name, TL name, Last working day, Leads, Status, Take action
  - Sorted: Pending (nearest LWD first), Rejected, Approved, Transferred. Records drop off once the login is deactivated
- Review opens a modal with the counsellor, TL, LWD, leads and scheduled transfer. I can
  - Approve — with a confirm step
  - Change last working day
  - Reject (reason mandatory)
- On an approved request I can Cancel any time before the transfer — all scheduled assignments get cleared
- On a rejected request I can Reopen, so the counsellor can apply again
- Once approved, new leads stop being allocated to the leaver

#### TL — Schedule Reassigned Leads (Tasks & Performance)

- As a TL, once the SM approves I get a bell alert and a Schedule Reassigned Leads tile in Important Business Tasks
  - SM sees the same tile for every team (and only the SM schedules if the leaver is a TL or has no TL)
- Clicking it opens a full-page modal with one accordion per leaving counsellor; one opens at a time
- Inside, I filter (Stage, Group, Servicing type, Program, Show) and click rows to select leads
  - Stage and Group each have Select all; only one filter list is open at a time
- I pick a counsellor and click Schedule assignment, then confirm — nothing moves until the transfer
  - Only active counsellors under the same SM appear, with their existing open leads
  - I can change any assignment until the transfer runs

#### Transfer (automatic, 8 PM, 7 days before LWD)

- At 8 PM, 7 days before the last working day (or 8 PM the day after approval on short notice), the CRM will
  - Split any unscheduled leads evenly across the TL's team
  - Move every lead with its open tasks, follow-ups, calls and notes
  - Send every student an email (from the leaver's account) and a WhatsApp (company number) with new counsellor, TL and SM details
  - Create Connect with Reassigned Leads tasks (Boost STI / Boost Deposit) and a bell alert for each new counsellor
- Failed messages retry 3 times over 1 hour
- The old counsellor stays active, and in the Leap group chats, until their last working day

#### Last Working Day (automatic)

- On the last working day the old counsellor is removed from the Leap group chats and their CRM login is deactivated; the record drops off the SM's list

#### New Counsellor — Tasks

- Connect with Reassigned Leads task for leads in Boost STI or Boost Deposit
  - Shows inside the existing Boost STI and Boost Deposit pipelines with a "Reassigned from [leaver]" badge, a Reassigned filter, and a reassigned count on the card
  - Closes when a meeting is booked and joined, or a call connects
  - Same tasks roll up into the TL's view
- No Leap group chat task in the CRM

#### Leap Group Chat (outside the CRM)

- After the 8 PM transfer, each new counsellor gets a WhatsApp notification to join each student's Leap group chat ("Join this group")
- Once they join, the LGC message is posted in the group from the LS number (Meta template built by Swapnil)
  - Carries student name, old counsellor (ID, email, name, LWD), new counsellor (ID, email, name) and SM (ID, email, name)
- If the student hasn't joined the LGC, they get a WhatsApp DM asking them to join — logic to be discussed with Swarnim

### Edge cases

- TL hasn't scheduled all leads by the transfer → leftover leads are split evenly across the TL's team
- Scheduled counsellor leaves before transfer → those leads go back to Unscheduled and the TL gets a bell alert
- LWD less than 7 days away → leads transfer at 8 PM the day after SM approval (short-notice warning shown)
- TL resigns, or a counsellor has no TL → same flow, SM approves and SM schedules

### Not in v0

- Template editor or preview for email or WhatsApp
- Per-student message list in the CRM
- Leap group chat tasks in the CRM (handled on WhatsApp)
- Reassigning leads between active counsellors
- HR and payroll integration

To be confirmed/Pending (Ayush Puhan):

- Done: existing lead reassignment tasks — new tasks are created based on Boost STI and Boost Deposit.
- Done: Leap group chat intros moved out of the CRM — new counsellors get a WhatsApp "Join this group" notification after the transfer.
- Not in scope of the current release: reassigning leads between active counsellors. This happens rarely, for 2–3 counsellors, and is communicated by POD or Counsellor Support.
- Open: overlap duration — 7 days in this design (a config value); confirm 7 vs 14 days with DS.
- Open: show incoming leads to the receiving counsellor after SM approval? Not built in v0 because of the risk of gossip about who is leaving.
- Open: logic for the join-LGC DM to students (when it is sent, reminders, how "not joined" is detected, join link) — with Swarnim.
- Open: Meta templates — student WhatsApp, LGC message and the new-counsellor join notification — to be built by Swapnil.

---

# Detailed


Lead Handover — Final PRD


## Objective


Give every resigning counsellor a self-serve way to hand over their leads inside the Counsellor CRM. The counsellor enters their last working day, the SM approves it, the TL schedules who gets each lead, and at 8 PM, 7 days before the last working day, the CRM moves every lead and messages every student on its own. The old counsellor stays active for that week so the new counsellors overlap with them, and their login is deactivated on the last working day — with one record of who got what. This document specifies every screen, filter, piece of copy, and rule needed to build it.


Scale: 3 to 5 resignations a month, 200 to 500 leads per leaver — roughly 600 to 2,500 leads a month that need a new owner, a student message, and a follow-up.


## Who This Is For

- Counsellor (leaver) — enters their last working day on View Profile and tracks progress. Uses it once.
- TL (Team Lead) — the leaver's TL. Schedules who gets each lead, from Tasks & Performance.
- SM (Senior Manager) — approves, rejects, changes the date, reopens or cancels requests, from Tasks & Performance. Can also schedule leads (when the TL is away, or when the leaver is a TL or has no TL).
- New counsellor (receiver) — does not use the handover flow. Gets a bell alert when leads arrive, Connect tasks in Boost STI / Boost Deposit, and a WhatsApp notification to join each student's Leap group chat.
- Students — not CRM users. Receive an email and a WhatsApp message at the transfer, and the LGC message once the new counsellor joins their group.

Login: everyone uses their existing CRM login. What each person sees depends on their existing CRM role. No new login is needed.


Reporting hierarchy: most counsellors report to one TL, and some report straight to an SM with no TL; every TL reports to one SM. "Same SM" means every counsellor under that SM.


## End-to-End Flow


| Step | Who | What happens | Request status after |
|---|---|---|---|
| 1 | Counsellor | Avatar → View Profile → Last Working Day card → picks last working day → Submit → red confirm → Yes, submit. | Pending SM approval |
| 2 | SM | Bell alert or Tasks & Performance → Last Working Day Approval tile → Review → Approve (with confirm), Change last working day, or Reject with a reason. | Approved · TL assigning (or Rejected) |
| 3 | CRM | On approval: stops new lead allocation to the leaver. Bell to the TL and the leaver; the TL's Schedule Reassigned Leads tile count goes up. | Approved · TL assigning |
| 4 | TL (or SM) | Schedule Reassigned Leads tile → opens the leaver → filters → selects rows → Schedule assignment → confirm. Can change any assignment until the transfer. | Approved · TL assigning |
| 5 | CRM, 8 PM, 7 days before LWD | Splits unscheduled leads across the TL's team → moves every lead → emails and WhatsApps every student → creates Connect tasks and bell alerts. New counsellors get a WhatsApp notification to join each student's Leap group chat. | Leads transferred |
| 6 | Old counsellor | Stays active and in the Leap group chats for the overlap week, helping the new counsellors settle in. | Leads transferred |
| 7 | CRM, last working day | Removes the old counsellor from the Leap group chats and deactivates their login. The record drops off the SM's list. | Deactivated |


## Where It Lives


| Who | Where | Entry point |
|---|---|---|
| Counsellor (or TL resigning) | View Profile page, below the profile card | Avatar (top right) → View Profile → Last Working Day card |
| SM | Tasks & Performance → Important Business Tasks (TL & POD rollup) | Last Working Day Approval tile → full-page modal |
| TL, and SM for any team | Tasks & Performance → Important Business Tasks | Schedule Reassigned Leads tile → full-page modal |
| New counsellor | Tasks & Performance → existing Boost STI / Boost Deposit cards | View Pipeline → reassigned students with a Reassigned badge |


The Learning & Development tab is unchanged. The two tiles sit in the existing Important Business Tasks grid next to Drop Off Approval and the other approval tasks, with the same count and load label (CLEAR at 0, MODERATE up to 20, HIGH above 20).


## Counsellor: View Profile


### 1. Last Working Day Card

- Placed on the View Profile page (avatar menu, top right), below the profile card. Same card style as the rest of the CRM (icon, title, subtitle, expand arrow).
- Title: "Last Working Day". Subtitle: "Enter your last working day — the rest happens automatically". No NEW badge.
- Visible to every user with the Counsellor or TL role (a TL can resign too). Not shown to SMs.
- A user can have only one request that is Pending SM approval, Approved · TL assigning or Leads transferred at a time.
- Once submitted, the counsellor cannot withdraw. Only the SM can cancel.

#### 1.1 Card States


| State | When | What the user sees | Actions |
|---|---|---|---|
| Not submitted | No active request, or last request was Cancelled, or Rejected and reopened by the SM. | Intro text, Your leads (total + breakdown), Last working day picker, transfer-date preview, Submit button. | Pick a date, Submit |
| Pending SM approval | After Yes, submit. | Headline "Your leads transfer on [date], 8 PM" (expected), tracker at SM approval, status, total leads. Note: "You can't withdraw this. Talk to your SM if you need to cancel it." | None |
| Approved · TL assigning | SM approved. | Headline with the confirmed transfer date, tracker at TL schedules leads ("[x] of [y] scheduled"). Last working day, with "(changed by [SM] from [date])" if changed. Note: "New leads are no longer assigned to you." | None |
| Rejected | SM rejected. | Red Rejected badge, the SM's reason, and "Please speak to your SM. Once they reopen your request you can pick a new date and apply again." | None until reopened |
| Rejected, reopened | SM clicked Reopen. | The last rejection (date, SM, reason, reopened date) above the Not submitted form. The button reads Apply again. | Pick a new date, Apply again |
| Cancelled | SM cancelled. | Grey "Cancelled by [SM name]" badge with date and time, above the Not submitted form. | Submit a new date |
| Leads transferred | Transfer job done. | Headline "Your leads moved on [date], 8 PM" and "You stay in your students' Leap group chats until your last working day, [date], to help the new counsellors settle in." Tracker done up to Leads transfer. | None |
| Deactivated | Last working day. | Login deactivated screen: "Your CRM login has been deactivated. Your [N] leads moved on [date], and you left your students' Leap group chats on your last working day, [date]. Thank you for your work at Leap." | None |


#### 1.2 Submit Form


Intro text (exact copy): "Enter your last working day — that's all you need to do. Your SM approves it, your TL schedules who gets each lead, and your leads transfer automatically at 8 PM, 7 days before your last working day. Students are told by email and WhatsApp."


| Field | Type | Required | Notes |
|---|---|---|---|
| Last working day | Date picker | Yes | Tomorrow or later. Earlier dates are greyed out. Submit stays disabled until a valid date is picked. |

- Transfer preview, shown under the picker as soon as a date is picked: "Your leads will transfer on [Day, DD Mon YYYY], 8 PM — 7 days before your last working day, so you overlap with the new counsellors."
- Short notice (date is less than 7 days away): amber note "Less than 7 days away. Your leads will transfer at 8 PM the day after your SM approves (if approved today: [date])."
- On Submit: opens the Confirm modal (1.4).
- Date no longer valid at submit (page left open overnight): red text under the field, "Pick a date from tomorrow onwards."
- Empty state (user owns 0 leads): "You have no leads. Nothing will be transferred." Submit still works.
- Error state (lead counts fail to load): "Couldn't load your lead counts. Refresh the page." Submit stays disabled.

#### 1.3 Your Leads

- The total is the headline (e.g. "325 leads"), with the stage-group breakdown in small text below it (e.g. "Pre STI / Open App 142 · STI 135 · Visa 8 · Pre ISL Drop Off 25 · Post ISL Drop Off 15"). Groups with 0 leads are left out. No boxes or tabs, so it doesn't look like leads can be picked one group at a time.
- Clicking it ("View leads ›") opens a read-only lead list modal: title "[Name]'s leads · [N]", the note "Read only. Every lead moves at the transfer.", a search box (student name or Lead ID), and one expandable section per stage group with a table: Student, Lead ID, Stage, Program, Next follow-up.
- The same lead list opens from the SM's Review modal ("view leads") and the request detail page.

#### 1.4 Confirm Modal (red, high emphasis)


| Element | Copy / Behaviour |
|---|---|
| Header (red band) | "Submit your last working day?" and "This can't be undone. Only your SM can cancel it." |
| Details | Counsellor: [name] · Last working day: [Day, DD Mon YYYY] · Total leads: [N] |
| Transfer note (red) | "[TL name] (your TL) will transfer your leads by [Day, DD Mon YYYY], 8 PM." On short notice it adds: "Less than 7 days' notice — this is the day after approval if your SM approves today." |
| Go back | Closes the modal. Returns to the form with the date kept. |
| Yes, submit (red button) | Creates the request (Pending SM approval), sends a bell to the SM, and switches the card to the tracker. |
| Error | Red text in the modal: "Couldn't submit. Check your connection and try again." Modal stays open. |


#### 1.5 Status Tracker

- Works like a delivery tracker: a fixed start (LWD submitted) and a fixed end (Leads transfer, a known date at 8 PM), with every step and its date visible, so the counsellor can tell students exactly when their lead moves.
- Above the tracker, the headline: "Your leads transfer on [date], 8 PM" — marked expected until the SM approves.

| Step | Shows | Done when |
|---|---|---|
| 1. LWD submitted | Submitted date and time | Always done |
| 2. SM approval | Approval date and time, or "Waiting for [SM name]" | SM approves |
| 3. TL schedules leads | "[x] of [y] scheduled", or "After approval" | Transfer runs |
| 4. Leads transfer (fixed end) | [Day, DD Mon YYYY], 8 PM — "(expected)" until approved | Transfer runs |
| 5. Last working day | [Day, DD Mon YYYY] · leave Leap group chats | Last working day |

- Read only — no buttons. Error state: "Couldn't load your request. Refresh the page."

## SM: Last Working Day Approval


### 2. Last Working Day Approval Tile and Modal

- Tile in Important Business Tasks — TL & POD rollup on the SM's Tasks & Performance: "Last Working Day Approval", count of pending requests, load label.
- Clicking it opens a full-page modal: "← Back to Tasks & Performance" at the top left, the title, and × at the top right. Esc also closes it. The page behind doesn't scroll.
- Subtitle: "Counsellors and TLs in your teams who have entered a last working day · [N] pending. Records drop off once the counsellor's login is deactivated."
- When more than one request is pending, their names show as pills above the table ("Pending approval: Priya Rao · LWD 06 Oct"). Clicking a pill opens Review.

#### 2.1 Requests Table


| Column | What it shows |
|---|---|
| Counsellor name | Leaver's name |
| TL name | Leaver's TL |
| Last working day | Final date (after any SM change) |
| Leads | Total leads owned by the leaver |
| Status | Status pill (2.2), with "Approved [date]" under it once approved |
| Take action | Pending → Review · Approved → Assign leads · Rejected → Reopen · Transferred → View |

- One row per leaver, showing their latest request only (a rejected request is replaced once the leaver applies again).
- Sort: Pending first (nearest last working day first), then Rejected, Approved, Transferred, Cancelled.
- Deactivated records are hidden — once a login is deactivated the row disappears, so no date filter is needed.
- Clicking a row does the same as its Take action. Assign leads switches the modal to Schedule Reassigned Leads with that leaver open (Section 3). View opens the request detail page.

| State | What shows |
|---|---|
| No requests | "No last working day requests." |
| Load fails | "Couldn't load requests. Refresh the page." |


#### 2.2 Status Pills


| Status | Pill colour |
|---|---|
| Pending SM approval | Amber |
| Approved · TL assigning | Blue |
| Leads transferred | Green |
| Rejected | Red |
| Cancelled | Grey |
| Deactivated | Grey (hidden from the list) |


#### 2.3 Review Modal

- Opens on top of the full-page modal, so the SM can work through several requests without leaving it.

| Element | Copy / Behaviour |
|---|---|
| Title | "Review last working day" |
| Details | Counsellor · TL · "Last working day entered by [counsellor name]" · Leads [N] with "view leads" (opens the lead list, 1.3) · Submitted · Scheduled transfer ([Day, DD Mon YYYY], 8 PM) |
| Note | "Leads transfer 7 days before the last working day. The counsellor stays in their students' Leap group chats until [date]." On short notice (amber): "Short notice. Less than 7 days to the last working day, so leads transfer at 8 PM the day after approval." |
| Buttons | Reject · Change last working day · Approve |


#### 2.4 Actions


| Action | What happens |
|---|---|
| Approve | Opens a confirm: "Approve [Name]'s last working day?" listing: last working day; "New leads stop going to [Name] now."; "[TL] gets a task to schedule the [N] leads."; "Leads transfer on [date], 8 PM."; "Login is deactivated on the last working day." Buttons Back / Confirm approval. On confirm: status → Approved · TL assigning, approval date recorded, leaver removed from allocation, bell to the TL and the leaver. |
| Change last working day | Date picker (tomorrow or later, pre-filled) → Continue → the same approval confirm, showing "(changed from [X])". History records "Last working day changed from [X] to [Y]". Invalid date: "Pick a date from tomorrow onwards." |
| Reject | "Reject [Name]'s last working day" with a Reason box (required, 1 to 500 characters) → Reject. Status → Rejected. Bell to the leaver with the reason. Empty reason: "Add a reason." |
| Reopen | From the table on a Rejected request. The counsellor's card returns to the form with the last rejection reason still shown. Bell to the leaver with the reason. |
| Cancel | From the request detail page on an approved request: "Cancel [Name]'s last working day? All scheduled assignments will be cleared and they will start getting new leads again." Keep it / Cancel request. On confirm: assignments cleared, status → Cancelled, leaver back in allocation, bell to the leaver and TL. Hidden once the transfer has started. |
| Any action fails | Red text: "Couldn't save. Try again." Status does not change. |


#### 2.5 Request Detail (View)

- Opened from View in the table. Back link: "← Back to Tasks & Performance".
- Header: name, role, TL, SM, status pill. Details: Submitted, SM approved, Last working day, Leads transfer ([date], 8 PM), Scheduled ([x] / [y]). The lead total with "View leads".
- Leads scheduled for / Leads transferred to: one row per receiving counsellor — Counsellor, TL, Leads, By stage group — plus an Unscheduled row before the transfer ("Split evenly across [TL]'s team at transfer").
- Student messages (after transfer): Email and WhatsApp counts — sent, failed, retrying. No per-student list.

#### 2.6 History

- Timeline of every action, newest first, with time, person and details. Visible to the SM and the leaver's TL.
- Shows the latest request only — if a rejected counsellor applies again, History starts fresh from the new submission.
- Examples: "Last working day submitted: 06 Oct 2026 · Priya Rao", "Scheduled 30 leads for Sneha Kulkarni (move 02 Oct 2026, 8 PM) · Ankit Sharma".

## TL (and SM): Schedule Reassigned Leads


### 3. Schedule Reassigned Leads Tile and Modal

- Tile in Important Business Tasks for the TL (own team) and the SM (every team): "Schedule Reassigned Leads", count of approved leavers waiting for the transfer, load label.
- Clicking it opens the same full-page modal. Subtitle: "Open a counsellor to schedule who gets each lead. Leads only move at 8 PM on their transfer date."
- One accordion per leaving counsellor, sorted by transfer date. Only one opens at a time, so the TL works on one counsellor at a time.
- Accordion header: initials, name, "LWD [date] · leads move [Day, DD Mon YYYY], 8 PM", "[x] / [y] scheduled" with a progress bar, status pill.
- TL view: leavers still waiting for SM approval are listed below, not openable ("you can schedule once the SM approves").
- Opened from the bell, or from Assign leads in the SM's requests table, the modal opens with that counsellor's accordion already open.

#### 3.1 Inside an Accordion

- Note: "Nothing moves until [Day, DD Mon YYYY], 8 PM. Anything left unscheduled then is split evenly across [TL name]'s team." Once everything is scheduled (green): "All [N] leads are scheduled. They move at [date], 8 PM — you can still change any of them until then."
- Lists every lead the leaver owns, in whatever stage it is in now — including Dead Lead and Lead Drop Off. One flat table, 50 rows per page.

#### 3.2 Filters


| Filter | Type | Options | Default | Behaviour |
|---|---|---|---|---|
| Stage | Multi-select checklist | Every Bofu Status value (Field Values Reference) | All stages | Select all at the top checks every stage (unchecking clears them). Button reads "All stages" (none or every stage checked), the one stage's name, or "[N] stages selected". Clear resets. |
| Group | Multi-select checklist | Pre STI / Open App, STI, Deposit Done, Visa, Pre ISL Drop Off, Post ISL Drop Off, Other | All groups | Select all at the top checks every group. Checking a group shows every lead in any of its stages. Button reads "All groups" when none or every group is checked. Clear resets. |
| Servicing type | Single-select | All, Free Service (FREE_SERVICE), Paid Service (PAID_SERVICE) | All | — |
| Program | Single-select | All, Masters (MASTERS), Under Graduation (UNDER_GRADUATION) | All | — |
| Show | Toggle | All / Unscheduled / Scheduled | All | — |

- All filters apply together (AND). They only change what the TL/SM sees; the transfer, student messages and tasks apply to every lead.
- All filter controls are the same height and width. Only one filter list (Stage or Group) is open at a time: opening one closes the other, and clicking outside or using another filter closes it.
- Stage, Servicing type and Program are read from the lead's existing CRM fields. This screen never edits them.

#### 3.3 Table and Selecting


Columns: Checkbox · Student · Lead ID · Stage · Servicing type · Program · Last contacted · Next follow-up · Scheduled for. Default sort: Next follow-up, soonest first, empty dates last. Click any column header to sort.

- Scheduled for is a status pill: blue with the counsellor's name, or amber "Unscheduled".
- The whole row is clickable to select it, not just the checkbox. The header checkbox selects every row matching the filters. There is no separate "Select all shown" button.
- Above the table: "Click rows to select them, then pick who to schedule them for." When rows are selected it changes to "[N] selected" and a red Clear selection button.

#### 3.4 Scheduling


With rows selected, a bar is pinned to the bottom of the modal: "[N] selected", a Schedule for dropdown, and a Schedule assignment button.


| Field | Type | Required | Notes |
|---|---|---|---|
| Schedule for | Dropdown | Yes | Active counsellors under the same SM, leaver excluded, and anyone who has entered their own last working day excluded. Each shown as "Name · TL name · [N] existing open leads". |


| Element | Copy / Behaviour |
|---|---|
| Schedule assignment | Disabled until a counsellor is picked. Opens the confirm. |
| Confirm | "Schedule assignment?" with Schedule for: [name] · Leads: [N] of [leaver]'s · Last working day · Leads move: [date], 8 PM. Note: "Nothing moves now. [Name] becomes the owner at [date], 8 PM. You can change this any time before then." Back / Schedule. |
| On Schedule | Rows update, progress updates, selection clears, toast "[N] leads scheduled for [Name]". Scheduling rows that already have a counsellor replaces the old choice. Every change saves immediately. |


| State | What shows |
|---|---|
| Leaver has 0 leads | "No leads to schedule." |
| Filters match nothing | "No leads match these filters." |
| Schedule fails | Red toast: "Couldn't schedule. Try again." Rows unchanged; nothing partial is saved. |
| Transferred | Read only, banner "Transfer complete." |


## New Counsellor: Tasks & Performance


### 4. Connect with Reassigned Leads (inside Boost STI / Boost Deposit)

- No new card. Tasks show inside the existing Boost STI and Boost Deposit pipelines, mixed with the counsellor's own Boost students.
- The Boost STI and Boost Deposit cards show a small "[N] reassigned" label. View Pipeline opens the existing drawer, which adds filter chips "All [N]" and "Reassigned [N]".
- Each reassigned student's card has the bucket badge plus a "Reassigned from [leaver]" badge, the How to close box (a "Connect with Reassigned Lead" chip and the guidance below), and View student / View Task.
- TL and SM versions cover the whole team; each card adds "· Assigned to [counsellor name]".

#### 4.1 Task Logic


| Cohort | Task Creation Logic | Task Closure Logic | Guidance shown on the card |
|---|---|---|---|
| Boost STI | Created at the 8 PM transfer for every reassigned lead whose stage is in the STI group. | A meeting is booked and joined by the new counsellor, or a call connects with the student — whichever comes first. | "Book and join a meeting with [student], or connect a call with them. Always log the interaction and update the follow-up date. Use 100ms, Jerry or the Leap Group Chat for all communication with the student." |
| Boost Deposit | Created at the 8 PM transfer for every reassigned lead whose stage is in the Deposit Done group. | Same as Boost STI. | Same as Boost STI. |

- Leads in any other group (Pre STI / Open App, Visa, both Drop Off groups) get no task.
- An unanswered call, or a meeting booked but not joined, leaves the task open.
- Created at the transfer, not when the TL schedules — ownership only moves at 8 PM.
- No Leap group chat task is created in the CRM (see Leap Group Chat).

## Automatic Transfer (8 PM, 7 Days Before LWD)


Runs daily at 8 PM for every Approved · TL assigning request whose transfer date is today. Transfer date = last working day minus 7 days. If that is earlier than the day after SM approval (short notice), the transfer date is the day after approval. The 7 days is a config value.


| Step | What happens | Rules |
|---|---|---|
| 1. Split unscheduled leads | Every lead with no assignment is given a counsellor. | Leaver is a Counsellor with a TL → pool = active counsellors in the TL's team, leaver excluded.<br>Pool empty, leaver is a TL, or leaver has no TL → pool = active counsellors under the same SM.<br>Round-robin by Lead ID, starting with the counsellor with the fewest open leads.<br>Pool still empty → do not transfer, bell to SM, retry at 8 PM next day. |
| 2. Move ownership | Owner changes to the scheduled counsellor. | Open tasks and follow-ups move. Past calls and notes stay on the lead. Old owner recorded as "Previous counsellor". |
| 3. Queue messages | One email and one WhatsApp per lead. | No email or no phone on the lead → that channel marked Failed (no retry). |
| 4. Create tasks | Connect with Reassigned Leads task for Boost STI / Boost Deposit leads. | See Section 4. |
| 5. Bell new counsellors | One bell per receiving counsellor. | "You received [N] leads from [Leaver name]." |
| 6. Leap group chat | WhatsApp notification to each new counsellor to join each student's Leap group chat. | Outside the CRM. See Leap Group Chat. |
| 7. Update status | Request status → Leads transferred. Bell to the leaver. | The leaver stays active until the last working day. |


### Last Working Day Job

- Runs on the last working day for every Leads transferred request.
- Removes the old counsellor from every student's Leap group chat, then deactivates their CRM login.
- Status → Deactivated. The record drops off the SM's requests table.

## Student Communication


| Channel | From | To | When |
|---|---|---|---|
| Email | Leaver's email account, through the CRM's existing email connection. Reply-To: new counsellor. CC: new counsellor, TL/POD, SM, debasish.sahoo@leapfinance.com. | Student's email on the lead | At the transfer |
| WhatsApp | Company WhatsApp Business number, one Meta-approved template | Student's phone on the lead | At the transfer |
| Leap group chat | LS number, Meta template built by Swapnil | Student's Leap group chat | When the new counsellor joins the group |

- Every student gets messages, including leads in closed-out stages (Dead Lead, Lead Drop Off, Admit Declined, and so on).
- Wording is fixed in code; there is no template editor. Templates are not shown anywhere in the CRM UI — no preview and no editor.
- No TL: if the new counsellor has no TL, the email drops the Team Lead line and WhatsApp uses its no-TL template. A blank TL is never sent.

#### Email Template (draft for build, not shown in UI)


Subject: Your new Leap counsellor: {{new_counsellor_name}}


Hi {{student_first_name}},


I'm moving on from Leap, and my last day is {{last_working_day}}. It's been great working with you.


From now on, {{new_counsellor_name}} will be your counsellor and will take things forward from where we left off.


Your new counsellor: {{new_counsellor_name}} · {{new_counsellor_phone}} · {{new_counsellor_email}}


Team Lead: {{tl_name}} · {{tl_phone}} · {{tl_email}}


Senior Manager: {{sm_name}} · {{sm_phone}} · {{sm_email}}


Just reply to this email to reach {{new_counsellor_name}} directly.


All the best,


{{leaver_name}}


#### WhatsApp Template (built in Meta by Swapnil, not shown in UI)


Hi {{1}}, your Leap counsellor {{2}} has moved on. Your new counsellor is {{3}} ({{4}}). You can also reach Team Lead {{5}} ({{6}}) or Senior Manager {{7}} ({{8}}). Reply here anytime.


| Variable | Filled with |
|---|---|
| {{1}} | Student first name |
| {{2}} | Leaver name |
| {{3}} | New counsellor name |
| {{4}} | New counsellor phone |
| {{5}} | TL name |
| {{6}} | TL phone |
| {{7}} | SM name |
| {{8}} | SM phone |

- No-TL version, used when the new counsellor has no TL: "Hi {{1}}, your Leap counsellor {{2}} has moved on. Your new counsellor is {{3}} ({{4}}). You can also reach Senior Manager {{5}} ({{6}}). Reply here anytime."

### Leap Group Chat (outside the CRM)

- Nothing about the Leap group chat is built in the CRM.
- After the 8 PM transfer, each new counsellor gets a WhatsApp notification for every student group: "Join this group". Once they join, they are in the group.
- When the new counsellor joins, the LGC message is posted in the group from the LS number (Meta template built by Swapnil).
- The old counsellor stays in the groups until their last working day, then is removed.
- If the student hasn't joined their LGC, they get a WhatsApp DM asking them to join — logic to be discussed with Swarnim.

| Entity | Fields in the event | Used in the LGC message |
|---|---|---|
| Student | {{student_name}} | Name |
| Old counsellor | {{old_counsellor_id}}, {{old_counsellor_email}}, {{old_counsellor_name}}, {{old_counsellor_lwd}} | Name, LWD |
| New counsellor | {{new_counsellor_id}}, {{new_counsellor_email}}, {{new_counsellor_name}} | Name, email |
| SM | {{sm_id}}, {{sm_email}}, {{sm_name}} | Name, email |


### Delivery Tracking & Retries


| Channel | Statuses tracked per student |
|---|---|
| Email | Pending, Sent, Failed |
| WhatsApp | Pending, Sent, Failed |
| Leap group chat | Tracked by the LGC system, not the CRM |

- Provider returns an error → retry 20, 40 and 60 minutes after the first attempt. Any attempt succeeds → Sent.
- 3rd retry fails → Failed with the provider's error text; counted on the request detail page (no per-student list).
- Missing email or phone → Failed straight away, no retry. The other channel still sends.

## Notifications (Bell Icon)


All staff alerts are CRM bell notifications — no email or WhatsApp to staff. Bells open the right place directly.


| Who | When | Message (opens) |
|---|---|---|
| SM | Last working day submitted | "[Name] submitted their last working day: [date]." (opens Last Working Day Approval) |
| TL | Request approved | "Schedule [Name]'s [N] leads before [date], 8 PM." (opens Schedule Reassigned Leads at that counsellor) |
| TL | A scheduled counsellor is no longer active | "[N] leads scheduled for [Name] are unscheduled again because they are no longer active" |
| Leaver | Request approved | "Your last working day ([date]) was approved. Your leads transfer on [date], 8 PM." |
| Leaver | Rejected / reopened | Rejected with the SM's reason / reopened with the reason and "You can pick a new date and apply again." |
| Leaver + TL | Request cancelled | "Your last working day request was cancelled by [SM]." / scheduled assignments cleared |
| Leaver | Transfer done | "Your [N] leads moved to new counsellors. You stay in your students' Leap group chats until [date]." |
| New counsellor | Transfer done | "You received [N] leads from [Leaver name]." |
| SM | No one available to receive leads | "No active counsellors to receive [Name]'s leads" |


## Request Status & Rules


#### Status Changes


| From | To | Triggered by | What also happens |
|---|---|---|---|
| — | Pending SM approval | Counsellor clicks Yes, submit | Bell to SM |
| Pending SM approval | Approved · TL assigning | SM confirms approval (or changes the date and approves) | Approval date recorded; leaver removed from allocation; bell to TL and leaver |
| Pending SM approval | Rejected | SM rejects with a reason | Bell to leaver |
| Rejected | Pending SM approval (new request) | SM reopens, then the counsellor applies again | Only the new request and its History are shown |
| Approved · TL assigning | Cancelled | SM cancels before the transfer starts | Assignments cleared; leaver back in allocation; bell to leaver and TL |
| Approved · TL assigning | Leads transferred | Transfer job (8 PM, 7 days before LWD) | See Automatic Transfer |
| Leads transferred | Deactivated | Last working day job | Removed from Leap group chats; login deactivated; row hidden from the SM list |


Rejected and Cancelled are final for that request; Deactivated is final. Statuses never go backwards.


#### Other Rules


| Rule | If | Then |
|---|---|---|
| Stop new leads | Request becomes Approved · TL assigning | Leaver excluded from lead allocation. |
| Stop new leads | Request becomes Cancelled | Leaver included in allocation again. |
| Who can receive leads | Counsellor is active, under the same SM, not the leaver, and has no last working day request of their own | Appears in Schedule for. Otherwise does not. |
| Scheduled counsellor becomes inactive before transfer | Deactivated, or their own last working day is approved | Their scheduled leads go back to Unscheduled, bell to the TL, entry in History. |
| Editing assignments | Status is Approved · TL assigning | TL or SM can schedule and reschedule freely. |
| Editing assignments | Any other status | Read only. |
| Same lead scheduled at the same time | TL and SM both save | Last save wins. Both recorded in History. |
| Lead edited by leaver after approval | Stage, Servicing type or Program changes | Allowed. The transfer uses the lead's state at 8 PM. |
| Overlap | Between transfer and last working day | The old counsellor stays active and in the Leap group chats; they no longer own the leads. |


## Role-Based Access


| Role | Sees | Can do |
|---|---|---|
| Counsellor | Last Working Day card on View Profile (own request only); lead list | Submit one request; view the tracker; apply again after a reopen |
| TL | Own Last Working Day card; Schedule Reassigned Leads tile and modal for their team; request detail and History | Schedule and reschedule leads for their team's leavers |
| SM | Last Working Day Approval and Schedule Reassigned Leads tiles for everyone under them; request detail | Approve, change last working day, reject, reopen, cancel, schedule |
| New counsellor | Bell alert; reassigned students in Boost STI / Boost Deposit | Work the Connect tasks |


## Admin & Settings


No admin screens in v0. Message wording is fixed in code. Transfer time (8 PM), the overlap (7 days) and retry timings are set in config by a developer. Stage, Servicing type and Program values are read from the CRM's existing lead fields — a new Bofu Status value appears in the Stage filter automatically.


## Stage Groups Reference


The 6 BOFU groups used on the summary boxes and the Group filter, confirmed by the team that owns the Bofu Status funnel. Groups 1 to 4 follow funnel order. Groups 5 and 6 sit outside the funnel — a lead can drop off at any point.


| Group | Bofu Status stages | Notes |
|---|---|---|
| 1. Pre STI / Open App | LS_PAYMENT_DONE, LS_COLLEGE_SHORTLISTED, LS_COLLEGE_FINALIZED, LS_APPLICATION_PROCESS_STARTED, LS_APPLICATION_PROCESS_ON_HOLD, LS_APPLICATION_IN_PROCESS, LS_APPLICATION_SUBMITTED_TO_AGGREGATOR, LS_APPLICATION_ON_HOLD_BY_AGGREGATOR, LS_APPLICATION_REJECTED_BY_AGGREGATOR | Starts at Payment Done — the first stage a counsellor is assigned a lead. |
| 2. STI | LS_APPLICATION_SUBMITTED_TO_INSTITUTE, LS_APPLICATION_ON_HOLD_BY_INSTITUTE, LS_ADMIT_REJECTED_BY_INSTITUTE, LS_CONDITIONAL_ADMIT_RECEIVED, LS_UNCONDITIONAL_ADMIT_RECEIVED, LS_ADMIT_RECEIVED, LS_ADMIT_ACCEPTED, LS_ADMIT_DECLINED, LS_OFFER_REVOKED_BY_UNIVERSITY | Every outcome after reaching the institute. Maps to the CRM's Boost STI bucket. |
| 3. Deposit Done | LS_DEPOSIT_PAID, LS_TUITION_FEE_PAID | Maps to the CRM's Boost Deposit bucket. |
| 4. Visa | LS_VISA_FILING_STARTED, LS_VISA_WIP, LS_VISA_APPLIED, LS_VISA_PROCESS_ON_HOLD, LS_VISA_GRANTED, LS_VISA_REJECTED, LS_VISA_DROPPED | Every visa status, any outcome. |
| 5. Pre ISL Drop Off | LS_LEAD_DROP_OFF, LS_DEAD_LEAD | Split from group 6 by lead history (table below). |
| 6. Post ISL Drop Off | LS_LEAD_DROP_OFF, LS_DEAD_LEAD | Split from group 5 by lead history (table below). |
| Other (no group) | LS_LEAD_CAPTURED, LS_WEBINAR_SCHEDULED, LS_WEBINAR_ATTENDED, LS_WEBINAR_CANCELLED, LS_COUNSELING_CALL_DONE, LS_AGENT_CHANGE_CALL_SCHEDULED, LS_USER_ACQUIRED, LS_LEAD_CLOSED_AFTER_SALES_CALL | Should not appear on a counsellor's book. If one does, it shows in the table and Stage filter, matches "Other" in the Group filter, and is left out of the 6 group totals. |


#### Pre ISL vs Post ISL Drop Off


For a lead whose current stage is Dead Lead or Lead Drop Off, read its last non-drop status from calculated_lead_status_change_log. The current stage code alone cannot answer this.


| Last non-drop status | Group |
|---|---|
| LS_PAYMENT_DONE or earlier | Pre ISL Drop Off |
| LS_COLLEGE_SHORTLISTED or later | Post ISL Drop Off |
| No earlier status in the log | Pre ISL Drop Off |


## Field Values Reference


#### Lead Stage (CRM field: Bofu Status)


Every value this field holds today, from a CRM export. All are selectable in the Stage filter.


| Value stored in the CRM | Shown in the UI as |
|---|---|
| LS_LEAD_CAPTURED | Lead Captured |
| LS_WEBINAR_SCHEDULED | Webinar Scheduled |
| LS_WEBINAR_ATTENDED | Webinar Attended |
| LS_WEBINAR_CANCELLED | Webinar Cancelled |
| LS_COUNSELING_CALL_DONE | Counseling Call Done |
| LS_AGENT_CHANGE_CALL_SCHEDULED | Agent Change Call Scheduled |
| LS_PAYMENT_DONE | Payment Done |
| LS_COLLEGE_SHORTLISTED | College Shortlisted |
| LS_COLLEGE_FINALIZED | College Finalized |
| LS_APPLICATION_PROCESS_STARTED | Application Process Started |
| LS_APPLICATION_IN_PROCESS | Application In Process |
| LS_APPLICATION_PROCESS_ON_HOLD | Application Process On Hold |
| LS_APPLICATION_SUBMITTED_TO_AGGREGATOR | Application Submitted To Aggregator |
| LS_APPLICATION_ON_HOLD_BY_AGGREGATOR | Application On Hold By Aggregator |
| LS_APPLICATION_REJECTED_BY_AGGREGATOR | Application Rejected By Aggregator |
| LS_APPLICATION_SUBMITTED_TO_INSTITUTE | Application Submitted To Institute |
| LS_APPLICATION_ON_HOLD_BY_INSTITUTE | Application On Hold By Institute |
| LS_CONDITIONAL_ADMIT_RECEIVED | Conditional Admit Received |
| LS_UNCONDITIONAL_ADMIT_RECEIVED | Unconditional Admit Received |
| LS_ADMIT_RECEIVED | Admit Received |
| LS_ADMIT_ACCEPTED | Admit Accepted |
| LS_ADMIT_DECLINED | Admit Declined |
| LS_ADMIT_REJECTED_BY_INSTITUTE | Admit Rejected By Institute |
| LS_OFFER_REVOKED_BY_UNIVERSITY | Offer Revoked By University |
| LS_DEPOSIT_PAID | Deposit Paid |
| LS_TUITION_FEE_PAID | Tuition Fee Paid |
| LS_VISA_FILING_STARTED | Visa Filing Started |
| LS_VISA_WIP | Visa WIP |
| LS_VISA_APPLIED | Visa Applied |
| LS_VISA_PROCESS_ON_HOLD | Visa Process On Hold |
| LS_VISA_GRANTED | Visa Granted |
| LS_VISA_REJECTED | Visa Rejected |
| LS_VISA_DROPPED | Visa Dropped |
| LS_USER_ACQUIRED | User Acquired |
| LS_LEAD_CLOSED_AFTER_SALES_CALL | Lead Closed After Sales Call |
| LS_LEAD_DROP_OFF | Lead Drop Off |
| LS_DEAD_LEAD | Dead Lead |


A lead with a blank Bofu Status (498 such leads org-wide in the export) still shows in the table and can be assigned, but matches only "All stages". A value not in this list shows with its raw value and also matches only "All stages".


#### Servicing Type and Program


| Field | Value stored in the CRM | Shown in the UI as |
|---|---|---|
| Servicing type | FREE_SERVICE | Free Service |
| Servicing type | PAID_SERVICE | Paid Service |
| Program | MASTERS | Masters |
| Program | UNDER_GRADUATION | Under Graduation |


## Edge Cases


| Situation | What the CRM does |
|---|---|
| Counsellor opens View Profile again after submitting | Shows the tracker, not the form. A second request is not possible — unless the request was rejected and the SM reopened it. |
| Last working day is less than 7 days away (or an immediate exit) | Leads transfer at 8 PM the day after SM approval. The counsellor's preview, the confirm and the SM's Review all show a short-notice warning. |
| TL hasn't scheduled everything by the transfer | Leftover leads are split across the TL's team (Automatic Transfer, step 1). |
| TL is on leave and nobody schedules | SM can use the same modal. If still nothing is scheduled, every lead is split at the transfer. |
| TL scheduled some leads, then the SM opens the same leaver | SM sees the true progress ("[x] / [y] scheduled"). Same the other way round. |
| SM tries to set a last working day before tomorrow | Not possible — the picker starts from tomorrow. |
| Student has no email or no phone | That channel is marked Failed. The other channel still sends. |
| Email or WhatsApp provider is down at 8 PM | Retried 3 times over 1 hour, then marked Failed. |
| Leaver has 0 leads | Flow still works. Nothing transfers; the login is deactivated on the last working day. |
| TL resigns, or leaver has no TL | Same flow. The SM approves and the SM schedules; leftover leads are split across all active counsellors under the SM. |
| New counsellor has no TL | Email drops the Team Lead line; WhatsApp uses the no-TL template. Connect tasks roll up to the SM. |
| New counsellor doesn't join a student's Leap group chat | Handled by the LGC system (reminders), not the CRM. |
| New counsellor of a Boost STI / Deposit lead is deactivated before connecting | Task moves with the lead. When the lead is reassigned and transferred again, the task is created for the next owner. |
| Resignations already in progress at launch | Finished the old way. The tool is used for new resignations only. |
| CRM opened on a mobile browser | Cards stack to one column. Tables scroll sideways. All actions still work. |


## FAQs


What if the counsellor changes their mind after submitting?


They cannot withdraw it themselves. They speak to their SM, who can cancel any time before the transfer. All scheduled assignments are cleared and they start getting new leads again.


What if the SM rejects by mistake?


The SM clicks Reopen. The counsellor still sees the rejection reason, picks a new (or the same) last working day and applies again. The SM sees it as a new pending request.


Why do leads move 7 days before the last working day?


So the old and new counsellor overlap for a week. The old counsellor stays in the Leap group chats and can help with questions, and the counsellor can tell students the exact date their lead moves.


Why send the email from the leaver's account?


The email is a personal goodbye, so it comes from them. Reply-To is the new counsellor, so replies reach the right person. The leaver's login is still active for the overlap week; it is deactivated on the last working day.


Will messaging students in closed-out stages cause problems?


Messaging students who haven't engaged recently can lead to blocks or reports and lower the WhatsApp number's quality rating. Watch the rating in the first month. If it drops, limit messages to leads in active stages.


What about the Leap group chat?


It is handled outside the CRM. After the transfer, each new counsellor gets a WhatsApp "Join this group" notification for every student group; once they join, the LGC message is posted. There is no LGC task in the CRM.


## Not Included in v0


| Not included | Why |
|---|---|
| Leap group chat tasks or joining in the CRM | Handled on WhatsApp by the LGC system. |
| Template editor or preview for email or WhatsApp | Wording rarely changes, WhatsApp changes need Meta approval anyway, and Swapnil builds the templates in Meta. |
| Per-student message list | Clutters the UI. Delivery shows as counts on the request detail page. |
| Showing incoming leads to the receiving counsellor before the transfer | Risk of gossip about who is leaving. Pending decision. |
| Reassigning leads between active counsellors | Happens rarely, for 2–3 counsellors; communicated by POD or Counsellor Support. |
| CSV export of transfers | The CRM already shows each lead's owner and previous counsellor. |
| Rule-based reassignment (by capacity, stage, servicing type or program) | TLs schedule by hand. The only automatic rule is the even split of leftover leads. |
| Editing Stage, Servicing type or Program from this tool | Those fields belong to the rest of the CRM. |
| Email or WhatsApp alerts to staff | All staff alerts are bell notifications. |
| Counsellor withdrawal or resubmission without the SM | The SM controls cancel and reopen. Only the latest request is shown. |
| Backfilling resignations in progress at launch | These finish the old way. |
| HR or payroll integration | Resignation acceptance and notice period stay with HR. |


## How We'll Know It's Working

- Zero leads owned by deactivated counsellors — checked with one CRM query each week.
- The SM pod gets no export or reassignment requests for resignations after launch.
- Every student gets Email and WhatsApp as Sent at the transfer, 7 days before the last working day, with Failed rows limited to missing contact details.
- Leavers and TLs stop asking the SM "where is my resignation?" because the tracker answers it.
- Reassigned Boost STI / Boost Deposit students get a connected call or a joined meeting within the overlap week.
