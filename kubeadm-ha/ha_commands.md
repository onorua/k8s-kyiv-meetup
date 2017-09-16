master node 1
```
docker stop etcd && docker rm etcd
rm -rf /var/lib/etcd-cluster
mkdir -p /var/lib/etcd-cluster
docker run -d \
--restart always \
-v /etc/ssl/certs:/etc/ssl/certs \
-v /var/lib/etcd-cluster:/var/lib/etcd \
-p 2380:2380 \
-p 2379:2379 \
--name etcd \
gcr.io/google_containers/etcd-amd64:3.0.17 \
etcd --name=etcd0 \
--advertise-client-urls=http://138.197.6.255:2379 \
--listen-client-urls=http://0.0.0.0:2379 \
--initial-advertise-peer-urls=http://138.197.6.255:2380 \
--listen-peer-urls=http://0.0.0.0:2380 \
--initial-cluster-token=9477af68bbee1b9ae037d6fd9e7efefd \
--initial-cluster=etcd0=http://138.197.6.255:2380,etcd1=http://174.138.74.173:2380,etcd2=http://174.138.70.103:2380 \
--initial-cluster-state=new \
--auto-tls \
--peer-auto-tls \
--data-dir=/var/lib/etcd
```

master node 2
```
docker stop etcd && docker rm etcd
rm -rf /var/lib/etcd-cluster
mkdir -p /var/lib/etcd-cluster
docker run -d \
--restart always \
-v /etc/ssl/certs:/etc/ssl/certs \
-v /var/lib/etcd-cluster:/var/lib/etcd \
-p 2380:2380 \
-p 2379:2379 \
--name etcd \
gcr.io/google_containers/etcd-amd64:3.0.17 \
etcd --name=etcd1 \
--advertise-client-urls=http://174.138.74.173:2379 \
--listen-client-urls=http://0.0.0.0:2379 \
--initial-advertise-peer-urls=http://174.138.74.173:2380 \
--listen-peer-urls=http://0.0.0.0:2380 \
--initial-cluster-token=9477af68bbee1b9ae037d6fd9e7efefd \
--initial-cluster=etcd0=http://138.197.6.255:2380,etcd1=http://174.138.74.173:2380,etcd2=http://174.138.70.103:2380 \
--initial-cluster-state=new \
--auto-tls \
--peer-auto-tls \
--data-dir=/var/lib/etcd
```

master node 3
```
docker stop etcd && docker rm etcd
rm -rf /var/lib/etcd-cluster
mkdir -p /var/lib/etcd-cluster
docker run -d \
--restart always \
-v /etc/ssl/certs:/etc/ssl/certs \
-v /var/lib/etcd-cluster:/var/lib/etcd \
-p 2380:2380 \
-p 2379:2379 \
--name etcd \
gcr.io/google_containers/etcd-amd64:3.0.17 \
etcd --name=etcd2 \
--advertise-client-urls=http://174.138.70.103:2379 \
--listen-client-urls=http://0.0.0.0:2379 \
--initial-advertise-peer-urls=http://174.138.70.103:2380 \
--listen-peer-urls=http://0.0.0.0:2380 \
--initial-cluster-token=9477af68bbee1b9ae037d6fd9e7efefd \
--initial-cluster=etcd0=http://138.197.6.255:2380,etcd1=http://174.138.74.173:2380,etcd2=http://174.138.70.103:2380 \
--initial-cluster-state=new \
--auto-tls \
--peer-auto-tls \
--data-dir=/var/lib/etcd
```

copy all from master01 to the rest of your masters:
```
scp -r /etc/kubernetes root@{ip}/etc/
```

replace IP
```
cd /etc/kubernetes/
perl -i -pe 's/138.197.6.255/174.138.74.173/g' $(ls -1 *conf)
```

start up kubelet
```
systemctl start kubelet
```

Copy kubectl config
```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```
