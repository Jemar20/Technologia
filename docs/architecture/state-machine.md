# State Machine Diagram — Listing Status
 
**Scope:** Our main entity (the object with a status field); initial and final states; states named as conditions; every transition labelled with its event.

```mermaid
---
title: "State Machine Diagram: Listing status lifecycle"
---
stateDiagram-v2

    [*] --> AWAITING_VERIFICATION : Owner submits listing

    AWAITING_VERIFICATION --> VERIFIED : Verifier approves<br/>[checklist complete]

    AWAITING_VERIFICATION --> REJECTED : Verifier rejects<br/>[checklist failed, reason recorded]

    REJECTED --> AWAITING_VERIFICATION : Owner resubmits<br/>after fixing issues

    VERIFIED --> RESERVED : Verifier records reservation<br/>[confirmed offline]

    VERIFIED --> TAKEN : Verifier records room filled

    VERIFIED --> EXPIRED : No update for 14 days<br/>[timeout]

    RESERVED --> TAKEN : Verifier records move-in

    RESERVED --> VERIFIED : Reservation cancelled<br/>or falls through

    TAKEN --> [*]

    EXPIRED --> [*]
```

**Key:** `[*]` = initial/final pseudo-state; arrow labels are the triggering event, bracketed text is the guard condition.

**Traceability:** These six state names are exactly the six values of the `ListingStatus` enumeration in `class.md`, and they are the values stored in the `status` column of `erd.md`. `AWAITING_VERIFICATION → VERIFIED / REJECTED` is the same decision drawn as the guarded branch in `activity.md`. Owners and students have no accounts at MVP stage, so every status change after submission is recorded by the Team Verifier (see "Update Listing Availability" in `use-cases.md`).
