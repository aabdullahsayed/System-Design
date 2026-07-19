# Practice — DevOps and Deployment

1. Write a Dockerfile for a simple API (Python/Flask, Node/Express, or your framework of choice), build it, and run it locally with `docker run`.
2. Write a `docker-compose.yml` that runs your app alongside a PostgreSQL and Redis container, networked together — confirm your app can connect to both.
3. Set up a basic CI pipeline (GitHub Actions is free and beginner-friendly) that runs your test suite automatically on every push.
4. Extend the pipeline to also build a Docker image on every push to `main`.
5. Move any hardcoded configuration/secrets in your project into environment variables, loaded via a `.env` file locally (added to `.gitignore`).
6. Research and write 3-4 sentences comparing Rolling, Blue-Green, and Canary deployment strategies — which would you choose for a high-traffic payment API, and why?
7. If you have access to a Kubernetes cluster (or Minikube/Kind locally): deploy your Docker image as a Kubernetes Deployment with 2 replicas, and expose it via a Service.

✅ Done? Move to `11-Microservices`.
