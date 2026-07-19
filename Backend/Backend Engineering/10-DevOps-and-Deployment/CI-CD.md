# CI/CD (Continuous Integration & Continuous Deployment)

## What is it?
**CI (Continuous Integration)**: automatically building and testing every code change as it's pushed, catching integration issues immediately rather than discovering them later. **CD (Continuous Deployment/Delivery)**: automatically deploying code that passes CI to staging/production, with minimal or no manual intervention.

## Why care?
Manual testing and deployment don't scale with team size or release frequency, and are genuinely error-prone (forgetting a step, deploying the wrong version). CI/CD is the standard, expected practice for any serious backend team, and directly enables shipping software faster and more safely.

## A typical CI pipeline
```yaml
# .github/workflows/ci.yml (GitHub Actions example)
name: CI
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Run linter
        run: flake8 .
      - name: Run tests
        run: pytest --cov=myapp
      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .
```
Every push/PR automatically triggers: install → lint → test → build — if ANY step fails, the pipeline fails, and (typically) merging is blocked until it's fixed. This is what "keeps main always in a working, deployable state" (referenced in `02-Git-and-Collaboration/Branching-Strategies.md`) actually enforced, automatically.

## Deployment strategies (minimizing risk when shipping to production)

### Rolling deployment
Gradually replace old instances with new ones, a few at a time — if something's wrong, only a fraction of traffic is affected while you catch and roll back.

### Blue-Green deployment
Run TWO complete, identical production environments ("Blue" = current live version, "Green" = new version). Deploy fully to Green, verify it's healthy, then switch ALL traffic over atomically — enables instant rollback (just switch back to Blue) if something's wrong.

### Canary deployment
Release the new version to a SMALL percentage of real traffic first (e.g., 5%), monitor closely for errors/performance regressions, then gradually increase to 100% if healthy — limits the "blast radius" of a bad deploy to a small fraction of real users before it affects everyone.

## Feature flags (decoupling deployment from release)
```python
if feature_flags.is_enabled("new_checkout_flow", user):
    return new_checkout_flow()
else:
    return old_checkout_flow()
```
Lets you deploy new code to production WITHOUT immediately activating it for all users — enabling gradual rollout, instant "kill switch" rollback (toggle the flag off, no redeploy needed), and A/B testing, decoupled entirely from the deployment process itself.

## CD pipeline example (extending the CI pipeline above)
```yaml
  deploy:
    needs: test          # only runs if the test job succeeded
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        run: |
          docker push myapp:${{ github.sha }}
          kubectl set image deployment/myapp myapp=myapp:${{ github.sha }}
```

## In backend engineering
- **CI/CD is what makes small, frequent deploys safe and normal** — rather than large, risky, infrequent "release day" deployments, teams with mature CI/CD can ship dozens of small, individually low-risk changes per day, each automatically tested and gradually rolled out.
- **Rollback speed matters as much as deployment speed**: a genuinely mature CI/CD setup makes "revert to the previous known-good version" fast and routine, turning bad deploys into brief, low-drama incidents rather than crises.
