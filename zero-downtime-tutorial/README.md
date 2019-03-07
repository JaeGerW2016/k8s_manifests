### Build the images
```
docker build -t 314315960/zero-downtime-tutorial:blue -f dockerfile-blue .
docker build -t 314315960/zero-downtime-tutorial:green -f dockerfile-green .
```

### Creat the application

```
kubectl apply -f myapp.yaml
kubectl apply -f myapp-svc.yaml
kubectl apply -f myapp-without-hook.yaml
kubectl apply -f myapp-without-hook-svc.yaml
```
