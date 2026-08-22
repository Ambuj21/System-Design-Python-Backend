# 01 — Client–Server Architecture

## What is it?
A **client** requests a service and a **server** processes the request and returns a response.

```mermaid
flowchart LR
    U[User] --> C[Browser / Angular Client]
    C -->|HTTP Request| S[Backend Server]
    S -->|Query| DB[(Database)]
    DB -->|Data| S
    S -->|HTTP Response| C
    C --> U
```

## Key concepts
- **Client:** Browser, Angular app, mobile app, etc.
- **Server:** Runs application/business logic and handles requests.
- **Database:** Stores persistent data.
- **API:** Interface through which clients communicate with backend services.

## Typical flow

```mermaid
sequenceDiagram
    participant U as User
    participant C as Angular/Browser
    participant B as Backend
    participant D as Database
    U->>C: Perform action
    C->>B: HTTP request
    B->>D: Query
    D-->>B: Data
    B-->>C: HTTP response
    C-->>U: Update UI
```

## Why not connect Angular directly to the database?

```text
❌ Angular → Database
✅ Angular → Backend API → Database
```

The backend handles authentication, authorization, validation, business logic, transactions, security, and controlled database access.

## Statelessness

A stateless application server does not depend on state stored only in its own memory.

```mermaid
flowchart LR
    C[Client] --> LB[Load Balancer]
    LB --> S1[Server 1]
    LB --> S2[Server 2]
    S1 --> R[(Shared Redis / DB)]
    S2 --> R
```

## Interview takeaway
A client sends a request to a server. The server processes it, may communicate with databases or other services, and returns a response.
