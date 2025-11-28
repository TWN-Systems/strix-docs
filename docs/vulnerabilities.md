# Vulnerability Types

Strix scans for a comprehensive range of vulnerability types. Each vulnerability has a specialized prompt module with advanced techniques, payloads, and validation methods.

## Vulnerability Categories

### Access Control

| Vulnerability | Module | Description |
|---------------|--------|-------------|
| **IDOR** | `idor` | Insecure Direct Object Reference (BOLA) - accessing resources by manipulating identifiers |
| **Broken Function Level Authorization** | `broken_function_level_authorization` | Accessing admin/privileged functions without proper authorization |

### Injection Attacks

| Vulnerability | Module | Description |
|---------------|--------|-------------|
| **SQL Injection** | `sql_injection` | Injecting SQL commands into queries |
| **Remote Code Execution** | `rce` | Command injection and code execution |
| **XXE** | `xxe` | XML External Entity injection |

### Server-Side Vulnerabilities

| Vulnerability | Module | Description |
|---------------|--------|-------------|
| **SSRF** | `ssrf` | Server-Side Request Forgery - making server issue requests |
| **Path Traversal** | `path_traversal_lfi_rfi` | Directory traversal, Local/Remote File Inclusion |
| **Insecure File Uploads** | `insecure_file_uploads` | Uploading malicious files |

### Client-Side Vulnerabilities

| Vulnerability | Module | Description |
|---------------|--------|-------------|
| **XSS** | `xss` | Cross-Site Scripting (Reflected, Stored, DOM) |
| **CSRF** | `csrf` | Cross-Site Request Forgery |
| **Open Redirect** | `open_redirect` | Redirecting users to malicious sites |

### Authentication & Session

| Vulnerability | Module | Description |
|---------------|--------|-------------|
| **JWT Vulnerabilities** | `authentication_jwt` | JWT signature bypass, algorithm confusion, claim manipulation |

### Business Logic

| Vulnerability | Module | Description |
|---------------|--------|-------------|
| **Business Logic Flaws** | `business_logic` | Workflow manipulation, state abuse |
| **Race Conditions** | `race_conditions` | Time-of-check to time-of-use (TOCTOU) |
| **Mass Assignment** | `mass_assignment` | Parameter pollution, unauthorized field modification |

### Information & Infrastructure

| Vulnerability | Module | Description |
|---------------|--------|-------------|
| **Information Disclosure** | `information_disclosure` | Sensitive data exposure, verbose errors |
| **Subdomain Takeover** | `subdomain_takeover` | Claiming abandoned subdomains |

## OWASP Alignment

Many vulnerabilities align with OWASP Top 10:

| OWASP Category | Strix Modules |
|----------------|---------------|
| A01: Broken Access Control | `idor`, `broken_function_level_authorization` |
| A02: Cryptographic Failures | `information_disclosure` |
| A03: Injection | `sql_injection`, `rce`, `xxe`, `xss` |
| A04: Insecure Design | `business_logic` |
| A05: Security Misconfiguration | `information_disclosure`, `subdomain_takeover` |
| A06: Vulnerable Components | (via web_search tool) |
| A07: Auth Failures | `authentication_jwt`, `csrf` |
| A08: Data Integrity Failures | `mass_assignment` |
| A09: Logging Failures | `information_disclosure` |
| A10: SSRF | `ssrf` |

## Enabling Specific Vulnerability Scans

Focus on specific vulnerabilities:

```bash
strix --target ./your-app --modules "sql_injection,xss,idor"
```

By default, Strix selects modules automatically based on the target.
