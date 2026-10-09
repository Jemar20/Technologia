# Draft ERD — Boarding House Listing Verification

**Scope:** One table per stored class; primary and foreign keys; crow's-foot cardinality matching our class diagram; PII columns marked.

```mermaid
---
title: "Draft ERD: Boarding House Listing Verification"
---
erDiagram

    USERS |o--o{ LISTINGS : verifies
    OWNERS ||--|{ LISTINGS : lists
    LISTINGS ||--|{ PHOTOS : has
    LISTINGS ||--o{ INQUIRIES : receives
    LISTINGS ||--o| VERIFICATION_CHECKLISTS : "checked by"

    USERS {
        uuid id PK
        string name "PII"
        string email "PII"
        string password_hash "PII - secret, hashed"
    }

    OWNERS {
        uuid id PK
        string name "PII"
        string contact_number "PII"
        string messenger_link "PII"
    }

    LISTINGS {
        uuid id PK
        uuid owner_id FK "required"
        uuid verified_by FK "nullable until verified"
        string title
        decimal price_monthly
        string address
        decimal latitude
        decimal longitude
        string facilities_description
        string status "ListingStatus value"
        string rejection_reason "nullable"
        datetime submitted_at
        datetime verified_at "nullable"
        datetime updated_at
    }

    PHOTOS {
        uuid id PK
        uuid listing_id FK
        string url
        int sort_order
    }

    INQUIRIES {
        uuid id PK
        uuid listing_id FK
        datetime sent_at
    }

    VERIFICATION_CHECKLISTS {
        uuid id PK
        uuid listing_id FK "unique"
        boolean price_confirmed
        boolean photos_confirmed
        boolean availability_confirmed
        datetime checked_at
    }
```

**Key:** crow's-foot notation: `||` = exactly one, `|o` / `o|` = zero or one, `o{` = zero or many, `|{` = one or many. A column commented `"PII"` holds personally identifiable information. `status` stores one of the six `ListingStatus` values from `class.md` (AWAITING_VERIFICATION, VERIFIED, REJECTED, RESERVED, TAKEN, EXPIRED).

**Cardinality check against class.md:** `USERS |o--o{ LISTINGS` matches `User "0..1" → Listing "0..*"` (a listing has no verifier until it is checked, so `verified_by` is nullable); `OWNERS ||--|{ LISTINGS` matches `Owner "1" → "1..*"`; `LISTINGS ||--|{ PHOTOS` matches `"1" → "1..*"`; `LISTINGS ||--o{ INQUIRIES` matches `"1" → "0..*"`; `LISTINGS ||--o| VERIFICATION_CHECKLISTS` matches `"1" → "0..1"`.

**PII note:** Student inquiries store no student identity (only listing and timestamp), so no student PII is kept. `address` is the property's address, not a person's home address, so it is not marked as PII.
