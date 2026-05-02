# System Architecture

## System Classification
Progressive Web Application (PWA) with offline-first architecture, supporting both mobile and desktop access through responsive web design with service worker-based content caching and background synchronization.

## Architecture Pattern
**Progressive Web App (PWA) with Client-Server Architecture**

### Justification
The PWA architecture is optimal for this learning platform because it provides native app-like offline functionality through service workers while maintaining cross-platform compatibility (mobile and desktop) without separate codebases. The offline-first approach with service workers enables students to download and access course content without internet, then sync progress when reconnected—directly addressing the offline requirement (FR-031 to FR-035). A traditional client-server model with RESTful API backend provides clear separation between content delivery and business logic, making it easier to scale from 50 to 500+ users. This architecture supports the low-cost constraint by using open source technologies and avoiding expensive native app development or complex microservices infrastructure.

### Trade-offs
- **Storage Limitations**: Browser storage quotas (typically 50MB-100MB) may limit how much content students can download offline. Large video files may require streaming-only access or compressed versions for offline use.
- **Browser Dependency**: Offline functionality relies on modern browser support for service workers and IndexedDB. Older browsers (IE11, older mobile browsers) will have degraded offline experience.
- **Sync Complexity**: Conflict resolution when multiple devices sync progress for the same student requires careful handling. The system will use "last write wins" strategy with timestamps, which may occasionally overwrite progress if students use multiple devices simultaneously.
- **Initial Load Time**: First-time users must download the PWA shell and cache assets, which may take 10-30 seconds on slow connections. Subsequent visits will be instant.

### Scalability Path
Current architecture supports 50 concurrent users on a single server instance. To scale to 500+ users:
1. Add horizontal scaling with load balancer distributing traffic across multiple application server instances
2. Implement CDN for static content and media files to reduce server load
3. Add database read replicas for improved query performance
4. Implement Redis caching layer for frequently accessed data (course lists, user sessions)
5. Move file storage from local disk to cloud object storage (S3-compatible)

## Key Architectural Decisions

### Offline-First Design
Service workers intercept network requests and serve cached content when offline. Students explicitly download courses, which caches all lesson content locally. Progress tracking uses local storage with background sync API to push updates when online.

### Responsive Web Design
Single codebase serves both mobile and desktop through CSS media queries and flexible layouts. Mobile-first design ensures optimal experience on smaller screens while scaling up for desktop.

### RESTful API
Backend exposes RESTful endpoints for all operations (authentication, course management, content delivery, progress tracking). Frontend consumes API through fetch requests with JWT authentication.

### Role-Based Access Control
Middleware validates user roles (instructor vs student) on every API request. Frontend conditionally renders UI based on user role stored in JWT token.

### File Storage Strategy
Course content (videos, PDFs) stored on server filesystem with database storing metadata and file paths. For scalability, this can migrate to S3-compatible object storage without architecture changes.