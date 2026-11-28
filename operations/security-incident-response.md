# Security Incident Response Runbook

## 1. Severity Classification

- **SEV-1 (Critical):** Active credential leakage (`JWT_SECRET`, DB password, S3 keys, GitHub PAT), cross-tenant data access bypass, or unauthenticated RCE.
- **SEV-2 (High):** Privilege escalation requiring authenticated tenant user, fail-open integrity check, or persistent DoS against API/workers.
- **SEV-3 (Medium/Low):** Hardening gap, misconfigured non-default dev profile, or high/moderate dependency CVE without confirmed exploit path.

## 2. Immediate Containment

1. Freeze affected deployments or isolate compromised workers (`docker compose stop <service>`).
2. Capture volatile evidence (container logs, git SHA, `/metrics` snapshot) without printing raw secrets.
3. For suspected account or token compromise, run `bun run revoke:auth-sessions` to increment `token_version` and invalidate active refresh tokens.

## 3. Credential Rotation

Rotate secrets in order of blast radius:
1. `JWT_SECRET` and `MW_INTERNAL_SECRET` (must remain distinct; >=32 random chars).
2. `DATABASE_URL` credentials and `POSTGRES_PASSWORD`.
3. `ATTACHMENT_S3_ACCESS_KEY` / `ATTACHMENT_S3_SECRET_KEY` (Garage bucket keys).
4. `WHATSAPP_GATEWAY_PASSWORD`, `WHATSAPP_WEBHOOK_SECRET`, `WHATSAPP_CLOUD_API_ACCESS_TOKEN`.
5. `METRICS_TOKEN`, `OBSERVABILITY_INGEST_TOKEN`, and GitHub PATs / deploy keys.

## 4. Secret Scan & Baseline Hygiene

1. Run `gitleaks detect --source . --config .gitleaks.toml --redact` locally.
2. Never add real credentials to `.gitleaks-baseline.json` or `.gitleaks.toml`. Remove false-positive entries once remediated and keep `check-gitleaks-config.mjs` cap enforced.

## 5. Remediation, Verification & Disclosure

1. Patch root cause with negative regression tests (`bun run verify`).
2. Tag patched release, publish correction notice referencing affected commit/version, and notify impacted tenant operators if tenant data was exposed.
3. Record postmortem trigger, timeline, and preventive CI/static guard within 72 hours.
