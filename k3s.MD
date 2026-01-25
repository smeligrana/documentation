Esempio di sperimentazione locale su UBUNTU di creazione di un cluster formato da 1 Master e 2 Worker.

Installare k3d client

```
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

Creare il cluster
```
k3d cluster create lab-ubuntu \
    --servers 1 \
    --agents 2 \
    --port "8080:80@loadbalancer"

k3d cluster create mycluster \
  --volume /etc/iscsi:/etc/iscsi \
  --volume /sbin/iscsiadm:/sbin/iscsiadm \
  --volume /lib/modules:/lib/modules \
  --agents 2 \
  --port "8080:80@loadbalancer"
```

Comandi utili per la gestione del cluster:
```
k3d cluster start / stop lab-ubuntu
```

Comandi kubectl
```
kubectl get pods -n longhorn-system
kubectl logs longhorn-manager-dpkjk -n longhorn-system
kubectl delete pod -n longhorn-system -l app=longhorn-manager
```

Creazione nodi k3s su vm separate. Dopo aver predisposto il sistema operativo sulla vm.

```
#Crea il primo master node con etcd distribuito
curl -sfL https://get.k3s.io | K3S_TOKEN=token-cluster sh -s - server --cluster-init --node-ip=192.168.1.120

#Aggancia i master node 
curl -sfL https://get.k3s.io | K3S_TOKEN=token-cluster sh -s - server --server https://192.168.1.120:6443 --node-ip=192.168.1.121

#Aggancia nodi worker per l'esecuzione dei pod
curl -sfL https://get.k3s.io | sh -s - agent --server https://192.168.1.120:6443 --token token-cluster --node-ip=192.168.1.123
```

Per la gestione dello storage usiamo Longhorn che replica i dati su tutti i nodi, per installarlo su Ubuntu bisonga prima aver installato iSCSI e NFS
```
sudo apt install -y open-iscsi nfs-common
sudo systemctl enable --now iscsid
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.5.1/deploy/longhorn.yaml
```

Estrarre le informazioni per collegarsi al cluster
```
sudo cat /etc/rancher/k3s/k3s.yaml
```

Installazione ArgoCd

```
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
