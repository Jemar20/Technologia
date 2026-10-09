# Use Case Diagram — Boarding House Listing Verification

**Scope:** Every actor from the context diagram; 5–12 goal-level use cases named verb + object, taken from our MVP feature list. 

```mermaid
---
title: "Use Case Diagram: Boarding House Listing Verification System"
---
flowchart LR
    actorStudent["Student Seeker"]
    actorOwner["Boarding House Owner / Agent"]
    actorVerifier["Team Verifier"]

    subgraph SYS["Boarding House Listing Verification System"]
        UC1(["Browse Verified Listings"])
        UC2(["Filter Listings by Price and Location"])
        UC3(["View Listing Details"])
        UC4(["Inquire About a Listing"])

        UC5(["Submit a Listing"])

        UC6(["Verify a Listing"])
        UC7(["Reject a Listing"])
        UC8(["Update Listing Availability"])
        UC9(["Log In as Verifier"])
    end

    %% Student Seeker
    actorStudent --> UC1
    actorStudent --> UC2
    actorStudent --> UC3
    actorStudent --> UC4

    %% Boarding House Owner/Agent
    actorOwner --> UC5

    %% Team Verifier
    actorVerifier --> UC6
    actorVerifier --> UC7
    actorVerifier --> UC8
    actorVerifier --> UC9

    %% Use Case Relationships
    UC6 -. "«include»" .-> UC9
    UC7 -. "«include»" .-> UC9
    UC8 -. "«include»" .-> UC9

    UC4 -. "«extend»" .-> UC3

    classDef actor fill:#08427b,stroke:#052e56,color:#ffffff;
    classDef usecase fill:#E8E8FF,stroke:#6B5BD2,color:#000000;
    class actorStudent,actorOwner,actorVerifier actor;
    class UC1,UC2,UC3,UC4,UC5,UC6,UC7,UC8,UC9 usecase;
```

**Key:** dark-blue rectangles = actors; light ovals = use cases; the large box is the system boundary; solid arrows = actor participation; dashed arrows = «include»/«extend» relationships.

**Why each use case matters (primary audience / risk reduced):** 

| Use case | Primary audience | Risk it reduces |
|---|---|---|
| Browse Verified Listings | Student Seeker | Students waste fare/load chasing outdated posts (our core validated problem) |
| Filter Listings by Price and Location | Student Seeker | Students can't quickly narrow ~50 listings to ones they can afford/reach |
| View Listing Details | Student Seeker | Experiment 1 learning: students want facilities & exact location, not just price |
| Inquire About a Listing | Student Seeker | This is literally our JVB success metric — counts as our "message the post" signal |
| Submit a Listing | Owner/Agent | Owners need a low-friction way to get a listing in front of the verifier |
| Verify a Listing | Team Verifier | De-risks JVB Assumption #3 — owner willingness to be verified |
| Reject a Listing | Team Verifier | Keeps unverifiable/inaccurate listings out of the feed |
| Update Listing Availability | Team Verifier | Prevents the "already taken" problem reported by C.G. and J.G. |
| Log In as Verifier | Team Verifier | Only trusted team members can mark something "Verified" |

**Note:** Resubmitting a rejected listing (see `activity.md` and `state-machine.md`) is the same goal as "Submit a Listing", so it is not drawn as a separate use case. Owners and students have no accounts at MVP stage; only the Team Verifier logs in.
