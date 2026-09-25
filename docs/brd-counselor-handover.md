# Counselor Handover — Business Requirements (BRD)

**Owner:** Debasish Sahoo (DS)
**Date:** 25 Sep 2026
**Status:** Draft for review
**Companion document:** prd-counselor-handover.md (screens, fields, rules)

---

## 1. Background
When a counselor resigns, their leads and students have to be handed to other counselors. Today this is a manual process that runs through the SM pod. It depends on the pod's time and the SM's access, and students are informed by hand. We handle 3 to 5 resignations a month, and each leaver holds 200 to 500 leads. That is roughly 600 to 2,500 leads a month moved by hand.

## 2. Business Problem
| Today (as-is) | What goes wrong |
|---|---|
| Counselor is asked to export all their leads | Exports are late, incomplete or in different formats |
| File goes to the SM pod, who reassign leads by hand | Depends on pod availability and access. Slow at month end. |
| Follow-ups and tasks stay with the leaver | Follow-ups are missed after the counselor leaves |
| SM or pod emails students one by one | Some students are never told. Others are told late. |
| No single record of who got which lead | Hard to answer "who owns this student now?" or audit a handover |
| Leaver keeps getting new leads during notice | New students start with someone who is about to leave |

## 3. Business Objectives
1. **No orphaned leads.** Every lead of a leaver has an active owner by the end of their last working day.
2. **No dependency on the SM pod.** A resignation is handled end to end inside the CRM by the counselor, their TL and their SM.
3. **Every student is informed** on the last working day, with contact details for the new counselor, TL and SM.
4. **Full traceability.** Every step (submission, approval, date change, assignment, transfer, messages) is recorded.
5. **Continuity for the student.** Open tasks, follow-ups and history move with the lead, so the new counselor carries on where the last one stopped.

## 4. Scope

