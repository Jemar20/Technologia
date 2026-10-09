# Activity Diagram — Listing Submission & Verification Workflow

**Scope:** Our core workflow; one swimlane per role and one for the system; every decision labelled with guards; start and end nodes.

```mermaid
---
title: "Activity Diagram: Listing submission and verification"
---
flowchart TB
 
    subgraph OWNER["OWNER / AGENT"]
        direction TB
        A([Start<br/>Has a room to list])
        B["Fill out form<br/>Price, photos, address"]
        C["Submit listing"]
        L([End: Listing live<br/>Visible to students])
        R([End: Owner may<br/>fix and resubmit])
        A --> B
        B --> C
    end

    subgraph SYSTEM["SYSTEM"]
        direction TB
        D["Save listing<br/>status = AWAITING_VERIFICATION"]
        E["Add listing to<br/>verifier queue"]
        J["Set status = VERIFIED<br/>Publish to student feed"]
        D --> E
    end

    subgraph VERIFIER["TEAM VERIFIER"]
        direction TB
        F["Visit in person<br/>Checklist inspection"]
        G{"Checks pass?"}
        H["Mark verified<br/>Checklist complete"]
        I["Mark rejected<br/>Reason required"]
        K["Tell owner the reason<br/>Messenger or call, manual"]
        F --> G
        G -->|"[Yes: all checks pass]"| H
        G -->|"[No: any check fails]"| I
        I --> K
    end

    C --> D
    E --> F
    H --> J
    J --> L
    K --> R

    classDef purple fill:#4338A0,color:#FFFFFF,stroke:#AAAAAA;
    classDef gray fill:#484844,color:#FFFFFF,stroke:#AAAAAA;
    classDef decision fill:#B45309,color:#FFFFFF,stroke:#AAAAAA;

    class B,C,F,H,I,K purple;
    class D,E,J gray;
    class G decision;
    class A,L,R gray;
```

**Key:** each outlined box is a swimlane (one role or the system); rounded nodes = start and end; rectangles = actions; diamond = decision, with the guard condition in brackets on each outgoing arrow.

**Traceability:** This is the workflow behind Experiments 1 and 2 on our JVB: a team member personally checks a listing before it reaches students. The "No" branch is our own addition, because our riskiest assumption (#3, owners willing to be verified) is still unvalidated. There is no automatic notification service in our architecture, so the system only queues the listing, and the Verifier tells the owner about a rejection by hand.
