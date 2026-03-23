# API Security Risk Analysis Report

## Cyber Security Task 3 (2026)
**By Future Interns**

---

## Overview

This repository contains a comprehensive API Security Risk Analysis report for the **JSONPlaceholder REST API**, conducted as part of the Cyber Security Task 3 internship assignment. The assessment follows the OWASP API Security Top 10 framework and identifies critical security vulnerabilities that would be catastrophic in a production environment.

---

## Target API Information

| Property | Value |
|----------|-------|
| **API Name** | JSONPlaceholder |
| **Base URL** | https://jsonplaceholder.typicode.com |
| **Description** | Free fake REST API for testing and prototyping |
| **Protocol** | HTTPS |
| **Data Format** | JSON |
| **Hosting** | Cloudflare |
| **Assessment Date** | March 22, 2026 |

---

## Scope & Ethics

This assessment follows strict ethical guidelines:

### Allowed
- Read-only requests (GET, safe POST where allowed)
- Public demo API endpoints only
- Documentation-based analysis
- Header, token, and response inspection

### NOT Allowed
- Exploitation or bypass attempts
- Flooding / DoS testing
- Attacking private or production APIs

---

## Key Findings Summary

| Severity | Count | Description |
|----------|-------|-------------|
| **Critical** | 1 | Write Operations Without Authentication |
| **High** | 3 | No Auth, Data Exposure, No Authorization |
| **Medium** | 3 | Rate Limiting, Security Headers, Input Validation |
| **Low** | 1 | Server Information Disclosure |

**Total Findings:** 8  
**OWASP API Security Risks Detected:** 6/10

---

## Most Critical Finding

### API-006: Write Operations Allowed Without Authentication (CVSS: 9.1)

The API allows POST, PUT, and DELETE operations without any authentication, enabling complete data manipulation by anonymous users.

**Evidence:**
```
POST /posts → 201 Created (no auth required)
PUT /posts/1 → 200 OK (no auth required)
DELETE /posts/1 → 200 OK (no auth required)
```

**Impact:** Complete data integrity compromise. In a production API, this would be catastrophic.

---

## OWASP API Security Top 10 Coverage

| ID | Risk | Detected |
|----|------|----------|
| API1:2023 | Broken Object Level Authorization | ✓ YES |
| API2:2023 | Broken Authentication | ✓ YES |
| API3:2023 | Broken Object Property Level Authorization | ✓ YES |
| API4:2023 | Unrestricted Resource Consumption | ✓ YES |
| API5:2023 | Broken Function Level Authorization | ✓ YES |
| API6:2023 | Unrestricted Access to Business Flows | ✗ NO |
| API7:2023 | Server Side Request Forgery | ✗ NO |
| API8:2023 | Security Misconfiguration | ✓ YES |
| API9:2023 | Improper Inventory Management | ✗ NO |
| API10:2023 | Unsafe Consumption of APIs | ✗ NO |

---

## Detailed Findings

### Critical Severity

#### API-006: Write Operations Without Authentication
- **CVSS Score:** 9.1
- **OWASP:** API5:2023 - Broken Function Level Authorization
- **Description:** POST, PUT, DELETE operations allowed without authentication
- **Remediation:** Implement authentication for all write operations, use RBAC

### High Severity

#### API-005: No Authorization Checks for Resource Access
- **CVSS Score:** 8.2
- **OWASP:** API1:2023 - Broken Object Level Authorization
- **Description:** Any client can access any user's data by changing ID parameter
- **Remediation:** Implement object-level authorization checks

#### API-001: No Authentication Required
- **CVSS Score:** 7.5
- **OWASP:** API2:2023 - Broken Authentication
- **Description:** Complete access without any authentication
- **Remediation:** Implement API keys, OAuth 2.0, or JWT tokens

