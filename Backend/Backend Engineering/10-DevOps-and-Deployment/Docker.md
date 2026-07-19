# Docker

## What is it?
Docker packages an application and everything it needs to run (code, dependencies, runtime, system libraries) into a **container** — a lightweight, portable, isolated unit that runs consistently across any environment (your laptop, a colleague's machine, a production server).

## Why care?
"It works on my machine" is a genuinely common, real problem Docker directly solves — and containerization is now the standard baseline for deploying essentially any modern backend service.

## Images vs Containers
```
Image:     a read-only TEMPLATE (like a class) — your app + dependencies + OS libraries, built once
Container: a RUNNING INSTANCE of an image (like an object) — you can run many containers from one image
```

## A basic Dockerfile
```dockerfile
FROM python:3.11-slim              # start from a base image

WORKDIR /app                         # set working directory inside the container

COPY requirements.txt .                # copy dependency list first (for layer caching — see below)
RUN pip install -r requirements.txt      # install dependencies

COPY . .                                   # copy the rest of the application code

EXPOSE 8000                                  # document which port the app listens on

CMD ["python", "app.py"]                       # command to run when the container starts
```

## Building and running
```bash
docker build -t myapp:latest .          # build an image from the Dockerfile
docker run -p 8000:8000 myapp:latest      # run it, mapping host port 8000 to container port 8000
docker ps                                    # list running containers
docker logs <container_id>                     # view a container's logs
docker exec -it <container_id> bash              # open a shell INSIDE a running container (debugging)
```

## Why layer ordering matters (a real performance detail)
Docker caches each instruction ("layer") in a Dockerfile — if a layer's inputs haven't changed, Docker reuses the cached result instead of re-running it. This is why `COPY requirements.txt .` + `RUN pip install` comes BEFORE `COPY . .` in the example above: your application code changes far more often than your dependency list, so this ordering means dependency installation (the slow step) is only re-run when dependencies actually change, not on every single code change.

## Docker Compose — running multiple containers together
```yaml
# docker-compose.yml
version: '3'
services:
  app:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db
      - redis
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: secret
  redis:
    image: redis:7
```
```bash
docker-compose up      # starts app + db + redis together, networked so they can reach each other by service name
```
This is exactly how most local development environments replicate a realistic multi-service production setup (app + database + cache) with a single command.

## In backend engineering
- **Consistency across environments**: the exact same container image that passes tests in CI is the one deployed to production — eliminating an entire class of "worked in testing, broke in prod" environment-mismatch bugs.
- **This is the foundation for Kubernetes** (`Kubernetes-Basics.md`) — Kubernetes orchestrates many containers across many machines; Docker (or a compatible container runtime) is what actually runs each individual container.
- **Multi-stage builds** (a more advanced Dockerfile technique) let you use a larger "build" image with full compiler toolchains, then copy only the final compiled artifacts into a much smaller final "runtime" image — reducing production image size and attack surface.
