# Actual Budget

## Creating SSL Cert

Generate certs in tailscale service pod:

```
tailscale cert <domain>
```

Copy certs out of pod and seal with kubeseal:

```
kubectl create secret generic -n misc actual-budget-certs --from-file=cert.crt --from-file=cert.key --dry-run=client -o yaml > secret.yaml
kubeseal -f secret.yaml -w mysealedsecret.yaml --format yaml --controller-namespace sealed-secrets
```
