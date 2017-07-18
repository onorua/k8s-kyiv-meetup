
Start up minikube 
```
minikube start --vm-driver=virtualbox --kubernetes-version=v1.7.0 --extra-config kubelet.EnableCustomMetrics=true --extra-config controller-manager.HorizontalPodAutoscalerUseRESTClients=true
```

Create namespace and access rights
```
kubectl create ns custom-metrics
kubectl create clusterrolebinding allowall-cm --clusterrole custom-metrics-server-resources --user system:anonymous
```

create certificate for custom certificate 
```
cd ~/.minikube
kubectl create  -n custom-metrics secret generic cm-adapter-serving-certs --from-file=ca.pem
cd - 
```

Create prometheus, we need operator first
```
kubectl apply -f prometheus-operator.yaml
watch -n2 'kubectl get po'
```

as soon as we got prometheus operator running, we are ready to start prometheus server itself
```
kubectl apply -f sample-prometheus-instance.yaml
watch -n2 'kubectl get po'
```

As soon as prometheus is ready, we are ready to start custom-metrics API server:
```
kubectl apply -f custom-metrics.yaml
watch -n2 'kubectl get -n custom-metrics po'
```

As soon as API server is ready, we are able to see custom-metrics API version:
```
kubectl api-versions | grep custom-metrics
```

No we are ready to start custom metrics app:
```
kubectl apply -f sample-metrics-app.yaml
```

check that prometheus has necessary metrics:
```
minikube service sample-metrics-prom
```

Check custom metrics API server logs:
```
kubectl -n custom-metrics get po 
kubectl -n custom-metrics logs {one from output}
```

