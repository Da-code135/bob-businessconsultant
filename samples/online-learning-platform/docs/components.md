# System Components

| Component | Responsibility | Connects To | Supports (FR-IDs) |
|-----------|---------------|------------|-------------------|
| **Web Client (PWA)** | Renders user interface, handles user interactions, manages offline content caching via service workers, provides responsive layout for mobile and desktop | API Gateway, Service Worker Cache | FR-001 to FR-040 (all user-facing functionality) |
| **Service Worker** | Intercepts network requests, caches static assets and course content, enables offline access, manages background sync for progress updates | Web Client, API Gateway, IndexedDB | FR-031, FR-032, FR-033, FR-034, FR-035 |
| **API Gateway** | Routes incoming HTTP requests to appropriate backend services, validates JWT tokens, enforces rate limiting | Web Client, Authentication Service, Course Service, Content Service, Enrollment Service, Assignment Service | All FR (entry point for all API calls) |
| **Authentication Service** | Handles user registration, login, password hashing, JWT token generation and validation, session management | API Gateway, Database | FR-001, FR-002, FR-003, FR-004, FR-005 |
| **Course Service** | Manages course CRUD operations, course publishing status, course listing and search | API Gateway, Database, Content Service | FR-006, FR-007, FR-008, FR-009, FR-010 |
| **Content Service** | Handles lesson content upload, storage, retrieval, organization into modules, content deletion, file serving | API Gateway, Database, File Storage | FR-011, FR-012, FR-013, FR-014, FR-015, FR-021, FR-025 |
| **Enrollment Service** | Manages student course enrollments, enrollment validation, access control to course content | API Gateway, Database, Course Service | FR-016, FR-017, FR-018, FR-019, FR-020 |
| **Progress Tracking Service** | Records lesson completion, calculates course progress percentages, syncs offline progress data | API Gateway, Database, Enrollment Service | FR-022, FR-023, FR-024, FR-034 |
| **Assignment Service** | Manages assignment creation, submission handling, file uploads, grading, feedback delivery | API Gateway, Database, File Storage, Enrollment Service | FR-026, FR-027, FR-028, FR-029, FR-030 |
| **Dashboard Service** | Aggregates data for instructor and student dashboards, generates progress reports and statistics | API Gateway, Database, Course Service, Enrollment Service, Progress Tracking Service, Assignment Service | FR-036, FR-037, FR-038, FR-039, FR-040 |
| **Database** | Persists all application data including users, courses, lessons, enrollments, progress, assignments, grades | All backend services | All FR (data persistence layer) |
| **File Storage** | Stores uploaded course content files (videos, PDFs, documents), assignment submissions | Content Service, Assignment Service | FR-011, FR-015, FR-027 |
| **IndexedDB (Client-side)** | Stores downloaded course content and cached data on user's device for offline access | Service Worker, Web Client | FR-032, FR-033 |

## Component Interaction Flow

### Course Creation Flow (Instructor)
1. Web Client → API Gateway → Authentication Service (validate instructor role)
2. Web Client → API Gateway → Course Service (create course)
3. Course Service → Database (persist course data)
4. Web Client → API Gateway → Content Service (upload lessons)
5. Content Service → File Storage (store files)
6. Content Service → Database (store metadata)

### Student Learning Flow (Online)
1. Web Client → API Gateway → Authentication Service (validate student)
2. Web Client → API Gateway → Enrollment Service (verify enrollment)
3. Web Client → API Gateway → Content Service (fetch lesson)
4. Content Service → File Storage (retrieve file)
5. Web Client → API Gateway → Progress Tracking Service (mark complete)
6. Progress Tracking Service → Database (update progress)

### Offline Download Flow
1. Web Client → API Gateway → Content Service (request course content)
2. Content Service → File Storage (retrieve all lesson files)
3. Service Worker → IndexedDB (cache content locally)
4. Web Client displays offline-available indicator

### Offline Sync Flow
1. Service Worker detects internet connection restored
2. Service Worker → API Gateway → Progress Tracking Service (sync progress)
3. Progress Tracking Service → Database (update with offline progress)
4. Service Worker → Web Client (confirm sync complete)

### Assignment Submission Flow
1. Web Client → API Gateway → Assignment Service (submit assignment)
2. Assignment Service → File Storage (store submission file)
3. Assignment Service → Database (record submission metadata)
4. Web Client → API Gateway → Dashboard Service (instructor views submissions)
5. Web Client → API Gateway → Assignment Service (instructor grades)
6. Assignment Service → Database (store grade and feedback)