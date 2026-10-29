# Security Policy

CityMove is designed with user safety, privacy, and infrastructure security at its core.

---

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |

---

## Reporting a Vulnerability

If you discover a security vulnerability in CityMove, please report it responsibly:

1. **Do not create public GitHub issues** for security vulnerabilities.
2. Email security concerns to `security@citymove.local` (or file a confidential GitHub Security Advisory).
3. Include detailed steps to reproduce the issue (proof-of-concept script, request payloads, or screenshots).
4. Our maintainers will acknowledge receipt within 48 hours and provide a timeline for triage and resolution.

---

## Built-in Security Safeguards

CityMove implements multi-layered security controls:

- **Strict Input Validation**: Every endpoint validates payloads with `zod` schemas. Text is sanitized against HTML/XSS injection.
- **SQL Injection Prevention**: 100% of SQL queries use parameterized prepared statements (`better-sqlite3` and `node:sqlite`).
- **HTTP Security Headers**: Express apps are protected with `helmet` (content security policy, frameguards, XSS filters).
- **Rate Limiting**: Protects against denial-of-service and bot spam using `express-rate-limit` with specialized tiers for report creation, voting, and transit beacon pings.
- **File Upload Safeguards**: Multer enforces max file size limits (5 MB) and MIME-type verification (JPEG, PNG, WebP only) before disk write.
- **Zero Location History**: Transit crowdsourcing stores only the single latest coordinate for an active session and automatically purges data older than 2 minutes.

