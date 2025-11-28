# Prompt Modules

Prompt modules are specialized knowledge packages that enhance Strix agents with deep expertise in specific vulnerability types, technologies, and testing methodologies.

## How Modules Work

When an agent is created, it can load up to 5 specialized prompt modules relevant to the task. Modules are dynamically injected into the agent's system prompt, providing:

- Advanced techniques beyond baseline security knowledge
- Practical payloads and test cases
- Validation methods to confirm findings
- Context-specific insights and edge cases

## Module Categories

| Category | Count | Purpose |
|----------|-------|---------|
| **Vulnerabilities** | 17 | Core vulnerability classes (IDOR, XSS, SQLi, etc.) |
| **Frameworks** | 2 | Framework-specific testing (FastAPI, Next.js) |
| **Technologies** | 2 | Third-party services (Supabase, Firebase) |
| **Protocols** | 1 | Protocol-specific patterns (GraphQL) |
| **Cloud** | - | AWS, Azure, GCP (coming soon) |
| **Reconnaissance** | - | Information gathering (coming soon) |

## Available Modules

### Vulnerability Modules

| Module | Description |
|--------|-------------|
| `authentication_jwt` | JWT authentication vulnerabilities |
| `broken_function_level_authorization` | Function-level access control flaws |
| `business_logic` | Business logic flaws and workflow manipulation |
| `csrf` | Cross-Site Request Forgery |
| `idor` | Insecure Direct Object Reference (BOLA) |
| `information_disclosure` | Sensitive information exposure |
| `insecure_file_uploads` | File upload vulnerabilities |
| `mass_assignment` | Mass assignment/parameter pollution |
| `open_redirect` | Open redirect vulnerabilities |
| `path_traversal_lfi_rfi` | Path traversal and file inclusion |
| `race_conditions` | Race condition vulnerabilities |
| `rce` | Remote code execution |
| `sql_injection` | SQL injection attacks |
| `ssrf` | Server-Side Request Forgery |
| `subdomain_takeover` | Subdomain takeover |
| `xss` | Cross-Site Scripting |
| `xxe` | XML External Entity injection |

### Framework Modules

| Module | Description |
|--------|-------------|
| `fastapi` | FastAPI/Starlette testing techniques |
| `nextjs` | Next.js framework testing |

### Technology Modules

| Module | Description |
|--------|-------------|
| `firebase_firestore` | Firebase/Firestore backend testing |
| `supabase` | Supabase (PostgREST, RLS, Auth, Storage) |

### Protocol Modules

| Module | Description |
|--------|-------------|
| `graphql` | GraphQL-specific attack vectors |

## Using Modules

Modules are automatically selected by agents based on the target and findings. You can also specify modules explicitly:

```bash
strix --target ./your-app --modules "sql_injection,idor,authentication_jwt"
```

## Contributing Modules

Community contributions are welcome. See the [prompts README](https://github.com/usestrix/strix/tree/main/strix/prompts) for module structure and guidelines.

Submit new modules via [pull requests](https://github.com/usestrix/strix/pulls).
