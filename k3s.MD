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

Per la gestione dello storage usiamo Longhorn che replica i dati su tutti i nodi, per installarlo su Ubuntu bisonga prima aver installato iSCSI e NFS

```
sudo apt install -y open-iscsi nfs-common
sudo systemctl enable --now iscsid
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.5.1/deploy/longhorn.yaml
```
