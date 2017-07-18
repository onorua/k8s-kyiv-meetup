Create minikube machine
```
minikube start --vm-driver=virtualbox --kubernetes-version=v1.7.0 --extra-config kubelet.EnableCustomMetrics=true --extra-config controller-manager.HorizontalPodAutoscalerSyncPeriod.Duration=5s 
```

enable heapster
```
minikube addons enable heapster
watch -n2 'kubectl get po --all-namespaces'
```

create apache deployment
```
kubectl run php-apache --image=gcr.io/google_containers/hpa-example --requests=cpu=200m --expose --port=80
watch -n2 'kubectl get po'
```

create autoscaler
```
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
```

check how is our hpa 
```
watch -n2 'kubectl describe hpa php-apache'
```

As soon as we got `0` in calculated, we can put some load:
```
kubectl run -i --tty load-generator --image=busybox /bin/sh
while true; do wget -q -O- http://php-apache.default.svc.cluster.local; done
```
