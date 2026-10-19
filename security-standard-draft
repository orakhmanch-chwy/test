# Security Standard

Status: Draft for review. Owner: Oleg Rakhmanchik, as identified in the [tech standards ownership guidance](https://chewyinc.atlassian.net/wiki/spaces/PE2/pages/5780473693/Tech+Standards+Gaps+Ownership+and+Evolution). Effective date and approvers: pending approval.

## Summary

This standard defines the minimum security requirements for building, deploying, and operating Chewy applications and their supporting infrastructure. Its intent is to prevent unauthorized access, protect data, limit exposure, and make security controls verifiable throughout the service lifecycle. The [Security Requirements checklist](https://chewyinc.atlassian.net/wiki/spaces/SEC/pages/5736498994/Security+Requirements) provides the detailed review checks.

## In and out of scope

- **In scope:** Chewy-managed applications, APIs, integrations, AI-enabled features, and their cloud infrastructure, identities, data stores, build pipelines, and telemetry in every environment.
- **In scope:** Third-party integrations and the access, configuration, and data flows Chewy controls.
- **Out of scope:** Corporate endpoint security, physical security, and vendor-internal controls that Chewy does not manage.
- **Out of scope:** Language conventions, general API design, and detailed platform, database, and CI/CD implementation guidance covered by their respective standards; their security requirements still apply.

## Rules

MUST and MUST NOT are mandatory upon approval. SHOULD and SHOULD NOT express recommendations; deviations require a documented rationale. A nonapplicable requirement needs a recorded explanation, not an exception. A deviation from a mandatory requirement needs an approved exception.

### Ownership and design

- Teams MUST identify service and data owners and maintain architecture diagrams, data flows, endpoints, integrations, and resource inventories with required owner, application, and environment tags.
- Teams MUST complete required security, privacy, and [architecture reviews](https://chewyinc.atlassian.net/wiki/spaces/CAG/pages/2081751286/Architecture+Review+Team+ART) before launch or material changes. PII processing MUST have a Privacy Impact Assessment and a documented, approved business purpose; new uses require reassessment.
- Teams SHOULD document trust boundaries and abuse cases during design, particularly for public endpoints, sensitive data, privileged operations, and AI-enabled features.

### Identity and secrets

- Workforce access MUST use Okta SSO. Service-to-service access MUST authenticate using patterns approved by the authentication team.
- Access, integrations, IAM permissions, and role trust policies MUST be limited to required operations, resources, and environments. Owners MUST periodically review access and promptly remove unused accounts and permissions.
- Workloads MUST use workload roles and temporary AWS credentials instead of long-lived AWS access keys. Required static credentials MUST rotate at least every 90 days.
- Secrets MUST be stored in AWS Secrets Manager, isolated by application and environment, and automatically rotated where supported. Unsupported rotation mechanisms require an approved alternative. Secrets MUST NOT be embedded in source code, images, or client applications.

### Application and API protection

- Applications MUST authorize every protected request, object access, and administrative operation on the server and fail closed. Tests MUST cover unauthorized access and business-logic abuse, including cross-user access and privilege escalation.
- Applications MUST validate inputs server-side, use parameterized queries, and apply context-aware output encoding and safe templates. Untrusted input MUST NOT be inserted directly into executable JavaScript or HTML.
- Applications MUST enforce request-body, header, and upload limits; reject oversized requests; and validate and malware-scan untrusted uploads before making them available for use.
- Server-side outbound requests MUST be restricted to approved destinations and protocols. Redirects and resolved addresses MUST NOT bypass those restrictions.
- JWT validation MUST verify the signature, allowed algorithm, issuer, audience, and expiration and reject failures. Sessions MUST expire, enforce timeouts, and invalidate on logout; session IDs MUST rotate after login. Session cookies MUST use Secure, HttpOnly, and an appropriate SameSite setting.
- Login, registration, and recovery flows MUST prevent account enumeration through generic responses, consistent status codes, and comparable timing. Login, recovery, sensitive actions, and expensive API calls MUST be rate-limited.
- Nonpublic API documentation, schemas, and GraphQL introspection MUST require appropriate access. Public schemas MUST NOT disclose secrets, internal infrastructure details, or administrative operations. Hiding schemas MUST NOT replace authorization for queries or mutations.
- Client-facing errors MUST NOT expose stack traces or sensitive implementation details. Anonymous management access MUST be limited to minimal health information; unnecessary debug endpoints and heap dumps MUST be disabled.
- Webhooks MUST use HTTPS and carry only required, approved data, excluding credentials and secrets. HMAC-signed webhooks MUST have their signature verified against the raw body before processing; missing or invalid signatures MUST be rejected.
- Sensitive operations such as payments and refunds MUST resist replay and duplicate execution. Teams MUST use atomic idempotency for duplicate processing and appropriate freshness or replay checks for signed messages; tests MUST cover retries and concurrent requests.
- AI-enabled features MUST mitigate prompt injection and prevent model output from bypassing authorization or approved data access. Teams SHOULD assess LLM observability needs, including [Arize](https://chewyinc.atlassian.net/wiki/spaces/O11y/pages/3725690130/User+QuickStart+Guide+-+Arize+LLM+Observability), without exposing sensitive prompts or responses.

### Platform and network protection

- Workloads MUST use approved AWS accounts and infrastructure patterns. Production and nonproduction MUST have separate access, credentials, and connectivity; lower environments MUST NOT access higher environments.
- Databases, backend services, and administrative interfaces MUST remain private. External application traffic MUST enter through approved gateways, with direct access that bypasses security controls blocked.
- Security groups, firewalls, and SDP policies MUST allow only approved sources, destinations, ports, protocols, and identities where supported, and deny other traffic by default.
- Public web applications and HTTP APIs MUST use approved WAF, DDoS, and bot protections. Other public protocols MUST have security-reviewed exposure and appropriate network protections.
- Workloads MUST use approved, supported, hardened images and runtimes, disable unnecessary services and privileges, and apply security updates within required remediation deadlines. EC2 instances MUST require IMDSv2 when instance metadata is enabled.
- AWS service access MUST use PrivateLink where available. S3 buckets MUST have encryption, lifecycle policies, and public access blocked.
- AWS resources and workloads MUST be onboarded to Wiz coverage. Unsupported resources or coverage gaps MUST have documented alternative controls and approved exceptions.

### Data protection and observability

- Teams MUST [classify data and maintain ownership, metadata, and retention records](https://chewyinc.atlassian.net/wiki/spaces/DBS/pages/5597593820/Data+Governance+Standards). Collection and sharing MUST be limited to approved business needs and minimize PII; teams SHOULD use opaque identifiers.
- Data MUST be encrypted in transit using TLS 1.2 or later and at rest, including internal connections, volumes, databases, and backups. TLS certificate validation MUST remain enabled; expiration monitoring and renewal MUST be automated.
- AWS encryption keys MUST use KMS with restricted access and documented rotation and recovery procedures.
- Nonproduction MUST use synthetic or de-identified test data unless production data use is approved and equivalently protected.
- Source code, URLs, errors, logs, and telemetry, including Dynatrace, MUST NOT contain secrets or unnecessary sensitive data. Approved sensitive-data processing MUST be minimized and protected; diagnostics MUST redact credentials, tokens, and sensitive personal data.
- Applications MUST produce required security, privacy, and compliance audit trails. Security failures MUST be logged with correlation identifiers, without exposing sensitive data.
- Required data MUST have automated backups protected against unauthorized deletion. Teams MUST test restoration against documented recovery objectives.

### Delivery and ongoing assurance

- Workloads and infrastructure MUST be deployed through [approved standardized CI/CD platforms](https://chewyinc.atlassian.net/wiki/spaces/RELENG/pages/5418354036/Build+and+Deploy+Standards+CI+CD+Standards). Required reviews and security checks MUST NOT be bypassed.
- Teams MUST address CodeQL, Dependabot, secret-scanning, and Wiz findings before launch or obtain approved exceptions. Ongoing findings MUST have remediation owners and due dates under the approved vulnerability policy; the conflicting checklist deadlines must be resolved before this standard is approved.
- Teams MUST retain applicable checklist results as Yes, No, or N/A, with evidence links and exception references. Evidence MUST include relevant configuration, test results, and scan results, not only an assertion of compliance.
- Teams SHOULD automate repeatable controls in shared libraries, platform configurations, infrastructure policy checks, and CI/CD gates.
- New systems and material changes MUST meet applicable requirements before release. Existing systems MUST have an owner-assigned gap assessment and an approved remediation plan; existing security obligations remain in effect.
- Exceptions MUST record the affected requirement, rationale, risk, compensating controls, accountable owner, authorized approver, expiry, and remediation plan. An expired exception MUST NOT be treated as approval.
- The standard owner MUST coordinate review at least annually and after material changes to threats, platforms, or governing policies, and explicitly approve revisions or retirement through the standards approval process.
