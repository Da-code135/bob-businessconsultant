# System Architecture Diagram
**University Internship Management System**

---

## High-Level Architecture

```mermaid
graph TB
    subgraph "Client Layer"
        SM[Student Mobile Browser]
        CD[Company Desktop Browser]
        SD[Staff Desktop Browser]
    end

    subgraph "Load Balancing"
        LB[Nginx Load Balancer<br/>SSL Termination]
    end

    subgraph "Application Layer"
        AS1[Application Server 1<br/>Node.js + Express]
        AS2[Application Server 2<br/>Node.js + Express]
        AS3[Application Server N<br/>Node.js + Express]
    end

    subgraph "Caching Layer"
        RC[Redis Cache<br/>Sessions & Data]
    end

    subgraph "Data Layer"
        PG[(PostgreSQL Database<br/>Primary)]
        PGR[(PostgreSQL<br/>Read Replica)]
    end

    subgraph "Storage Layer"
        S3[Object Storage<br/>S3/MinIO<br/>Documents & Files]
    end

    subgraph "External Services"
        ES[Email Service<br/>SendGrid/SES]
        MON[Monitoring<br/>Prometheus + Grafana]
        ERR[Error Tracking<br/>Sentry]
    end

    subgraph "Backup & Recovery"
        BK[Automated Backups<br/>Database + Storage]
    end

    SM -->|HTTPS| LB
    CD -->|HTTPS| LB
    SD -->|HTTPS| LB

    LB -->|Round Robin| AS1
    LB -->|Round Robin| AS2
    LB -->|Round Robin| AS3

    AS1 <-->|Read/Write| PG
    AS2 <-->|Read/Write| PG
    AS3 <-->|Read/Write| PG

    AS1 -->|Read Only| PGR
    AS2 -->|Read Only| PGR
    AS3 -->|Read Only| PGR

    AS1 <-->|Cache| RC
    AS2 <-->|Cache| RC
    AS3 <-->|Cache| RC

    AS1 <-->|Upload/Download| S3
    AS2 <-->|Upload/Download| S3
    AS3 <-->|Upload/Download| S3

    AS1 -->|Send Email| ES
    AS2 -->|Send Email| ES
    AS3 -->|Send Email| ES

    AS1 -->|Metrics| MON
    AS2 -->|Metrics| MON
    AS3 -->|Metrics| MON

    AS1 -->|Errors| ERR
    AS2 -->|Errors| ERR
    AS3 -->|Errors| ERR

    PG -->|Daily Backup| BK
    S3 -->|Versioning| BK

    style SM fill:#e1f5ff
    style CD fill:#e1f5ff
    style SD fill:#e1f5ff
    style LB fill:#fff3e0
    style AS1 fill:#f3e5f5
    style AS2 fill:#f3e5f5
    style AS3 fill:#f3e5f5
    style RC fill:#fce4ec
    style PG fill:#e8f5e9
    style PGR fill:#e8f5e9
    style S3 fill:#fff9c4
    style ES fill:#e0f2f1
    style MON fill:#e0f2f1
    style ERR fill:#e0f2f1
    style BK fill:#efebe9
```

---

## Application Component Architecture

