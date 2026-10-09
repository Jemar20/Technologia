# Sequence Diagram — Student Views a Listing and Sends an Inquiry

**Scope:** Our riskiest flow (it is literally our JVB success metric); replies drawn; branches shown with alt; an external system (Facebook Messenger) as a lifeline.

```mermaid
---
title: "Sequence Diagram: Student views a listing and sends an inquiry"
---
sequenceDiagram

    actor Student
    participant Page as Listing Page<br/>(Next.js)
    participant API as API Route<br/>(Next.js)
    participant DB as Supabase<br/>(Database)
    participant Messenger as Facebook Messenger

    Student->>Page: Open verified listing
    Page->>API: GET /api/listings/:id
    API->>DB: Fetch listing, photos, and owner
    DB-->>API: Listing data
    API-->>Page: 200 OK, listing JSON
    Page-->>Student: Display listing details

    Student->>Page: Click "Message Owner"
    Page->>API: POST /api/listings/:id/inquiries
    API->>DB: Save inquiry (listingId, sentAt)
    DB-->>API: Inquiry saved
    API-->>Page: 201 Created, owner contact info

    alt Owner has a Messenger link
        Page-)Messenger: Redirect with pre-filled message (asynchronous hand-off)
        Messenger--)Student: Owner chat opens
    else Owner has no Messenger link
        Page-->>Student: Display owner's phone number
    end
```

**Key:** solid arrow with filled head = synchronous request; dashed arrow = reply; open-head arrow = asynchronous message (the browser hands the student over to Messenger and does not wait); `alt` / `else` = either-or branch.

**Why this is our riskiest flow:** every inquiry logged here is exactly what our JVB success criterion counts ("at least 15 of ~50 students message the post within 5 days"). If this flow breaks or undercounts, we cannot tell whether an experiment passed or failed. The inquiry is saved before the redirect, so it is counted even if the student never completes the Messenger chat.
