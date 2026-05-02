# System Components
**University Internship Management System**

---

| Component | Responsibility | Connects To | Supports (FR-IDs) |
|-----------|---------------|------------|-------------------|
| **Web Application Server** | Hosts the main application, handles HTTP requests, executes business logic, manages sessions, and serves responsive UI for all user types. | Database Server, Object Storage, Email Service, Cache Layer | FR-001 through FR-035 (all functional requirements) |
| **Database Server** | Stores all structured data including user accounts, profiles, internship postings, applications, placements, evaluations, and audit logs. Ensures data integrity and supports complex queries. | Web Application Server | FR-001, FR-002, FR-003, FR-007, FR-011, FR-012, FR-016, FR-017, FR-021, FR-022, FR-025, FR-027, FR-029, FR-030, FR-031, FR-032 |
| **Object Storage Service** | Stores and serves uploaded documents (CVs, transcripts, cover letters) with secure access control. Handles large file storage separately from database. | Web Application Server | FR-008, FR-009, FR-010, FR-014, FR-017 |
| **Authentication Module** | Manages user registration, login, password reset, session management, and role-based access control. Enforces security policies. | Database Server, Email Service | FR-001, FR-002, FR-003, FR-004, FR-005, FR-006 |
| **Student Portal** | Mobile-optimized interface for students to manage profiles, browse opportunities, submit applications, upload documents, and provide feedback. | Web Application Server, Authentication Module | FR-007, FR-008, FR-009, FR-016, FR-017, FR-018, FR-019, FR-020, FR-030 |
| **Company Portal** | Desktop-optimized interface for companies to manage profiles, post opportunities, review applications, select candidates, and evaluate interns. | Web Application Server, Authentication Module | FR-011, FR-012, FR-013, FR-014, FR-015, FR-029 |
| **Staff Dashboard** | Desktop interface for university staff to oversee all system activities, approve registrations, assign placements, monitor progress, generate reports, and send announcements. | Web Application Server, Authentication Module | FR-022, FR-023, FR-024, FR-025, FR-026, FR-027, FR-028 |
| **Application Management Engine** | Handles application workflow including submission, status tracking, notifications, and state transitions. Enforces business rules (no duplicate applications, deadline validation). | Database Server, Notification Service | FR-016, FR-017, FR-018, FR-019, FR-020, FR-021 |
| **Notification Service** | Sends email notifications and manages in-app notifications for application updates, placement assignments, and announcements. Handles notification preferences. | Email Service, Database Server | FR-019, FR-028, FR-033, FR-034, FR-035 |
| **Evaluation Module** | Manages post-internship evaluations from both companies and students. Calculates ratings and stores feedback. | Database Server | FR-029, FR-030, FR-031, FR-032 |
| **Reporting Engine** | Generates statistical reports on placements, student participation, company engagement, and evaluation metrics. Supports data export. | Database Server | FR-027 |
| **Search & Filter Service** | Provides fast search and filtering capabilities for internship opportunities, student profiles, and applications. Implements indexing for performance. | Database Server, Cache Layer | FR-016, FR-022 |
| **Cache Layer** | Improves performance by caching frequently accessed data (active internship listings, user sessions, search results). Reduces database load. | Web Application Server, Database Server | Performance optimization for all read-heavy operations |
| **Load Balancer** | Distributes incoming traffic across multiple application server instances. Provides health checks and automatic failover. | Web Application Server | Supports scalability and reliability requirements |
| **Email Service** | External SMTP service for sending transactional emails (password resets, notifications, announcements). | Notification Service | FR-005, FR-019, FR-028, FR-033 |
| **Backup Service** | Automated daily backups of database and document storage with 30-day retention. Supports disaster recovery. | Database Server, Object Storage | Data reliability and recovery requirements |
| **Monitoring & Logging** | Collects application logs, performance metrics, error tracking, and security audit logs. Provides real-time alerts. | All Components | Security audit logging, system health monitoring |

---

## Component Interaction Flow

### Student Application Submission
1. Student authenticates via **Authentication Module**
2. **Student Portal** displays opportunities from **Search & Filter Service**
3. Student selects opportunity and uploads documents to **Object Storage Service**
4. **Application Management Engine** validates and creates application in **Database Server**
5. **Notification Service** sends confirmation to student and alert to company via **Email Service**

### Company Candidate Selection
1. Company authenticates via **Authentication Module**
2. **Company Portal** retrieves applications from **Database Server**
3. Company reviews student profiles and documents from **Object Storage Service**
4. Company updates application status via **Application Management Engine**
5. **Notification Service** alerts student and staff of status change

### Staff Placement Assignment
1. Staff authenticates via **Authentication Module**
2. **Staff Dashboard** displays all approved applications from **Database Server**
3. Staff assigns student to placement via **Application Management Engine**
4. **Notification Service** sends confirmation to student and company
5. **Reporting Engine** updates placement statistics

### Performance Optimization
- **Cache Layer** stores frequently accessed data (active listings, user sessions)
- **Load Balancer** distributes traffic during peak periods
- **Search & Filter Service** uses indexed queries for fast results
- **Object Storage Service** serves documents via CDN for faster downloads