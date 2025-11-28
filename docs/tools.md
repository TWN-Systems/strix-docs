# Agent Tools

Strix agents have access to a suite of specialized tools for security testing. These tools enable comprehensive vulnerability discovery and validation.

## Available Tools

| Tool | Purpose |
|------|---------|
| **browser** | Multi-tab browser automation for XSS, CSRF, auth flow testing |
| **proxy** | HTTP proxy for request/response interception and manipulation |
| **terminal** | Interactive shell for command execution in sandbox |
| **python** | Python runtime for exploit development and validation |
| **file_edit** | Source code analysis and modification |
| **web_search** | Information gathering via web search |
| **notes** | Knowledge management and findings documentation |
| **reporting** | Vulnerability report generation |
| **agents_graph** | Multi-agent orchestration and coordination |
| **thinking** | Internal reasoning and planning |
| **finish** | Scan completion and result finalization |

## Tool Details

### Browser

Playwright-based browser automation for testing client-side vulnerabilities:
- Multi-tab support for complex attack flows
- Screenshot capture for evidence
- Cookie and session manipulation
- JavaScript execution
- Form interaction and submission

Requires a vision-capable LLM model for screenshot analysis.

### Proxy

Full HTTP/HTTPS proxy for traffic interception:
- Request/response modification
- Header manipulation
- Parameter tampering
- Replay attacks
- Traffic logging

### Terminal

Sandboxed shell environment:
- Command execution
- Tool installation (curl, wget, etc.)
- Network operations
- File system access within sandbox

### Python

Custom Python runtime:
- Exploit development
- Payload generation
- Response parsing
- Cryptographic operations
- Custom validation scripts

### File Edit

Source code analysis:
- Read and analyze code files
- Identify vulnerable patterns
- Track data flows
- Modify files for testing

### Web Search

External information gathering (requires `PERPLEXITY_API_KEY`):
- CVE research
- Technology fingerprinting
- Documentation lookup
- Known vulnerability patterns

### Notes

Internal knowledge management:
- Store findings during scan
- Track discovered endpoints
- Document attack paths
- Maintain context across agents

### Reporting

Vulnerability documentation:
- Generate structured findings
- Include proof-of-concept details
- Provide remediation guidance
- Export to standard formats

### Agents Graph

Multi-agent coordination:
- Spawn specialized sub-agents
- Coordinate parallel testing
- Share findings between agents
- Manage agent lifecycle

## Sandbox vs Non-Sandbox Mode

Some tools behave differently based on execution mode:

| Tool | Sandbox | Non-Sandbox |
|------|---------|-------------|
| terminal | Isolated container | Host system (caution) |
| file_edit | Container filesystem | Actual target files |
| reporting | Available | Available |
| agents_graph | Limited | Full access |

By default, Strix runs in sandbox mode using Docker for isolation.