```mermaid
graph TB
    subgraph "Frontend - React Application"
        SP[Student Portal<br/>Mobile-First UI]
        CP[Company Portal<br/>Desktop UI]
        SDP[Staff Dashboard<br/>Desktop UI]
        AUTH[Authentication Module<br/>Login/Register/Reset]
    end

    subgraph "Backend - Express API"
        API[REST API Layer<br/>OpenAPI 3.0]
        
        subgraph "Business Logic"
            UM[User Management<br/>Registration & Profiles]
            AM[Application Management<br/>Workflow Engine]
            PM[Placement Management<br/>Assignment & Tracking]
            EM[Evaluation Module<br/>Ratings & Feedback]
            NM[Notification Module<br/>Email & In-App]
            RM[Reporting Engine<br/>Analytics & Export]
            SM[Search Module<br/>Full-Text Search]
        end

        subgraph "Middleware"
            AUTHM[Auth Middleware<br/>JWT Validation]
            RBAC[RBAC Middleware<br/>Permission Checks]
            VAL[Validation Middleware<br/>Joi Schemas]
            LOG[Logging Middleware<br/>Winston + Morgan]
            ERH[Error Handler<br/>Centralized Errors]
        end
    end

    subgraph "Data Access Layer"
        ORM[Database Models<br/>Sequelize ORM]
        FS[File Service<br/>S3 SDK]
        CS[Cache Service<br/>Redis Client]
    end

    SP --> AUTH
    CP --> AUTH
    SDP --> AUTH
    AUTH --> API

    API --> AUTHM
    AUTHM --> RBAC
    RBAC --> VAL
    VAL --> LOG

    LOG --> UM
    LOG --> AM
    LOG --> PM
    LOG --> EM
    LOG --> NM
    LOG --> RM
    LOG --> SM

    UM --> ORM
    AM --> ORM
    PM --> ORM
    EM --> ORM
    RM --> ORM
    SM --> ORM

    AM --> FS
    UM --> FS

    UM --> CS
    AM --> CS
    SM --> CS

    NM --> ES[Email Service]

    UM --> ERH
    AM --> ERH
    PM --> ERH
    EM --> ERH
    NM --> ERH
    RM --> ERH
    SM --> ERH

    style SP fill:#e1f5ff
    style CP fill:#e1f5ff
    style SDP fill:#e1f5ff
    style AUTH fill:#fff3e0
    style API fill:#f3e5f5
    style AUTHM fill:#fce4ec
    style RBAC fill:#fce4ec
    style VAL fill:#fce4ec
    style LOG fill:#fce4ec
    style ERH fill:#fce4ec
    style UM fill:#e8f5e9
    style AM fill:#e8f5e9
    style PM fill:#e8f5e9
    style EM fill:#e8f5e9
    style NM fill:#e8f5e9
    style RM fill:#e8f5e9
    style SM fill:#e8f5e9
    style ORM fill:#fff9c4
    style FS fill:#fff9c4
    style CS fill:#fff9c4
```

---

## Data Flow - Student Application Process

```mermaid
sequenceDiagram
    participant S as Student<br/>(Mobile)
    participant LB as Load Balancer
    participant AS as Application Server
    participant RC as Redis Cache
    participant DB as PostgreSQL
    participant S3 as Object Storage
    participant ES as Email Service

    S->>LB: Browse internships
    LB->>AS: Forward request
    AS->>RC: Check cached listings
    alt Cache Hit
        RC-->>AS: Return cached data
    else Cache Miss
        AS->>DB: Query active internships
        DB-->>AS: Return results
        AS->>RC: Cache results (5 min TTL)
    end
    AS-->>S: Display opportunities

    S->>LB: Upload CV & transcript
    LB->>AS: Forward files
    AS->>S3: Store documents
    S3-->>AS: Return file URLs
    AS->>DB: Save file metadata
    AS-->>S: Upload successful

    S->>LB: Submit application
    LB->>AS: Forward application
    AS->>DB: Create application record
    AS->>DB: Update application status
    DB-->>AS: Confirm transaction
    AS->>RC: Invalidate related caches
    AS->>ES: Send confirmation email
    ES-->>S: Email notification
    AS-->>S: Application submitted

    AS->>ES: Notify company
    ES->>Company: New application alert
```

---

## Data Flow - Company Review & Selection

```mermaid
sequenceDiagram
    participant C as Company<br/>(Desktop)
    participant LB as Load Balancer
    participant AS as Application Server
    participant DB as PostgreSQL
    participant S3 as Object Storage
    participant ES as Email Service

    C->>LB: View applications
    LB->>AS: Forward request
    AS->>DB: Query applications for company
    DB-->>AS: Return application list
    AS-->>C: Display applications

    C->>LB: View student profile & documents
    LB->>AS: Forward request
    AS->>DB: Get student details
    AS->>S3: Generate presigned URLs
    S3-->>AS: Return secure URLs
    AS-->>C: Profile + document links

    C->>LB: Shortlist candidate
    LB->>AS: Update status request
    AS->>DB: Update application status
    DB-->>AS: Confirm update
    AS->>ES: Notify student
    ES->>Student: Status update email
    AS->>ES: Notify staff
    ES->>Staff: Shortlist notification
    AS-->>C: Status updated

    C->>LB: Accept candidate
    LB->>AS: Final selection
    AS->>DB: Update to accepted
    AS->>DB: Close other applications
    DB-->>AS: Transaction complete
    AS->>ES: Notify all parties
    AS-->>C: Selection confirmed
```

