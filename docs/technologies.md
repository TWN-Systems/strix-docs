# Supported Technologies

Strix has specialized testing modules for various frameworks, backend services, and protocols.

## Web Frameworks

| Framework | Module | Notes |
|-----------|--------|-------|
| **FastAPI** | `fastapi` | Starlette ASGI stack, dependency injection, OpenAPI |
| **Next.js** | `nextjs` | Full-stack React, API routes, server components |

### FastAPI Testing

The FastAPI module covers:
- Dependency injection vulnerabilities
- OpenAPI/Swagger exposure
- Starlette middleware issues
- Pydantic validation bypass
- Background task security

### Next.js Testing

The Next.js module covers:
- API route security
- Server-side rendering (SSR) issues
- Static generation vulnerabilities
- Middleware bypass
- Client/server boundary issues

## Backend Services

| Service | Module | Notes |
|---------|--------|-------|
| **Supabase** | `supabase` | PostgREST, Row Level Security, Auth, Storage |
| **Firebase** | `firebase_firestore` | Firestore, Authentication, Cloud Functions |

### Supabase Testing

- PostgREST API security
- Row Level Security (RLS) bypass
- GoTrue authentication flaws
- Storage bucket permissions
- Edge Functions security
- Realtime subscriptions

### Firebase Testing

- Firestore security rules
- Authentication bypass
- Cloud Functions vulnerabilities
- Storage bucket access
- Realtime Database rules

## Protocols

| Protocol | Module | Notes |
|----------|--------|-------|
| **GraphQL** | `graphql` | Query language vulnerabilities |

### GraphQL Testing

- Introspection exposure
- Query depth attacks
- Batching vulnerabilities
- Authorization bypass
- Field-level permissions
- N+1 query abuse

## Additional Protocols (Built-in)

Strix also tests these without dedicated modules:

- **REST APIs** - Standard HTTP API testing
- **WebSocket** - Real-time communication
- **OAuth2** - Authentication flows
- **JWT** - Token-based auth (via `authentication_jwt` module)

## Coming Soon

These technologies are planned but not yet implemented:

- **Django** - Python web framework
- **Express** - Node.js framework
- **Spring Boot** - Java framework
- **AWS** - Cloud services
- **Azure** - Cloud services
- **GCP** - Cloud services
- **Kubernetes** - Container orchestration

## Requesting New Technologies

Request support for new technologies via [GitHub Issues](https://github.com/usestrix/strix/issues) or contribute modules via [Pull Requests](https://github.com/usestrix/strix/pulls).
