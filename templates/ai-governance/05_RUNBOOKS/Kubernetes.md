# Runbook — Kubernetes

> Only applies if/when the project outgrows Compose (multiple hosts, autoscaling). Adopting K8s requires an ADR — it adds real operational cost.

## Inspect

```bash
kubectl get pods -A                     # everything running?
kubectl get deploy,svc,ingress -n <ns>
kubectl describe pod <pod> -n <ns>      # events = why it's broken
kubectl logs -f <pod> -n <ns> [--previous]
kubectl top pods -n <ns>                # resource usage
kubectl get events -n <ns> --sort-by=.lastTimestamp
```

## Operate

```bash
kubectl apply -f k8s/                   # deploy manifests
kubectl rollout status deploy/<app> -n <ns>
kubectl rollout undo deploy/<app> -n <ns>     # ROLLBACK
kubectl rollout restart deploy/<app> -n <ns>
kubectl scale deploy/<app> --replicas=3 -n <ns>
kubectl exec -it <pod> -n <ns> -- sh
kubectl port-forward svc/<app> 8080:80 -n <ns>
```

## Governance rules when on K8s

- Images pinned by tag/digest; `imagePullPolicy` not relying on `latest`
- Resource requests AND limits on every container
- Liveness + readiness probes wired to the Phase 08 health endpoint
- Secrets via Secret objects / external secret manager — never in manifests committed to git
- NetworkPolicies: DB reachable only from app pods
- PodDisruptionBudgets + ≥2 replicas for anything user-facing

## Project-specific

```
Cluster:    <where>
Namespace:  <ns>
Deploy:     <command / GitOps repo>
Rollback:   kubectl rollout undo deploy/<app> -n <ns>
```
