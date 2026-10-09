# Deployment Diagram — Provisional

**Scope:** Titled "Provisional"; nodes, execution environments, and artifacts; a protocol on every path; no secrets or real addresses.

```mermaid
---
title: "Deployment Diagram (Provisional): Planned hosting"
---
flowchart LR

    subgraph ClientNode["Node: Student / Owner / Verifier Device"]
        direction TB
        Browser["Execution Environment:<br/>Mobile / Desktop Browser"]
    end

    subgraph VercelNode["Node: Vercel Platform"]
        direction TB
        WebAppArtifact["Artifact:<br/>Next.js Application<br/>(pages + API routes)"]
    end

    subgraph DBNode["Node: Supabase Cloud (Managed Service)"]
        direction TB
        DBArtifact[("Artifact:<br/>Production Database")]
    end

    subgraph CloudinaryNode["Node: Cloudinary (Managed Service)"]
        direction TB
        ImgArtifact[("Artifact:<br/>Listing Photo Storage")]
    end

    subgraph MapsNode["Node: Google Cloud (External SaaS)"]
        direction TB
        MapsArtifact["Artifact:<br/>Google Maps Service"]
    end

    subgraph MessengerNode["Node: Meta Platforms (External SaaS)"]
        direction TB
        MsgArtifact["Artifact:<br/>Facebook Messenger"]
    end

    Browser -->|"HTTPS"| WebAppArtifact
    WebAppArtifact -->|"HTTPS / Supabase API"| DBArtifact
    WebAppArtifact -->|"HTTPS"| ImgArtifact
    WebAppArtifact -->|"HTTPS"| MapsArtifact
    Browser -->|"HTTPS redirect (pre-filled chat link)"| MsgArtifact

    classDef client fill:#08427b,stroke:#052e56,color:#ffffff,stroke-width:1px;
    classDef ours fill:#1168bd,stroke:#0b4884,color:#ffffff,stroke-width:1px;
    classDef external fill:#999999,stroke:#6b6b6b,color:#ffffff,stroke-width:1px;

    class Browser client;
    class WebAppArtifact,DBArtifact,ImgArtifact ours;
    class MapsArtifact,MsgArtifact external;
```

**Key:** each outlined box is a node (a device or a cloud execution environment), read left to right as a request travels outward; the box or cylinder inside a node is the artifact (a deployed build or dataset) running there; blue = ours or managed for us, grey = external SaaS we do not control; every arrow carries its protocol.

**Why "Provisional":** we have not yet signed up for Vercel, Supabase, or Cloudinary accounts. These are our planned providers based on the Next.js stack, not confirmed infrastructure. No real hostnames, API keys, or connection strings appear here. The Messenger redirect starts from the student's browser, as in `sequence.md`.
