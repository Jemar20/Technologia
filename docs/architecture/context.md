# C4 System Context Diagram — Boarding House Listing Verification

**Scope:** The whole MVP as one box, every user role, and every external system it depends on.

```mermaid
---
title: "C4 System Context Diagram: Boarding House Listing Verification"
---
flowchart TB

    %% =========================
    %% ACTORS
    %% =========================

    student["<b>Student Seeker</b><br/><i>[Person]</i><br/>1st-year or 4th-year transferee<br/>SSU Bulan student looking for<br/>a boarding house"]

    owner["<b>Boarding House Owner / Agent</b><br/><i>[Person]</i><br/>Lists available rooms near<br/>SSU Bulan Campus"]

    verifier["<b>Team Verifier</b><br/><i>[Person]</i><br/>Technologia member who<br/>checks listings in person"]


    %% =========================
    %% MAIN SYSTEM
    %% =========================

    bbv["<b>Boarding House Listing Verification</b><br/><i>[Software System]</i><br/>Allows students to browse boarding houses<br/>personally checked for price, photos,<br/>and current availability"]


    %% =========================
    %% EXTERNAL SYSTEMS
    %% =========================

    messenger["<b>Facebook Messenger</b><br/><i>[External System]</i><br/>Chat application used by students<br/>to contact boarding house owners"]

    maps["<b>Google Maps</b><br/><i>[External System]</i><br/>Displays the exact pinned<br/>location of a listing"]


    %% =========================
    %% SYSTEM RELATIONSHIPS
    %% =========================

    student -->|"Browses verified listings<br/>and sends inquiries"| bbv

    owner -->|"Submits boarding house<br/>listing for verification"| bbv

    verifier -->|"Reviews, verifies, and<br/>updates listing availability"| bbv

    bbv -->|"Opens direct inquiry<br/>chat with the owner"| messenger

    bbv -->|"Displays exact listing<br/>location and map pin"| maps


    %% =========================
    %% STYLING
    %% =========================

    classDef person fill:#08427b,stroke:#052e56,color:#ffffff,stroke-width:1px;
    classDef system fill:#1168bd,stroke:#0b4884,color:#ffffff,stroke-width:1px;
    classDef external fill:#999999,stroke:#6b6b6b,color:#ffffff,stroke-width:1px;

    class student,owner,verifier person;
    class bbv system;
    class messenger,maps external;
```

**Key:** dark-blue boxes = people (actors); medium-blue box = our system (center of the diagram); grey boxes = external systems we depend on but do not own. Every arrow is labelled with the intent of that interaction, not just "uses." 

**Traceability:** Student Seeker, Boarding House Owner/Agent, and Team Verifier are the same three roles validated in Experiment 1 of our Javelin Validation Board (JVB) and named in our Lean Canvas Customer Segments / Early Adopters boxes. Facebook Messenger and Google Maps are carried over directly from the "Bulan Barter" Facebook group channel we already validated — this MVP wraps a verification layer and a browsable list around that channel, it does not replace it.

**Scope note:** Supabase (database) and Cloudinary (photo storage) are not drawn here. They are managed services used only by our system, so they appear as containers in `containers.md`.
