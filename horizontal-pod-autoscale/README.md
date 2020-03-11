# Deploy php-apache server

```
kubectl apply -f php-apache.yaml
```

# Create Horizontal Pod Autoscaler

```
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
kubectl get hpa
```

# Increase load

```
kubectl run --generator=run-pod/v1 -it --rm load-generator --image=busybox /bin/sh
while true; do wget -q -O- http://php-apache.kube-system.svc.cluster.local; done
```

# Monitoring

```
kubectl get hpa
kubectl get deployment php-apache
```

Ref to [link](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough).

