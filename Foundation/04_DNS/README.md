# 04 — DNS (Domain Name System)

## What is DNS?
DNS is a distributed naming system that translates domain names into IP addresses.

```text
google.com
    ↓
   DNS
    ↓
IP address
```

## DNS lookup

```mermaid
flowchart TD
    A[User enters google.com] --> B[Browser / OS]
    B --> C[DNS Resolver]
    C --> D{Answer cached?}
    D -->|Yes| E[Return cached IP]
    D -->|No| F[Root DNS]
    F --> G[.com TLD Server]
    G --> H[Authoritative DNS for google.com]
    H --> E
    E --> I[Client connects to IP:443]
```

## Root, TLD and authoritative DNS

```text
Root DNS
   ↓
.com TLD
   ↓
Authoritative DNS for google.com
   ↓
DNS record / IP address
```

The root directs the resolver toward the appropriate TLD infrastructure. The TLD directs it toward authoritative DNS servers.

## Cache and TTL
Resolvers can cache DNS answers. **TTL (Time To Live)** indicates how long a DNS result can generally be cached before refreshing.

```mermaid
flowchart LR
    R[Resolver] --> C[DNS Cache]
    C -->|HIT| IP[Return IP]
    C -->|MISS| H[Authoritative DNS]
    H --> IP
    IP --> C
```

## Important records

| Record | Purpose |
|---|---|
| A | IPv4 address |
| AAAA | IPv6 address |
| CNAME | Alias to another domain name |

## DNS does not call the web server
DNS answers:

> What IP address belongs to this domain?

Then the client uses that IP to establish the actual connection.

```mermaid
sequenceDiagram
    participant C as Client
    participant D as DNS
    participant S as Web Server
    C->>D: What is google.com?
    D-->>C: IP address
    C->>S: Connect to IP:443
```

## Interview takeaway

```text
Domain → DNS → IP → Port → Service
```
