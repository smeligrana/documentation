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