### In scope (v0)
- Resignation request raised by the counselor (or TL) inside the CRM
- SM approval, rejection, date change and cancellation
- Lead assignment by the leaver's TL (or SM), in bulk
- Automatic transfer at 8 PM on the last working day, including tasks, follow-ups and history
- Even split of any leads the TL did not assign
- Stopping new lead allocation to the leaver after approval
- Student email (from the leaver's account) and WhatsApp message (from the company number)
- CRM task for the new counselor to post an intro in the student's Leap group chat
- Delivery tracking with automatic retries
- Automatic deactivation of the leaver's CRM login after transfer
- Action history visible to SM and TL

### Out of scope (v0)
- Auto-joining the new counselor to the Leap group chat (planned for v1)
- Editing message templates without a developer
- Export of transfer lists
- Rule-based reassignment by capacity, program or performance
- HR or payroll integration and notice period rules
- Resignations already in progress on launch day, which finish the old way

## 5. People Involved
| Who | Role in this process |
|---|---|
| Counselor (leaver) | Raises the request with a last working day |
| TL | Assigns the leaver's leads to counselors |
| SM | Approves, rejects, changes the date, cancels. Can assign when the TL is away or when a TL resigns. Monitors message delivery. |
| New counselor | Receives leads, tasks and history. Posts an intro in the Leap group chat. |
| Students | Receive email and WhatsApp with new contact details |
| SM pod | No longer involved in resignations |
| DS | Business owner. CC'd on student emails. |
| Tech team | Builds inside the existing CRM |

## 6. To-be Process
1. Counselor opens Learning & Development in the CRM, opens the Resignation card, picks a last working day, reviews their lead counts and confirms.
2. SM is alerted and approves, rejects with a reason, or changes the date and approves.
3. On approval the leaver stops receiving new leads and the TL is alerted.
4. TL (or SM) assigns leads in bulk to counselors under the same SM. They can change assignments until the transfer.
5. At 8 PM on the last working day the CRM:
   - splits any unassigned leads evenly across the TL's team,
   - moves every lead with its tasks, follow-ups and history,
   - emails and WhatsApps every student,
   - creates a Leap group chat task for each new counselor,
   - alerts each new counselor,
   - deactivates the leaver's login.
6. SM sees sent and failed counts. Failed sends are retried 3 times within an hour, then listed for follow-up.

## 7. Business Requirements
| ID | Requirement | Priority |
|---|---|---|
| BR-01 | A counselor or TL can raise a resignation in the CRM with a last working day, without contacting anyone. | Must |
| BR-02 | A resignation must be approved by the SM before any lead moves. | Must |
| BR-03 | The SM can reject (with a reason), change the last working day, or cancel before the transfer. | Must |
| BR-04 | A leaver cannot withdraw their own request. Only the SM can cancel it. | Must |
| BR-05 | The leaver stops receiving new leads once the SM approves. | Must |
| BR-06 | The leaver's TL decides who receives each lead, choosing from counselors under the same SM. | Must |
| BR-07 | The SM can do the assignment when the TL is unavailable, and always does it when a TL resigns. | Must |
| BR-08 | Every lead the leaver ever owned (open, enrolled, lost, closed) moves on the last working day. | Must |
| BR-09 | No lead may be left with the leaver. Unassigned leads are split evenly across the TL's team. | Must |
| BR-10 | Open tasks, follow-ups and past call and note history move with the lead. | Must |
| BR-11 | Every moved student receives an email from the leaver's account naming the new counselor, TL and SM. Replies go to the new counselor. The new counselor, TL/POD, SM and DS are copied. | Must |
| BR-12 | Every moved student receives a WhatsApp message from the company number with the same contacts. | Must |
| BR-13 | Each new counselor gets a task to introduce themselves in the student's Leap group chat. | Must |
| BR-14 | Failed messages are retried automatically and any that still fail are visible to the SM. | Must |
| BR-15 | The leaver's CRM login is deactivated automatically after transfer. | Must |
| BR-16 | Every action on a resignation is recorded and visible to the SM and TL. | Must |
| BR-17 | The leaver, TL, SM and new counselors are alerted through the CRM bell at each step. | Must |
| BR-18 | The new counselor is added to the Leap group chat automatically. | v1 |

## 8. Success Measures
| Measure | Target | How to check |
|---|---|---|
| Leads owned by deactivated counselors | 0 | Weekly CRM query |
| Resignation requests reaching the SM pod | 0 after launch | Pod ticket count |
| Students messaged on the last working day | All with valid contact details | SM card: Sent vs Failed |
| Time spent by SM pod per resignation | 0 | Pod time log |

## 9. Assumptions
- All counselors, TLs and SMs already have CRM accounts with a correct reporting line (counselor → TL → SM).
- The CRM can send email on a counselor's behalf through a stored connection.
- The company WhatsApp Business number and provider can send an approved template message.
- Lead stages and programs already exist as fields in the CRM.
- HR continues to own the resignation itself and the notice period. This tool only handles leads and students.

## 10. Constraints
- It must be built inside the existing CRM, on the Learning & Development page, using existing roles and the bell.
- The WhatsApp template must be approved by Meta before launch.
- Message wording is fixed in v0.

## 11. Risks
| Risk | Impact | Mitigation |
|---|---|---|
| Messaging lost and closed students on WhatsApp | Blocks or reports lower the company number's quality rating | Watch the rating in the first month. Limit to open and enrolled if it drops. |
| Leaver's email connection not set up | Emails can't be sent from their account | Check the connection at SM approval. Build question 1 in the PRD. |
| TL doesn't assign in time | Leads split evenly, not matched to the right counselor | Bell alert at approval, and SM can assign |
| Wrong last working day | Leads move too early or too late | SM can change the date or cancel before 8 PM |
| High email volume to DS | Up to about 2,500 CC emails a month | Confirm CC or switch to a daily summary in v1 |

## 12. Dependencies
- CRM tech team capacity
- WhatsApp provider and Meta template approval
- Email sending connection per counselor
- Leap group chat API (v1 only)

## 13. Sign-off
| Name | Role | Decision | Date |
|---|---|---|---|
| Debasish Sahoo | Business owner | | |
| | SM representative | | |
| | CRM tech lead | | |
