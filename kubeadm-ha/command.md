#### install kubeadm
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF > /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y docker.io kubeadm
```

#### Deploy one master cluster:
```
kubeadm init --config kubeadm.yaml
```

#### Update cluster or deploy cluster with external ETCD servers 
``
kubeadm init --skip-preflight-checks --config kubeadm.yaml
```

#### install flannel
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel-rbac.yml
```

#### check certificate
```
openssl x509 -in apiserver.crt -text -noout
```
