# Questão 16

Adicionar mais um node no cluster.

## Configuração Previa
0. criar nova máquina na AWS:
```bash
 alias k=kubectl
 alias kgp="k get pods"
 alias kcns="k create ns"
 alias kgns="k get ns"
 alias kgtx="k config get-contexts"
 alias kctx="k config set-context --current --namespace"
 complete -F __start_kubectl k
```

## Criação do Namespace e Definição Contexto 
1. Crie o namespace `q16-ns`.
```bash
kcns q16-ns
ou
kubectl create namespace q16-ns
```
2. Mude o contexto para o namespace `q16-ns`, criado no passo anterior.
```bash
kctx q16-ns
ou
kubectl config set-context --current --namespace q16-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução
4. 
```bash
    sudo hostname k8sworker03
    vi /etc/hostname
    curl -fsSL https://get.docker.com | bash

    cat > /etc/docker/daemon.json <<EOF
  {
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
  }
EOF
```
5. 
```bash
sudo mkdir -p /etc/systemd/system/docker.service.d

systemctl daemon-reload
systemctl restart docker
docker info | grep -i cgroup
swapoff -a
apt-get update && apt-get install -y apt-transport-https gnupg2
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list
apt-get update
apt-get install -y kubelet kubeadm kubectl

    kubeadm token create --print-join-command
    kubeadm join 172.31.44.232:6443 --token 7ul7e2.0wqfdfy5bsfgxvzk     --discovery-token-ca-cert-hash sha256:1d96aa02f0bd491e04874af76d973a192105aead172a04e7e234973bc96fdb6d 
```


## Testando a solução
7. 
```bash
    
```


## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
    kubectl get nodes -o wide
    kubectl drain k8sworker03 --delete-emptydir-data --force --ignore-daemonsets
    kubectl get nodes
```
12. Acesse o worker que será removido do ambiente (Caso necessário)
```bash
    kubeadm reset
```
12. Acesse o master do ambiente (Caso necessário)
```bash
    kubectl delete node k8sworker03
    kubectl get nodes
```

13. Valide que nenhum artefato está presente no namespace `q16-ns`
```bash
    kubectl get all -A | grep -i q16-ns
```