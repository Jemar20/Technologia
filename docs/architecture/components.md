# UML Component Diagram — API Container

**Scope:** Our API container; the interfaces each component provides and requires; every external service kept behind an adapter interface.
 
```mermaid
---
title: "UML Component Diagram: Next.js API container"
---
flowchart TB

    subgraph API["API Container - Next.js API Routes"]
        direction TB
        ListingComp["Listing Component"]
        VerificationComp["Verification Component"]
        InquiryComp["Inquiry Component"]
        AuthComp["Authentication Component"]
    end

    IListingAPI(["«interface»<br/>IListingAPI"])
    IVerificationAPI(["«interface»<br/>IVerificationAPI"])
    IInquiryAPI(["«interface»<br/>IInquiryAPI"])
    IAuthAPI(["«interface»<br/>IAuthAPI"])

    IListingRepository(["«interface»<br/>IListingRepository"])
    IUserRepository(["«interface»<br/>IUserRepository"])
    IImageStorage(["«interface»<br/>IImageStorage"])
    IMapsProvider(["«interface»<br/>IMapsProvider"])
    IMessengerLinks(["«interface»<br/>IMessengerLinks"])

    Supabase[("Supabase<br/>Database")]
    Cloudinary[("Cloudinary<br/>Image Storage")]
    GoogleMaps["Google Maps"]
    Messenger["Facebook Messenger"]

    ListingComp -->|provides| IListingAPI
    VerificationComp -->|provides| IVerificationAPI
    InquiryComp -->|provides| IInquiryAPI
    AuthComp -->|provides| IAuthAPI

    ListingComp -->|requires| IListingRepository
    ListingComp -->|requires| IImageStorage
    ListingComp -->|requires| IMapsProvider
    VerificationComp -->|requires| IListingRepository
    VerificationComp -->|requires| IAuthAPI
    InquiryComp -->|requires| IListingRepository
    InquiryComp -->|requires| IMessengerLinks
    AuthComp -->|requires| IUserRepository

    IListingRepository -.->|adapter wraps| Supabase
    IUserRepository -.->|adapter wraps| Supabase
    IImageStorage -.->|adapter wraps| Cloudinary
    IMapsProvider -.->|adapter wraps| GoogleMaps
    IMessengerLinks -.->|adapter wraps| Messenger

    classDef component fill:#4338A0,stroke:#AAA,color:#FFF,stroke-width:1px;
    classDef interface fill:#4A4A46,stroke:#AAA,color:#FFF,stroke-width:1px;
    classDef external fill:#999999,color:#FFF,stroke:#6B6B6B,stroke-width:1px;

    class ListingComp,VerificationComp,InquiryComp,AuthComp component;
    class IListingAPI,IVerificationAPI,IInquiryAPI,IAuthAPI,IListingRepository,IUserRepository,IImageStorage,IMapsProvider,IMessengerLinks interface;
    class Supabase,Cloudinary,GoogleMaps,Messenger external;
```

**Key:** purple rectangles = components; grey rounded nodes = interfaces; solid arrows = provides / requires; dashed arrows = an adapter wrapping an external service, so no component talks to Supabase, Cloudinary, Google Maps, or Messenger directly.

**Traceability:** The five external dependencies match `containers.md` (Supabase, Cloudinary, Google Maps, Messenger). Keeping each behind its own interface means a future swap, such as replacing Cloudinary with Supabase Storage, only touches the adapter and not the Listing or Verification components. The Messenger adapter only builds the pre-filled chat link used in `sequence.md`.
