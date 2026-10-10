# Lead Handover — PRD

[Clickable mockup (Vercel)](https://leap-lead-reassign.vercel.app/)

## Goal

A self-serve handover flow in the Counsellor CRM: when a counsellor resigns, every lead moves to a new counsellor, every student is told who their new counsellor is, and nothing is done by hand.

## Objective

Zero leads left with a counsellor who has left, and every student told who their new counsellor is 7 days before the old counsellor leaves.

## Principles

- No lead is ever left without an owner.
- The leaver, TL and SM always know where the request stands without asking anyone.
- The TL stays in control of who gets which lead.
- The old and new counsellor overlap for at least 7 days.

## Scope and Users

- **Counsellor (leaver)** — enters their last working day (LWD). This flow is for counsellors only; TLs don't own leads, so they don't use it.
- **SM** — approves, changes the date, rejects, reopens or cancels. Can also schedule leads if the TL is away or the counsellor has no TL.
- **TL** — schedules which counsellor gets each lead.
- **New counsellor** — receives leads, follow-up tasks and a WhatsApp prompt to join each student's Leap group chat.
- **Students** — get an email and a WhatsApp at the transfer, and the Leap group chat message once the new counsellor joins.

Scale: 3–5 resignations a month, 200–500 leads each (600–2,500 leads a month).

## End-to-End Flow

| # | Who | What happens | Status after |
|---|---|---|---|
| 1 | Counsellor | Avatar › View Profile › Last Working Day › picks a date › confirms. | Pending SM approval |
| 2 | SM | Tasks & Performance › Last Working Day Approval › reviews › approves, changes the date, or rejects. | Approved · TL assigning |
| 3 | TL (or SM) | Tasks & Performance › Schedule Reassigned Leads › picks who gets each lead. | Approved · TL assigning |
| 4 | CRM | 8 PM, 7 days before LWD: moves every lead, messages every student, creates tasks, prompts new counsellors to join the Leap group chats. | Leads transferred |
| 5 | CRM | On the LWD: removes the old counsellor from the Leap group chats and deactivates their login. | Deactivated |

## Requirements

### 1. Counsellor — Last Working Day (View Profile)

- Lives on the View Profile page (avatar menu, top right), below the profile card. Card title "Last Working Day". The Learning & Development tab is unchanged.
- The counsellor only enters the LWD (tomorrow or later); everything else is automatic.
- Shows the total leads (e.g. 325) with the stage-group breakdown in small text. Clicking it opens the actual leads, read only, grouped by stage group and searchable.
- As soon as a date is picked, it shows the transfer date: 8 PM, 7 days before the LWD. If the LWD is less than 7 days away, it warns that leads transfer at 8 PM the day after SM approval.
- Submit opens a red confirm: "Submit your last working day? This can't be undone." It shows the counsellor's name, LWD, total leads and "[TL] (your TL) will transfer your leads by [date], 8 PM."
- After submitting, the card shows "Your leads transfer on [date], 8 PM" (marked expected until approved) and a tracker with dates: LWD submitted › SM approval › TL schedules leads › Leads transfer › Last working day. The counsellor can share the transfer date with students.
- No withdrawing — only the SM can cancel. If rejected, the counsellor sees the reason; once the SM reopens, they can pick a new date and apply again, and the reason stays visible. Only the latest request and its history are shown.
- After the transfer, the counsellor stays active and in the Leap group chats until the LWD to help the new counsellors.

### 2. SM — Last Working Day Approval (Tasks & Performance)

- A "Last Working Day Approval" tile in the existing Important Business Tasks grid (TL & POD rollup), with the pending count and load label like the other tiles.
- Clicking it opens a full-page modal. When several requests are pending, their names show as pills on top.
- Table: Counsellor name · TL name · Last working day · Leads · Status (with approval date) · Take action. Sorted Pending (nearest LWD first), Rejected, Approved, Transferred. A row drops off once that login is deactivated.
- Review opens a modal on top: counsellor, TL, LWD, leads (with a link to view them) and scheduled transfer time. Actions:

- **Approve** — a confirm step lists what happens next: new leads stop, the TL gets a task to schedule the leads, the transfer time, and that the login is deactivated on the LWD.
- **Change last working day** — new date, then the same confirm.
- **Reject** — reason required.
- On approval, new leads stop going to the counsellor, and the TL gets a bell and a task.
- The SM can Reopen a rejected request, and Cancel an approved one any time before the transfer (scheduled assignments are cleared and the counsellor gets new leads again).
- The View action on a transferred request shows who got the leads (by counsellor and stage group), email/WhatsApp delivery counts, and the history.

### 3. TL — Schedule Reassigned Leads (Tasks & Performance)

- A "Schedule Reassigned Leads" tile in Important Business Tasks, for the TL (own team) and the SM (all teams).
- Opens a full-page modal with one accordion per leaving counsellor, showing the LWD, the transfer time and "x / y scheduled". One opens at a time.
- Inside: every lead the counsellor owns (all stages), with filters for Stage, Group, Servicing type, Program and Show (All / Unscheduled / Scheduled). Stage and Group have Select all, and only one filter list opens at a time.
- Click rows to select (the whole row is clickable). A red Clear selection appears when rows are selected.
- Pick a counsellor (active, same SM, shown with their existing open leads), then Schedule assignment. A confirm states that nothing moves until the transfer time.
- Any assignment can be changed until the transfer. Anything left unscheduled is split evenly across the TL's team at the transfer.

### 4. Transfer — 8 PM, 7 Days Before LWD (automatic)

- Transfer date = LWD minus 7 days. If that's earlier than the day after SM approval (short notice), it runs at 8 PM the day after approval. The 7 days is a config value.
- Split any unscheduled leads evenly (round-robin, fewest open leads first) across the TL's team; if there's no TL, across everyone under the SM.
- Move every lead with its open tasks and follow-ups. Past calls and notes stay on the lead. The old owner is recorded as "Previous counsellor".
- Send every student an email and a WhatsApp, including leads in closed-out stages.
- Create Connect with Reassigned Leads tasks for Boost STI and Boost Deposit leads, and a bell for each new counsellor ("You received [N] leads from [Name]").
- Prompt each new counsellor on WhatsApp to join each student's Leap group chat (outside the CRM).
- The old counsellor stays active until the LWD.

### 5. Last Working Day (automatic)

- On the LWD, the old counsellor is removed from every student's Leap group chat and their CRM login is deactivated. The record drops off the SM's list.

### 6. New Counsellor — Connect with Reassigned Leads

- No new card. Tasks appear inside the existing Boost STI and Boost Deposit pipelines, with a "Reassigned from [leaver]" badge, a Reassigned filter in the drawer, and a "[N] reassigned" count on the card.
- Created at the transfer for leads in the STI group (Boost STI) or Deposit Done group (Boost Deposit). Other groups get no task.
- Closes when a meeting is booked and joined, or a call connects — whichever comes first.
- Task guidance: "Book and join a meeting with [student], or connect a call with them. Always log the interaction and update the follow-up date. Use 100ms, Jerry or the Leap Group Chat for all communication with the student."
- The same tasks roll up into the TL's view, or the SM's if the new counsellor has no TL.

### 7. Leap Group Chat (outside the CRM)

- Nothing about the Leap group chat is built in the CRM.
- After the transfer, each new counsellor gets a WhatsApp notification per student group: "Join this group."
- When they join, the LGC message is posted in the group from the LS number (Meta template, built by Swapnil).
- If the student hasn't joined their group, they get a WhatsApp DM asking them to join (logic with Swarnim).

## Student Messages

Sent at the transfer. Wording is fixed in code and not shown in the CRM (no preview, no editor). Templates are built in Meta by Swapnil.

### Email

From the leaver's email account. Reply-To: new counsellor. CC: new counsellor, TL/POD, SM, debasish.sahoo@leapfinance.com.

> **Subject:** Your new Leap counsellor: {{new_counsellor_name}}
> Hi {{student_first_name}},
> I'm moving on from Leap, and my last day is {{last_working_day}}. It's been great working with you.
> From now on, {{new_counsellor_name}} will be your counsellor and will take things forward from where we left off.
> Your new counsellor: {{new_counsellor_name}} · {{new_counsellor_phone}} · {{new_counsellor_email}}  
> Team Lead: {{tl_name}} · {{tl_phone}} · {{tl_email}}  
> Senior Manager: {{sm_name}} · {{sm_phone}} · {{sm_email}}
> Just reply to this email to reach {{new_counsellor_name}} directly.
> All the best,  
> {{leaver_name}}

### WhatsApp (company number)

> Hi {{1}}, your Leap counsellor {{2}} has moved on. Your new counsellor is {{3}} ({{4}}). You can also reach Team Lead {{5}} ({{6}}) or Senior Manager {{7}} ({{8}}). Reply here anytime.

{{1}} student first name · {{2}} leaver name · {{3}} new counsellor name · {{4}} new counsellor phone · {{5}} TL name · {{6}} TL phone · {{7}} SM name · {{8}} SM phone.

No TL: the email drops the Team Lead line, and WhatsApp uses this version (with {{5}} SM name and {{6}} SM phone):

> Hi {{1}}, your Leap counsellor {{2}} has moved on. Your new counsellor is {{3}} ({{4}}). You can also reach Senior Manager {{5}} ({{6}}). Reply here anytime.

### Leap Group Chat message

Posted when the new counsellor joins the group. The event carries:

| Entity | Fields |
|---|---|
| Student | Name |
| Old counsellor | ID, email, name, LWD |
| New counsellor | ID, email, name |
| SM | ID, email, name |

### Delivery

- A failed email or WhatsApp is retried 20, 40 and 60 minutes after the first attempt, then marked Failed. The counts show on the request detail page.
- If the lead has no email or no phone, that channel is marked Failed straight away; the other channel still sends.

## Notifications (CRM bell only)

| Who | When | Message |
|---|---|---|
| SM | LWD submitted | "[Name] submitted their last working day: [date]." Opens the approval modal. |
| TL | Approved | "Schedule [Name]'s [N] leads before [date], 8 PM." Opens the scheduling modal. |
| Counsellor | Approved / rejected / reopened / cancelled | Approval with the transfer date; the reason if rejected or reopened; who cancelled. |
| Counsellor | Transfer done | "Your [N] leads moved to new counsellors. You stay in your students' Leap group chats until [LWD]." |
| New counsellor | Transfer done | "You received [N] leads from [Name]." |
| TL | Scheduled counsellor becomes inactive | "[N] leads scheduled for [Name] are unscheduled again." |

## Statuses

Pending SM approval (amber) › Approved · TL assigning (blue) › Leads transferred (green) › Deactivated (hidden). Side paths: Rejected (red), which can be reopened as a new request, and Cancelled (grey), only before the transfer. Statuses never go backwards.

## Stage Groups

| Group | Bofu Status stages |
|---|---|
| Pre STI / Open App | Payment Done, College Shortlisted, College Finalized, Application Process Started / On Hold / In Process, Submitted To / On Hold By / Rejected By Aggregator |
| STI (Boost STI) | Submitted To / On Hold By Institute, Admit Rejected By Institute, Conditional / Unconditional Admit, Admit Received / Accepted / Declined, Offer Revoked |
| Deposit Done (Boost Deposit) | Deposit Paid, Tuition Fee Paid |
| Visa | Visa Filing Started, WIP, Applied, On Hold, Granted, Rejected, Dropped |
| Pre / Post ISL Drop Off | Lead Drop Off, Dead Lead — split by the last non-drop status in calculated_lead_status_change_log (Payment Done or earlier = Pre ISL; College Shortlisted or later = Post ISL) |

## Edge Cases

- **LWD less than 7 days away (or an immediate exit)** — leads transfer at 8 PM the day after approval. The counsellor, the confirm and the SM's review all show a short-notice warning.
- **TL hasn't scheduled everything** — leftover leads are split evenly across the TL's team at the transfer.
- **Counsellor has no TL** — the SM approves and schedules. Leftover leads are split across everyone under the SM.
- **Scheduled counsellor leaves before the transfer** — those leads go back to Unscheduled, and the TL gets a bell.
- **Anyone with their own approved LWD** — can't be picked to receive leads.
- **TL and SM schedule at the same time** — the last save wins; both are recorded in history.
- **No one available to receive leads** — the transfer waits, the SM gets a bell, and it retries at 8 PM the next day.
- **Resignations already in progress at launch** — finished the old way.

## Not in v0

- Leap group chat tasks in the CRM (handled on WhatsApp)
- Template editor or preview for email or WhatsApp
- Per-student message list in the CRM
- Showing incoming leads to the receiving counsellor before the transfer
- Reassigning leads between active counsellors (rare; handled by POD or Counsellor Support)
- CSV export, rule-based reassignment, and HR or payroll integration

## How We'll Know It's Working

- Zero leads owned by deactivated counsellors (weekly CRM check).
- No export or reassignment requests to the SM pod for resignations.
- Almost every student shows Email and WhatsApp as Sent at the transfer.
- Reassigned Boost STI / Boost Deposit students get a connected call or a joined meeting within the overlap week.