---

## Data Flow - Staff Coordination

```mermaid
sequenceDiagram
    participant ST as Staff<br/>(Desktop)
    participant LB as Load Balancer
    participant AS as Application Server
    participant DB as PostgreSQL
    participant ES as Email Service

    ST->>LB: View dashboard
    LB->>AS: Request overview
    AS->>DB: Query statistics
    DB-->>AS: Return metrics
    AS-->>ST: Display dashboard

    ST->>LB: Approve company registration
    LB->>AS: Approval request
    AS->>DB: Update company status
    AS->>ES: Send approval email
    ES->>Company: Account activated
    AS-->>ST: Company approved

    ST->>LB: Assign placement
    LB->>AS: Assignment request
    AS->>DB: Create placement record
    AS->>DB: Update application status
    DB-->>AS: Confirm assignment
    AS->>ES: Notify student
    AS->>ES: Notify company
    AS-->>ST: Placement assigned

    ST->>LB: Generate report
    LB->>AS: Report request
    AS->>DB: Complex analytics query
    DB-->>AS: Return aggregated data
    AS-->>ST: Download report (CSV/PDF)
```

---

## Security Architecture

```mermaid
graph TB
    subgraph "Security Layers"
        SSL[SSL/TLS Encryption<br/>Let's Encrypt]
        WAF[Web Application Firewall<br/>Rate Limiting]
        AUTH[JWT Authentication<br/>Passport.js]
        RBAC[Role-Based Access Control<br/>Middleware]
        HASH[Password Hashing<br/>bcrypt]
        VALID[Input Validation<br/>Joi + Sanitization]
        AUDIT[Audit Logging<br/>All Sensitive Operations]
    end

    subgraph "Data Protection"
        ENC[Encryption at Rest<br/>Database + Storage]
        BACK[Encrypted Backups<br/>30-day Retention]
        GDPR[GDPR Compliance<br/>Data Export/Deletion]
    end

    Internet -->|HTTPS Only| SSL
    SSL --> WAF
    WAF --> AUTH
    AUTH --> RBAC
    RBAC --> VALID
    VALID --> Application
    Application --> HASH
    Application --> AUDIT
    Application --> ENC
    ENC --> BACK
    Application --> GDPR

    style SSL fill:#ffcdd2
    style WAF fill:#ffcdd2
    style AUTH fill:#f8bbd0
    style RBAC fill:#f8bbd0
    style HASH fill:#e1bee7
    style VALID fill:#e1bee7
    style AUDIT fill:#d1c4e9
    style ENC fill:#c5cae9
    style BACK fill:#c5cae9
    style GDPR fill:#bbdefb
```

---

## Deployment Architecture

```mermaid
graph TB
    subgraph "Development"
        DEV[Developer Workstation<br/>Docker Compose]
    end

    subgraph "CI/CD Pipeline"
        GIT[GitHub Repository]
        GHA[GitHub Actions<br/>Test + Build]
        REG[Container Registry<br/>Docker Hub]
    end

    subgraph "Staging Environment"
        STAG[Staging Server<br/>Mirror Production]
    end

    subgraph "Production Environment"
        PROD[Production Cluster<br/>Load Balanced]
    end

    DEV -->|Push Code| GIT
    GIT -->|Trigger| GHA
    GHA -->|Run Tests| GHA
    GHA -->|Build Image| REG
    REG -->|Deploy| STAG
    STAG -->|Manual Approval| PROD

    style DEV fill:#e8f5e9
    style GIT fill:#fff3e0
    style GHA fill:#f3e5f5
    style REG fill:#e1f5ff
    style STAG fill:#fff9c4
    style PROD fill:#ffcdd2