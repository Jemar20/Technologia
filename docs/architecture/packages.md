# Package Diagram — Folder Structure

**Scope:** The folder structure we will create; dependency arrows; one sentence stating our layering rule.

```mermaid
---
title: "Package Diagram: Planned folder structure and dependencies"
---
flowchart TB

    app["<b>app/</b><br/>(Pages & API Routes)"]
    components["<b>components/</b><br/>(Reusable UI Components)"]
    services["<b>services/</b><br/>(Listing Service<br/>Verification Service<br/>Inquiry Service)"]
    data["<b>data/</b><br/>(Supabase Client<br/>Repositories)"]
    lib["<b>lib/</b><br/>(Authentication<br/>Image Storage Adapter<br/>Maps Client<br/>Messenger Link Helper)"]

    supabase[("Supabase<br/>Database")]
    cloudinary[("Cloudinary<br/>Image Storage")]
    maps["Google Maps"]
    messenger["Facebook Messenger"]

    app --> components
    app --> services
    app --> lib
    components --> lib
    services --> data
    services --> lib

    data -->|"Database queries"| supabase
    lib -->|"Image upload / display"| cloudinary
    lib -->|"Map services"| maps
    lib -->|"Inquiry links"| messenger

    classDef appLayer fill:#4338A0,stroke:#AAA,color:#FFFFFF,stroke-width:1px;
    classDef serviceLayer fill:#1168BD,stroke:#0B4884,color:#FFFFFF,stroke-width:1px;
    classDef libraryLayer fill:#4A4A46,stroke:#AAA,color:#FFFFFF,stroke-width:1px;
    classDef external fill:#999999,stroke:#6B6B6B,color:#FFFFFF,stroke-width:1px;

    class app,components appLayer;
    class services,data serviceLayer;
    class lib libraryLayer;
    class supabase,cloudinary,maps,messenger external;
```

**Key:** arrows point from a package to the package it depends on (imports from); grey boxes and cylinders are external services, not folders.

**Layering rule:** Pages and API routes in `app/` never import `data/` directly; only `services/` may import `data/`, `components/` never imports `services/` or `data/`, and every external service except the database is reached only through `lib/`.
