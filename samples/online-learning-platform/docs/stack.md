# Technology Stack

| Layer | Technology | Reason | Cost |
|-------|-----------|--------|------|
| **Frontend Framework** | React 18 | Industry-standard library with excellent PWA support, large community, extensive documentation, and strong ecosystem for building responsive interfaces. Hooks API simplifies state management for offline sync. | Free |
| **PWA Tooling** | Workbox 7 | Google's production-ready service worker library that simplifies offline caching strategies, background sync, and precaching. Handles complex offline scenarios with minimal code. | Free |
| **UI Component Library** | Material-UI (MUI) v5 | Comprehensive React component library with responsive design built-in, reducing development time. Provides consistent mobile and desktop experience with minimal custom CSS. | Free |
| **State Management** | React Context + Hooks | Built-in React solution sufficient for this scale, avoiding external dependencies. Manages user authentication state, course data, and offline sync status without additional libraries. | Free |
| **Client-side Storage** | IndexedDB (via Dexie.js) | Browser-native storage for large offline content. Dexie.js provides simple API over IndexedDB for storing downloaded course materials and progress data. | Free |
| **Backend Framework** | Node.js + Express.js | Lightweight, JavaScript-based backend enables code sharing with frontend. Express provides simple routing and middleware for RESTful API. Large ecosystem and excellent documentation for junior developers. | Free |
| **API Architecture** | RESTful API | Simple, well-understood pattern suitable for CRUD operations. Easy to document and test. Sufficient for this application's complexity without GraphQL overhead. | Free |
| **Authentication** | JSON Web Tokens (JWT) | Stateless authentication suitable for PWA architecture. Tokens stored client-side enable offline validation of user roles. Implemented via jsonwebtoken library. | Free |
| **Password Security** | bcrypt | Industry-standard password hashing with configurable salt rounds. Protects user credentials in database. | Free |
| **Database** | PostgreSQL 15 | Robust open-source relational database with excellent data integrity, ACID compliance for enrollment and progress tracking. Strong JSON support for flexible content metadata. Free tier available on most cloud providers. | Free |
| **File Storage** | Local Filesystem (development) / MinIO (production) | Development uses local disk storage. Production uses MinIO, an S3-compatible open-source object storage, enabling future migration to AWS S3 if needed without code changes. | Free |
| **File Upload Handling** | Multer | Express middleware for handling multipart/form-data file uploads. Simple integration for course content and assignment submissions. | Free |
| **Video Streaming** | HTTP Range Requests | Native browser support for video streaming via range requests. No external video platform needed. Enables seeking and bandwidth-efficient delivery. | Free |
| **API Documentation** | Swagger/OpenAPI 3.0 | Auto-generated API documentation from code annotations. Provides interactive testing interface for developers. | Free |
| **Development Server** | Vite 5 | Fast development server with hot module replacement. Optimized production builds with code splitting. Better performance than Create React App. | Free |
| **Testing Framework** | Jest + React Testing Library | Industry-standard testing tools for JavaScript. Jest for unit tests, RTL for component testing. Ensures code quality and catches regressions. | Free |
| **Code Quality** | ESLint + Prettier | Automated code formatting and linting. Enforces consistent style across team. Catches common errors before runtime. | Free |
| **Version Control** | Git | Standard version control for tracking changes and collaboration. | Free |
| **Deployment Platform** | Railway / Render (Free Tier) | Free hosting for small applications with PostgreSQL database included. Automatic deployments from Git. Sufficient for 50 concurrent users. Upgrade path available. | Free |
| **CDN (Optional)** | Cloudflare (Free Tier) | Optional CDN for static assets and media files. Improves load times globally. Free tier sufficient for starting out. | Free |
| **Monitoring (Optional)** | UptimeRobot (Free Tier) | Basic uptime monitoring with email alerts. Tracks service availability. Free tier monitors up to 50 endpoints. | Free |

## Development Tools

| Tool | Purpose | Cost |
|------|---------|------|
| **VS Code** | Primary code editor with excellent JavaScript/React support | Free |
| **Postman** | API testing and documentation during development | Free |
| **Chrome DevTools** | Debugging, performance profiling, PWA testing | Free |
| **Lighthouse** | PWA audit tool for performance and offline capability testing | Free |

## Deployment Architecture

### Development Environment
- Local Node.js server on port 3001 (backend)
- Vite dev server on port 5173 (frontend)
- Local PostgreSQL instance
- Local filesystem for file storage

### Production Environment
- Railway/Render hosting (single instance initially)
- Managed PostgreSQL database (included in free tier)
- MinIO container for file storage
- Cloudflare CDN (optional, for static assets)

## Justification for Stack Choices

**Cost Optimization**: Entire stack uses free and open-source technologies, meeting the budget constraint. Free hosting tiers (Railway/Render) provide sufficient resources for 50 concurrent users with upgrade path to paid tiers as user base grows.

**Offline Capability**: React + Workbox combination provides robust PWA functionality with service workers, enabling the critical offline download and sync requirements. IndexedDB handles large content storage client-side.

**Cross-Platform**: Single React codebase serves both mobile and desktop through responsive design, avoiding costly native app development. PWA can be installed on mobile devices for app-like experience.

**Developer-Friendly**: JavaScript across full stack reduces context switching. All technologies have extensive documentation and large communities, making it easier for junior developers to learn and find solutions.

**Scalability**: PostgreSQL and Node.js scale horizontally. Current architecture supports growth to 500+ users by adding server instances behind a load balancer and implementing caching layers.

**Security**: JWT authentication, bcrypt password hashing, and PostgreSQL's built-in security features protect student personal information and payment data as required.