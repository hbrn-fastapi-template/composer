# Kubernetes Manifests

Apply in order:

```bash
kubectl apply -f secrets.yaml
kubectl apply -f db.yaml
kubectl apply -f account.yaml
kubectl apply -f auth.yaml
kubectl apply -f gateway.yaml
```

Or all at once:

```bash
kubectl apply -f .
```

## Secret Configuration

Before deploying, replace placeholders in `secrets.yaml` or use:

```bash
kubectl create secret generic app-secrets \
  --from-literal=postgres-user=myuser \
  --from-literal=postgres-password=mypassword \
  --from-literal=postgres-db=mydb \
  --from-literal=database-url=jdbc:postgresql://db:5432/mydb \
  --from-literal=database-username=myuser \
  --from-literal=database-password=mypassword \
  --from-literal=jwt-secret-key=your-secret-key
```
