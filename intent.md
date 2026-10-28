# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by PRD N-17 and is history only: nothing in this intent is approved by it.

## Problem

- **I-1** The fictional clinic receives avoidable calls from patients checking whether their results are ready. (PRD: "avoidable calls from patients checking whether results are ready")

## Proposed outcome

- **I-2** The patient is notified when a result is ready for their review in the existing authenticated portal. (PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal.")
- **I-3** "Ready" means a doctor has reviewed and released the result. A notification is eligible only after both. Lab upload alone is never enough, and an abnormal result does not change the gate. (PRD: "A doctor must review and release the result before any notification is eligible."; product owner: "'Ready' must not mean that the laboratory upload merely finished.")
- **I-4** The message only says that an update is available and directs the patient to the existing authenticated portal. (PRD)

### How success will be observed

- **I-5** Results-status calls, counted separately from clinical questions, are compared against a measured baseline. (PRD: "Measure reduction in results-status calls"; support lead: "count status-only calls separately from clinical questions; compare against a measured baseline.")
- **I-6** Delivery failures are tracked. (PRD: "Measure reduction in ... delivery failures.")
- **I-7** No baseline or numeric target is approved yet. Both will be agreed during discovery, so this intent sets no target. (PRD: "none is approved yet.")

## Affected users and systems

- **I-8** Patients: the only approved recipients of the notification.
- **I-9** Doctors: their review and release triggers notification eligibility.
- **I-10** The existing authenticated portal result route: where the patient reads the result. (PRD: "Use the existing portal result route."; engineering lead: "the existing portal path is adequate.")
- **I-11** The notification and delivery mechanism, including duplicate prevention and delivery-failure handling. The channel is still open (OPEN-1).
- **I-12** Clinic support staff: they receive the status calls and own the call counting. (Support lead)
- **I-13** Notification and operational logs: covered by the no-clinical-content rule (C-1).

## Out of scope

- **X-1** Portal redesign. (PRD scope)
- **X-2** Appointment scheduling. (PRD scope)
- **X-3** Clinical interpretation. (PRD scope)
- **X-4** Notifying third parties. (PRD scope) The referring doctor is not decided here. It is a separate open question (OPEN-2).
- **X-5** A new or rebuilt patient dashboard: a separate project. (Engineering lead)
- **X-6** Everything that only the superseded request asks for: sending results as soon as the lab uploads them, putting the test result in an SMS, copying the referring doctor, rebuilding the patient dashboard, and a 90% call reduction in one week. None of these is approved.

## Constraints

- **C-1** No diagnosis, test name, test result, clinical measurement or treatment detail goes in the message, the notification logs or the operational logs. (PRD; privacy owner: "no sensitive content in messages or operational logs.")
- **C-2** Doctor review and release must happen before any notification (I-3).
- **C-3** Access to the result is authenticated in the existing portal. No attachments and no public result link. (PRD)
- **C-4** No duplicate notifications for the same released result version. Delivery failures are handled without sending the patient duplicate messages. (PRD)
- **C-5** No recipient other than the patient is approved. Nobody else may be added without an owner's decision. (PRD; clinical owner)
- **C-6** Channel and consent are subject to the privacy owner's consent and delivery review. (PRD; privacy owner)

## Open questions and owners

- **OPEN-1** Channel (SMS, email or portal push). Owners: product owner and privacy owner. They decide after the consent and delivery review. (PRD; privacy owner: "Consent and the actual channel are still under review.")
- **OPEN-2** Whether the referring doctor is notified, and how that recipient is authorized. Owners: clinical owner and privacy owner. This is not approved by the PRD, and approval must not be inferred from the superseded request. (PRD; clinical owner)
- **OPEN-3** The baseline and the numeric target for the reduction in status calls. To be agreed during discovery, with no owner named in the sources. This is not recorded as a decision below.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