#### API-002: Excessive Data Exposure
- **CVSS Score:** 7.1
- **OWASP:** API3:2023 - Broken Object Property Level Authorization
- **Description:** Complete user objects with PII exposed (emails, phones, addresses)
- **Remediation:** Use DTOs, implement field-level authorization

### Medium Severity

#### API-008: No Input Validation
- **CVSS Score:** 6.5
- **Description:** API accepts arbitrary JSON without validation
- **Remediation:** Implement JSON Schema validation

#### API-003: Missing Rate Limiting
- **CVSS Score:** 5.3
- **Description:** Large data endpoints without pagination
- **Remediation:** Implement pagination with default limits

#### API-004: Missing Security Headers
- **CVSS Score:** 5.0
- **Description:** Missing HSTS, CSP, X-Frame-Options
- **Remediation:** Add security headers to all responses

### Low Severity

#### API-007: Server Information Disclosure
- **CVSS Score:** 3.7
- **Description:** Server header reveals Cloudflare platform
- **Remediation:** Remove or obfuscate Server header

---

## Tested Endpoints

| Method | Endpoint | Status | Auth Required |
|--------|----------|--------|---------------|
| GET | /users | 200 OK | No |
| GET | /users/1 | 200 OK | No |
| GET | /posts | 200 OK | No |
| GET | /comments | 200 OK | No |
| GET | /todos | 200 OK | No |
| POST | /posts | 201 Created | No |
| PUT | /posts/1 | 200 OK | No |
| DELETE | /posts/1 | 200 OK | No |

---

## Tools Used

- **Python** - For automated API testing and analysis
- **Requests Library** - For HTTP requests
- **Browser DevTools** - For manual inspection
- **JSONPlaceholder Documentation** - For endpoint discovery

---

## Repository Structure

```
api_github_repo/
├── README.md                          # This file
├── report/
│   └── api_security_report.pdf        # Full analysis report (18 pages)
├── test_evidence/
│   └── endpoint_test_results.json     # Raw test results
└── api_documentation/
    └── jsonplaceholder_endpoints.md   # API endpoint reference
```

---

## Report Contents

The full PDF report (18 pages) includes:

1. **Executive Summary** - Key findings and statistics
2. **Assessment Overview** - Scope, objectives, methodology
3. **OWASP API Security Top 10** - Coverage analysis
4. **Risk Summary** - All findings overview
5. **Detailed Findings** - Complete vulnerability breakdown
6. **API Endpoint Analysis** - Tested endpoints
7. **Recommendations** - Prioritized remediation steps
8. **Conclusion** - Key takeaways and references

---

## Recommendations Summary

### Immediate (24 hours)
1. Implement authentication for all endpoints
2. Restrict write operations to authenticated users
3. Enable authorization checks

### Short-Term (30 days)
1. Implement data filtering with DTOs
2. Add pagination to list endpoints
3. Add security headers
4. Implement input validation

### Medium-Term (90 days)
1. Implement rate limiting
2. Add request logging
3. Implement API versioning
4. Add monitoring and alerting

---

## Key Takeaways

1. **Authentication is fundamental** - Never allow access to sensitive data without authentication
2. **Authorization matters** - Verify users can only access their own resources
3. **Limit data exposure** - Only return necessary data for each use case
4. **Validate all input** - Never trust client-provided data
5. **Implement defense in depth** - Layer multiple security controls

---

## References

- [OWASP API Security Top 10:2023](https://owasp.org/API-Security/)
- [OWASP API Security Project](https://github.com/OWASP/API-Security)
- [API Security Checklist](https://github.com/shieldfy/API-Security-Checklist)
- [JSONPlaceholder API](https://jsonplaceholder.typicode.com/)

---

## Disclaimer

This assessment was conducted on the JSONPlaceholder demo API, which is intentionally designed for testing purposes. The security issues identified would be critical in a production environment but are expected in a demo API designed for learning.

---

**Prepared by:** Future Interns  
**Date:** March 22, 2026  
**Classification:** Security Assessment Document
