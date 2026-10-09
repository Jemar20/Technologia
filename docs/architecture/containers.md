# C4 Container Diagram — Boarding House Listing Verification

**Scope:** Every separately deployable unit with its technology; every arrow labelled with intent and protocol.
 
```mermaid
---
title: "C4 Container Diagram: Boarding House Listing Verification"
---
flowchart TB

    %% =========================
    %% ACTORS
    %% =========================
    student["<b>Student Seeker</b><br/><i>[Person]</i>"]
    owner["<b>Boarding House Owner / Agent</b><br/><i>[Person]</i>"]
    verifier["<b>Team Verifier</b><br/><i>[Person]</i>"]

    %% =========================
    %% SYSTEM BOUNDARY
    %% =========================
    subgraph boundary["Boarding House Listing Verification - System Boundary"]
        direction TB

        webapp["<b>Web App</b><br/><i>[Container: Next.js, React + API Routes]</i><br/>Serves listing pages and handles<br/>submission, verification, and<br/>inquiry requests"]

        db[("<b>Supabase</b><br/><i>[Container: Managed PostgreSQL Database]</i><br/>Stores users, listings, photos,<br/>owners, verification data,<br/>and inquiry records")]

        images[("<b>Cloudinary</b><br/><i>[Container: Managed Image Storage]</i><br/>Stores and serves<br/>listing photos")]
    end

    %% =========================
    %% EXTERNAL SYSTEMS
    %% =========================
    messenger["<b>Facebook Messenger</b><br/><i>[External System]</i>"]
    maps["<b>Google Maps</b><br/><i>[External System]</i>"]

    %% =========================
    %% USER INTERACTIONS
    %% =========================
    student -->|"Views listings and<br/>contacts owner - HTTPS"| webapp

    owner -->|"Submits boarding house<br/>listing - HTTPS"| webapp

    verifier -->|"Logs in and verifies or<br/>updates listings - HTTPS"| webapp

    %% =========================
    %% WEB APP CONNECTIONS
    %% =========================
    webapp -->|"Reads and writes users,<br/>listings, inquiries - HTTPS"| db

    webapp -->|"Uploads and retrieves<br/>listing photos - HTTPS"| images

    webapp -->|"Opens pre-filled<br/>inquiry chat - HTTPS redirect"| messenger

    webapp -->|"Displays listing location<br/>and map pin - HTTPS"| maps

    %% =========================
    %% STYLING
    %% =========================
    classDef person fill:#08427b,stroke:#052e56,color:#ffffff,stroke-width:1px;
    classDef container fill:#1168bd,stroke:#0b4884,color:#ffffff,stroke-width:1px;
    classDef external fill:#999999,stroke:#6b6b6b,color:#ffffff,stroke-width:1px;

    class student,owner,verifier person;
    class webapp,db,images container;
    class messenger,maps external;
```

**Key:** dark-blue boxes = people; medium-blue boxes inside the outlined boundary = our containers (cylinder shape = a database/storage container); grey boxes = external systems. Arrows are labelled "intent — protocol."
 
**One-sentence container decision:** We draw our Next.js app as a **single container**, because its pages and API routes are built and deployed together as one unit on Vercel — we have no separate backend service at MVP stage.

**Managed services:** Supabase and Cloudinary are hosted services we rent, but each stores data that only our system uses, so we draw them as containers inside the boundary. They appear as separate boxes in `deployment.md` and are not external systems in `context.md`.
