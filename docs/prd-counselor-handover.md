# Lead Handover — PRD (revised flow)

[Clickable mockup (Vercel)](https://leap-lead-reassign.vercel.app/)

## Goal

When a counsellor resigns, every lead moves to a new counsellor, every student is introduced to their new counsellor, and nothing is exported or done by hand.

## Objective

- Zero leads left with a counsellor who has left.
- Every student has been introduced to their new counsellor, in their WhatsApp group, before the old counsellor's last working day (LWD).
- Reassignment happens in the CRM, by the SM or TL, with no dependency on the SM pod.

## What changed from the earlier version

| Before | Now |
|---|---|
| Leads were scheduled and moved by a job at 8 PM, 7 days before the LWD | **No schedule.** Every approved assignment moves its leads immediately. The T-7 overlap and the "Run transfer" job are gone |
| SM added an LWD in one step | **Two steps:** SM raises the request, then approves it. The lead list opens only after approval |
| Assignments could be changed until the transfer | **Irreversible.** "Are you sure? This action is irreversible." Reassigned leads are locked |
| Unassigned leads were split evenly at the transfer | **Not split.** If any lead is unassigned at 8 PM on the LWD, deactivation is blocked and the SM and TL are alerted |
| A DM went to every student at the transfer | Messages go out **when the new counsellor joins** the student's WhatsApp group, in the group |
| Old counsellor left the groups on the LWD | Old counsellor and old RM leave **when the new counsellor joins** |
| Tasks only for Boost STI / Boost Deposit leads | **One handover task per lead**, with a join sub-task for all leads and a call sub-task for post-STI, pre-visa-approved leads |

## Principles

- No lead is ever left without an owner.
- The SM, TL and new counsellor always know where a request stands without asking anyone.
- A reassignment is final. Mistakes are prevented by a clear confirm, not fixed afterwards.
- The old counsellor stays in the student's group until the new counsellor joins, so a student is never left without someone.

## Scope and Users

- **SM** — raises and approves the request, changes the LWD, reassigns leads (when the TL is away, the counsellor has no TL, or the TL has also left), and watches the handover.
- **TL** — reassigns the leaver's leads. Owns the 24-hour task.
- **POD leader** — does the TL's part when the counsellor has no TL.
- **New counsellor** — receives leads and one handover task per lead.
- **RM (Relationship Manager)** — every counsellor is mapped to an RM. When a student's counsellor changes, the RM changes with it, and the new RM is tasked to join the group.
- **Old counsellor** — has no control in this flow. Stays in the group until the new counsellor joins, and stays active until the LWD.
- **Students** — receive a WhatsApp message in their group and an email when the new counsellor joins.

Scale: ~260 counsellors exited in the last 3 months. After the cleanup, about 500 active leads per counsellor.

## End-to-End Flow

| # | Who | What happens | Status after |
|---|---|---|---|
| 1 | Counsellor | Applies to resign. Appears in the SM's list. | Applied |
| 2 | SM | Selects the counsellor, raises the request with the LWD. | Pending approval |
| 3 | SM | Approves. Fresh leads stop going to the counsellor. The TL gets a task. The lead list opens. | Assigning leads |
| 4 | TL or SM | Selects leads (one by one, select all, or by stage), picks a new counsellor, clicks **Approve Assignment**, confirms. Leads move immediately. Repeat until none are left. | Assigning leads (x / y) |
| 5 | CRM | Each new counsellor gets one handover task per lead. The RM is tasked to join the group. | — |
| 6 | New counsellor | Joins the student's WhatsApp group. Old counsellor and old RM are removed; the student gets the WhatsApp message and email. | — |
| 7 | New counsellor | Calls the student (post-STI, pre-visa-approved only). | — |
| 8 | CRM | The last lead is reassigned; the SM and TL are told. | Leads reassigned |
| 9 | CRM | 8 PM on the LWD: login deactivated, only if every lead is reassigned. | Completed |

Side paths: **Deleted** (before any lead has moved) and **Deactivation blocked** (LWD passed with unassigned leads).

## Requirements

### 1. SM — Counsellor Last Working Day

- The entry point is always visible to the SM: Tasks & Performance › Important Business Tasks › **Counsellor Last Working Day**.
- It lists every counsellor who has applied to resign, with TL/PL, LWD, lead count and status. In progress and Past are separate tabs.
- **Raise request** (step 1): pick the counsellor and the LWD (tomorrow or later). The SM sees the lead counts (Open App / Post STI / Post Deposit / Drop off) but **not** the lead list.
- **Approve** (step 2): a review then a red confirm. It states: fresh lead assignment stops; the TL gets a task; every approved assignment moves leads immediately and can't be undone; new counsellors and RMs get tasks; the login is deactivated at 8 PM on the LWD only if every lead is reassigned; and the 11 PM alert.
- **Change last working day:** from a calendar, at any time before the LWD, including after leads have moved. Leads already reassigned stay where they are. The deactivation and alert move to the new date.
- **Delete:** only while no lead has moved. Once a lead has moved it can't be undone, so delete is not offered.
- **+ Raise for another counsellor:** for an exit that didn't come through an application (for example a termination).
- A row opens whatever is next: raise, approve, reassign, or track the handover.
- No CSV export. Everything is done in the UI.

### 2. Reassigning leads (TL, POD leader, or SM)

- Reassign Leads tile in Important Business Tasks: the TL sees their team's leavers, the POD leader theirs, the SM everyone's. One accordion per leaver, one open at a time.
- **Columns:** name (with Lead ID), servicing type (Premium / Free), intake, country, UG / PG. *Stage is also shown, because assignment can be done stage by stage.*
- **Filters:** Stage and Group (server-side, over the whole book); Servicing type, UG / PG and Show (All / Unassigned / Assigned), which run locally on the leads already loaded.
- **Lazy load:** 500 leads per page, **Load next 500**. The list holds up to 2,000 leads; above that the screen says so and reassignment waits for the cleanup.
- **Select:** click rows, or the header checkbox (selects the unassigned leads that are loaded and match the filters). Reassigned rows are locked.
- **Assign:** pick a counsellor (active, under the same SM, with their existing open leads shown; not anyone who has applied or has their own request) and click **Approve Assignment**.
- **Confirm:** "Are you sure? This action is irreversible." It shows the new counsellor, the lead count, the pre-STI / post-STI / visa-stage split and the new RM.
- **On confirm:** the leads move immediately with open tasks and follow-ups. Past calls and notes stay on the lead. The old owner is recorded. The assignment is written to the counsellor assignment log (used for incentives and commission). The new counsellor gets a bell: "You received [N] leads from [Name]: [pre-STI, post-STI, visa-stage]".
- When the last lead is reassigned the status becomes **Leads reassigned**, and the SM and TL are told.
- **24-hour task:** the TL / PL should reassign everything within 24 hours of approval. Otherwise an email goes to their reporting manager and Debasish Sahoo.

### 3. New counsellor — handover tasks

One task per reassigned lead, with two sub-tasks.

| Sub-task | Applies to | Done when |
|---|---|---|
| ① Join the student's WhatsApp group | All leads | The new counsellor joins. If the student has no group, nothing is sent and there is nothing to join: the counsellor introduces themselves directly. |
| ② Call the student | Post-STI, pre-visa-approved leads only: STI group, Deposit Done, and Visa Filing Started / WIP / Applied / On Hold | A call connects (over 10 seconds), or 3 unanswered attempts (each ringing 30 seconds or more). |

- The call button stays off until the group is joined (or the student has no group).
- There is no way to close ② without a call attempt.
- **Fallback:** after the 3rd unanswered attempt the task closes and the student gets an email and an LGC notification.
- Pre-STI, visa-approved and dropped-off leads only need ①.
- **Where it lives:** a dedicated **Connect with Reassigned Leads** drawer, listing every reassigned lead in any stage. It opens with a summary of what arrived (pre-STI, post-STI, visa-stage, dropped off), has category filters, and shows each sub-task's state. It also appears in the bell and under **View assigned leads**.
- Call order: deposit done, STI done, visa in process.
- TL, POD leader and SM see the same tasks for their team. They can join a student's group to follow along; it doesn't change the counsellor's task.

### 4. When the new counsellor joins the group

- The **old counsellor** and the **old RM** are removed from the group.
- The **old TL** is removed too, **only if** the lead moved to a counsellor under a different TL. If the TL is unchanged, the TL stays.
- The student gets a **WhatsApp message** in the group. The copy depends on the stage: pre-application or post-application (see below).
- The student gets an **email**: CC old counsellor and new counsellor; BCC TL and SM.
- If the student has no WhatsApp group: no message is sent.
- If the new counsellor never joins: the old counsellor stays in the group as the fallback, and is still deactivated on the LWD.
- Overlap between old and new counsellor in the group is handled through the new-counsellor assignment table only. There is no fixed overlap window.

### 5. RM (Relationship Manager)

- Every counsellor is mapped to an RM. When a student's counsellor changes, the student's RM changes to the new counsellor's RM.
- Like the counsellor, the new RM gets a task to join the student's WhatsApp group.
- When the new counsellor joins the group, the **old RM is removed** with the old counsellor.
- The handover view shows each new counsellor's RM and how many RMs have joined.
- The RM-side task itself lives in the RM CRM and is not built in this mockup; only its status is tracked here.

### 6. Last working day (automatic)

- **11 PM on the day before the LWD:** if any new counsellor has joined none of their groups, an alert email goes to the SM. If the LWD is tomorrow and the request is approved today, the alert fires the same day.
- **8 PM on the LWD:** the old counsellor's login is deactivated **only if every lead has been reassigned**.
  - If leads are unassigned: deactivation is **blocked**, the account stays active, and the SM and TL are alerted. The block lifts, and the login deactivates, the moment the last lead is reassigned.
  - Where the new counsellor never joined, the old counsellor stays in those groups as the fallback.
  - The old counsellor can't log in after deactivation. Removing them from groups where they remain is handled separately by infra (escalation cases).

## Student Messages

Wording is fixed in code and not shown in the CRM. Templates are built in Meta by Swapnil. *Wording below marked draft is not confirmed.*

### WhatsApp message in the group — posted when the new counsellor joins

The event carries: student name; old counsellor (ID, email, name, LWD); new counsellor (ID, email, name); SM (ID, email, name).

Two versions, by stage. **Pre-application** = before the application starts (Payment Done, College Shortlisted, College Finalized). **Post-application** = Application Process Started onward.

> **Pre-application (draft):** Hi {{student_first_name}}, I'm {{new_counsellor_name}}, your new Leap counsellor, taking over from {{old_counsellor_name}}. I'll help you shortlist universities and prepare your applications from here. Message me here anytime.

> **Post-application (draft):** Hi {{student_first_name}}, I'm {{new_counsellor_name}}, your new Leap counsellor, taking over from {{old_counsellor_name}}. I have your application status and next steps and will continue from where you left off. Message me here anytime.

### Email — triggered when the new counsellor joins

From the leaver's email account. Reply-To: new counsellor. **CC:** old counsellor, new counsellor. **BCC:** TL, SM.

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

No TL: the email drops the Team Lead line.

### Delivery

- A failed email is retried 20, 40 and 60 minutes after the first attempt, then marked Failed. If the lead has no email, it is marked Failed straight away.
- A student with no WhatsApp group gets no WhatsApp message. The new counsellor introduces themselves directly.

## Notifications

| Who | When | Message |
|---|---|---|
| TL / POD | Request approved | "Task: reassign [Name]'s [N] leads before [date], 8 PM." |
| New counsellor | Each approved assignment | "You received [N] leads from [Name]: [pre-STI, post-STI, visa-stage]. Join each student's WhatsApp group." |
| New counsellor | SM or TL sends a reminder | "Reminder: join [N] groups for leads from [Name]." |
| SM, TL | All leads reassigned | "[Name]'s [N] leads are all reassigned. Track the new counsellors joining." |
| SM (email + bell) | 11 PM, LWD − 1 | "[Counsellor] has joined none of their groups from [Name]." |
| SM, TL | LWD 8 PM, leads unassigned | "[Name]'s last working day has passed with [N] leads unassigned. The login stays active." |
| SM, TL | LWD 8 PM, deactivated | "[Name]'s last working day is done: login deactivated." |
| Reporting manager + Debasish Sahoo | TL / PL misses the 24-hour deadline | Email: reassignment task overdue. |
| TL | LWD changed | "[Name]'s last working day changed to [date]." |

## Statuses

Applied › Pending approval (amber) › Assigning leads (blue, with x / y reassigned) › Leads reassigned (green) › Completed. Side paths: Deleted (grey) and **Deactivation blocked** (red). Statuses never go backwards.

## Stage Groups

| Group | Bofu Status stages |
|---|---|
| Pre STI / Open App | Payment Done, College Shortlisted, College Finalized, Application Process Started / On Hold / In Process, Submitted To / On Hold By / Rejected By Aggregator |
| STI (Boost STI) | Submitted To / On Hold By Institute, Admit Rejected By Institute, Conditional / Unconditional Admit, Admit Received / Accepted / Declined, Offer Revoked |
| Deposit Done (Boost Deposit) | Deposit Paid, Tuition Fee Paid |
| Visa | Visa Filing Started, WIP, Applied, On Hold, Granted, Rejected, Dropped |
| Pre / Post ISL Drop Off | Lead Drop Off, Dead Lead — split by the last non-drop stage in calculated_lead_status_change_log (*assumed boundary: dropped before College Shortlisted = Pre ISL, after = Post ISL; see open questions*) |

The new counsellor's summary uses four buckets: **pre-STI** (Pre STI / Open App), **post-STI** (STI + Deposit Done), **visa-stage** (Visa), **dropped off**.

## Edge Cases

- **LWD is tomorrow and approved today** — the 11 PM alert fires the same day.
- **Unassigned leads at 8 PM on the LWD** — deactivation is blocked and escalated; it completes when the last lead is reassigned.
- **New counsellor never joins** — the old counsellor stays in the group as fallback, and is deactivated on the LWD at 8 PM.
- **Student has no WhatsApp group** — no message is sent; nothing to join; the new counsellor introduces themselves directly.
- **Student doesn't pick up** — 3 attempts, then the task closes and an email and LGC notification go out.
- **Counsellor has no TL** — the POD leader gets the task; the SM can also reassign.
- **TL and counsellor both resign** — the SM reassigns the leads to another TL's counsellors. The RM is also tasked.
- **TL and SM assign at the same time** — a lead can only be assigned once; the second attempt finds it already moved.
- **A new counsellor leaves before joining** — their groups still show as not joined; the SM reassigns those leads again through a fresh request for them.
- **More than 2,000 leads** — the list can't load them all; clean up first.
- **Request deleted** — only before any lead has moved; the TL's task closes and fresh leads resume.
- **Termination or immediate relief** — the TL introduces the student to the new counsellor on a video call. *Documented only; no screen in this release.*
- **Deactivated counsellor** — can't log in. Removing them from any group that remains is handled by infra, not this flow.

## One-Time Lead Cleanup

Done once, before go-live, outside the UI.

- **Problem:** a counsellor can't be deactivated until all leads are reassigned. This has been enforced by the system for 3 years. As a result, ~260 counsellors who exited in the last 3 months had their leads moved onto remaining counsellors, and one counsellor now has 1,000+ leads.
- **Visa-approved leads:** null them out (no active servicing needed).
- **Invoicing-stage leads:** must keep counsellor attribution, for incentive tracking.
- **Attribution:** the counsellor assignment log table records which counsellor was assigned at each stage and date. Incentives and commission read from it.
- **Target:** about 500 active leads per counsellor before go-live. The 2,000-lead list limit makes this a prerequisite for the UI.
- **Owner and timing:** Swapnil to complete the cleanup within 5 days, then coordinate the reassignment.
- The deactivation rule (block until all leads are reassigned) stays. The difference is that it is now visible and escalated, instead of being worked around by bulk transfers.

## Not in this release

- Scheduled reassignment or a transfer date. It is removed.
- CSV export. All actions are done in the UI.
- Rule-based reassignment (by capacity, program or conversion).
- Template editor or preview for email and WhatsApp.
- The RM-side task screen (RM CRM).
- A video-call introduction flow for termination cases.
- Server-side filters other than Stage (Stage is server-side; the rest run on the loaded page). Stage filtering over the whole book is being handled separately.
- Reassigning leads between active counsellors outside a resignation.

## How We'll Know It's Working

- Zero leads owned by deactivated counsellors (weekly CRM check).
- No reassignment or export requests to the SM pod for resignations.
- Every approved assignment shows its new counsellor joined to the group within 24 hours.
- Fewer than 1 in 10 students reaches the LWD without their new counsellor in the group.
- Reassigned post-STI students get a connected call, or three attempts, within the first week.

## Open Questions

1. **Pre- and post-application boundary.** I set it at "Application Process Started" (before = Payment Done, College Shortlisted, College Finalized). Confirm, and supply the final WhatsApp copy for both versions.
2. **Fallback timing.** The email and LGC notification are sent once, after the 3rd unanswered attempt. Should they go out earlier, after the first unanswered attempt?
3. **TL and counsellor both resign.** The notes say the SM reassigns to another TL's counsellors. The mockup also routes to the POD leader when a counsellor has no TL. Should the POD leader be removed from that path when the TL has also left?
4. **Stage column.** Your column list is name, servicing type, intake, country, UG / PG. Stage is kept as an extra column because assignment is stage-wise. Confirm or remove.
5. **Servicing label.** "Premium" / "Free" is used in the list (the data field is PAID_SERVICE / FREE_SERVICE). Confirm.
6. **RM task.** Where does the RM see the join task, and does the RM CRM already support a per-student WhatsApp group task?
7. **Termination / immediate relief.** How is it marked on the request, and does the TL's video call need a task of its own?
8. **Above 2,000 leads.** Should the screen block, or allow reassigning the first 2,000 and ask for cleanup of the rest?
9. **Date change after leads have moved.** Allowed in this build. Confirm that is wanted.
10. **24-hour TL / PL deadline.** Kept from the previous flow. Confirm it still applies now that assignments are immediate.
11. **Stage filter on the server.** "Handled separately" — when, and does the Group filter (which is derived from stage) move with it?
12. **Pre / Post ISL boundary.** The drop-off split reads the status change log; confirm which last stage counts as Pre ISL and which as Post ISL.
13. **Testing delivery target.** To be moved (your next steps).
