# Environment Configuration

## What is it?
Managing settings that differ between environments (local development, staging, production) — database URLs, API keys, feature flags — WITHOUT hardcoding them into your application code.

## Why care?
Hardcoded configuration (especially secrets like API keys/database passwords) is a real, common security risk (accidentally committed to git, visible in source control history forever) AND an operational headache (can't easily run the same code against different environments). Proper config management solves both.

## The 12-Factor App principle: config in the environment
```python
import os

DATABASE_URL = os.environ["DATABASE_URL"]
API_KEY = os.environ["THIRD_PARTY_API_KEY"]
DEBUG = os.environ.get("DEBUG", "false").lower() == "true"
```
Configuration is read from environment variables (or a config service), NOT hardcoded — the exact same code/container image runs unmodified in every environment, just with different environment variables injected at runtime.

## `.env` files for local development
```
# .env (NEVER commit this file — add it to .gitignore)
DATABASE_URL=postgresql://localhost/myapp_dev
THIRD_PARTY_API_KEY=sk_test_abc123
DEBUG=true
```
```bash
# .gitignore
.env
```
Most frameworks/libraries (`python-dotenv`, Node's `dotenv`) load these automatically in development — while production environments typically inject real environment variables directly through the deployment platform (Kubernetes Secrets, cloud provider's environment variable configuration, etc.), never via a committed `.env` file.

## Secrets management (beyond basic env vars, for genuinely sensitive values)
For production-grade secrets (database passwords, private keys, third-party API secrets), dedicated secrets management tools provide additional security beyond plain environment variables:
```
AWS Secrets Manager / Parameter Store
HashiCorp Vault
Kubernetes Secrets
Azure Key Vault / Google Secret Manager
```
These typically offer: encryption at rest, fine-grained access control (which services can read which secrets), automatic rotation, and audit logging of secret access — meaningfully more robust than plain environment variables for genuinely sensitive production credentials.

## Never commit secrets to git (and what to do if you accidentally do)
```bash
# If a secret WAS accidentally committed:
# 1. Immediately rotate/invalidate that secret (assume it's compromised, even if you delete it from history)
# 2. Remove it from git history (git filter-repo, or BFG Repo-Cleaner) — but rotation is the real fix, not just hiding it
```
**The critical point**: simply deleting a secret from a later commit does NOT remove it from git history — anyone with repo access can still find it in an earlier commit. The only real fix once a secret is exposed is to rotate/invalidate it.

## Environment-specific configuration files (an alternative/complementary pattern)
```
config/
  development.yaml
  staging.yaml
  production.yaml
```
```python
env = os.environ.get("APP_ENV", "development")
config = load_config(f"config/{env}.yaml")
```
Useful for NON-secret configuration that varies meaningfully by environment (log levels, feature flag defaults, timeout values) — while genuinely sensitive secrets still flow through the dedicated secrets management approach above, not plain config files (even environment-specific ones risk being committed to git).

## In backend engineering
- **This is a foundational practice, not an advanced one** — proper environment configuration is one of the very first things a professional backend project sets up correctly, and getting it wrong (hardcoded secrets, no environment separation) is a common, real red flag in code review for junior engineers.
- **CI/CD pipelines** (`CI-CD.md`) inject environment-specific configuration/secrets automatically at deploy time, keeping the same tested container image portable across staging and production with only its configuration changing.
