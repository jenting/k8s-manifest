# Deploy php-apache server

To demonstrate Horizontal Pod Autoscaler we will use a custom docker image based on the php-apache image. The Dockerfile has the following content:

```
FROM php:5-apache
ADD index.php /var/www/html/index.php
RUN chmod a+rx index.php
```

It defines an index.php page which performs some CPU intensive computations:

```
<?php
  $x = 0.0001;
  for ($i = 0; $i <= 1000000; $i++) {
    $x += sqrt($x);
  }
  echo "OK!";
?>
```

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

