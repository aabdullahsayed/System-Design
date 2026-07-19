# Kubernetes Basics

## What is it?
Kubernetes (K8s) is a container orchestration platform — it manages running, scaling, healing, and networking many containers across a CLUSTER of machines, automatically, based on a declarative configuration you provide.

## Why care?
While Docker runs individual containers, real production systems need MANY containers running across MANY machines, with automatic scaling, self-healing, and rolling deployments — Kubernetes is the dominant, industry-standard tool for this, and widely expected knowledge for backend/infrastructure-adjacent roles.

## Core concepts

### Pod — the smallest deployable unit
A Pod wraps one (or occasionally a few tightly-coupled) container(s), with shared networking/storage — you don't usually manage individual containers directly in Kubernetes, you manage Pods.

### Deployment — manages a set of identical Pod replicas
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3                          # run 3 identical copies
  selector:
    matchLabels: { app: myapp }
  template:
    metadata:
      labels: { app: myapp }
    spec:
      containers:
        - name: myapp
          image: myapp:v1.2.0
          ports: [{ containerPort: 8000 }]
```
This declares: "always keep 3 healthy replicas of this container running" — Kubernetes continuously reconciles reality to match this desired state, automatically restarting/replacing failed Pods without manual intervention.

### Service — stable networking for a set of Pods
Pods are ephemeral (they get replaced, get new IPs) — a Service provides a STABLE network endpoint that automatically routes to whichever healthy Pods currently exist, functioning as a built-in load balancer across your replicas.
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector: { app: myapp }
  ports: [{ port: 80, targetPort: 8000 }]
```

## Self-healing (a core, defining Kubernetes benefit)
If a Pod crashes, or the machine it's running on fails entirely, Kubernetes automatically detects this (via health checks, similar to load balancer health checks in `08-System-Design-Basics/Load-Balancing.md`) and starts a replacement Pod elsewhere in the cluster — without any human intervention, restoring the declared "3 replicas" state automatically.

## Horizontal Pod Autoscaling
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
spec:
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource: { name: cpu, target: { averageUtilization: 70 } }
```
Automatically scales the number of Pod replicas up/down based on real-time load (e.g., CPU utilization) — a direct, practical implementation of the horizontal scaling concepts in `08-System-Design-Basics/Scalability-Concepts.md`.

## Basic kubectl commands
```bash
kubectl get pods                    # list running pods
kubectl get deployments               # list deployments
kubectl logs <pod-name>                 # view a pod's logs
kubectl exec -it <pod-name> -- bash       # shell into a running pod
kubectl apply -f deployment.yaml            # apply/update configuration
kubectl rollout status deployment/myapp       # check a deployment's rollout progress
kubectl rollout undo deployment/myapp           # roll back to the previous version
```

## In backend engineering
- **Kubernetes is genuinely complex** — many smaller teams/companies are well-served by simpler platforms (managed container services like AWS ECS/Fargate, or PaaS options like Heroku/Render) rather than running/managing Kubernetes themselves. Knowing WHEN Kubernetes' complexity is actually justified (large-scale, multi-service, high-availability requirements) versus overkill is itself a meaningful engineering judgment call.
- **This is the practical foundation underlying microservices at scale** (`11-Microservices`) — Kubernetes is very commonly the actual runtime platform that many independently-deployed microservices run on in real production systems.
