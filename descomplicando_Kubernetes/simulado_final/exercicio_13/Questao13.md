# Questão 13

EXTRA - Realizar o backup do etcd.

## Configuração Previa
0. alguns alias recomendados:
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
1. Crie o namespace `q13-ns`.
```bash
kcns q13-ns
ou
kubectl create namespace q13-ns
```
2. Mude o contexto para o namespace `q13-ns`, criado no passo anterior.
```bash
kctx q13-ns
ou
kubectl config set-context --current --namespace q13-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução
4. Listar os pods do namespace kube-system para poder identificar o pod que está rodando etcd
```bash
    kubectl get pods -n kube-system
```
5. Vizualise as informaç~eos do pod etcd
```bash
    kubectl describe pod etcd-k8smaster01 -n kube-system
    (...)
    --advertise-client-urls=https://172.31.44.232:2379
    --cert-file=/etc/kubernetes/pki/etcd/server.crt
    --key-file=/etc/kubernetes/pki/etcd/server.key
    --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    (...)
```
6. Essas são as varáveis mais importantes para poder interagir com o etcd
```bash
    --cert-file
    --key-file
    --trusted-ca-file
```

## Testando a solução
7. 
```bash
ETCD_VER=v3.4.7
GOOGLE_URL=https://storage.googleapis.com/etcd
GITHUB_URL=https://github.com/etcd-io/etcd/releases/download
DOWNLOAD_URL=${GOOGLE_URL}
rm -f /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
rm -rf /tmp/etcd-download-test && mkdir -p /tmp/etcd-download-test
curl -L ${DOWNLOAD_URL}/${ETCD_VER}/etcd-${ETCD_VER}-linux-amd64.tar.gz -o /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
tar xzvf /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz -C /tmp/etcd-download-test --strip-components=1
rm -f /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
/tmp/etcd-download-test/etcd --version
/tmp/etcd-download-test/etcdctl version
```
8. 
```bash
kubectl exec -it etcd-k8smaster01 -n kube-system \
-- etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
--cert=/etc/kubernetes/pki/etcd/server.crt get / --prefix --keys-only
```
9. Criar um arquivo yaml para o deployment 
```bash
    kubectl create -f https://k8s.io/examples/controllers/nginx-deployment.yaml --dry-run=client -o yaml > deployment-pods-q13-dry-run.yaml
```
10. Modifica-lo caso necessário
```bash
    cp deployment-pods-q13-dry-run.yaml deployment-pods-q13.yaml
    vi deployment-pods-q13.yaml  
```
11. Criar o deployment com a versão desejada do nginx
```bash
   kubectl create -f deployment-pods-q13.yaml
```
12. 
```bash
kubectl exec -it etcd-k8smaster01 -n kube-system \
-- etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
--cert=/etc/kubernetes/pki/etcd/server.crt get /registry/pods/default \
--prefix=true --keys-only
```
13. 
```bash
kubectl exec -it etcd-k8smaster01 -n kube-system \
-- etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
--cert=/etc/kubernetes/pki/etcd/server.crt get /registry/pods/default/nginx \
--prefix=true    
```
14. Faça o backup do etcd (no próprio Master)
```bash
   ETCDCTL_API=3 etcdctl \
--cacert /etc/kubernetes/pki/etcd/ca.crt  \
--key /etc/kubernetes/pki/etcd/server.key \
--cert /etc/kubernetes/pki/etcd/server.crt \
--endpoints https://[127.0.0.1]:2379  \
snapshot save /tmp/snapshot.db
```
15. Verifique o status do Snapshot (no próprio Master)
```bash
   ETCDCTL_API=3 etcdctl \
--cacert /etc/kubernetes/pki/etcd/ca.crt  \
--key /etc/kubernetes/pki/etcd/server.key \
--cert /etc/kubernetes/pki/etcd/server.crt \
--endpoints https://[127.0.0.1]:2379  \
--write-out=table \
snapshot status /tmp/snapshot.db
```

14. Faça o backup do etcd (dentro do pod)
```bash
kubectl exec -it etcd-k8smaster01 -n kube-system \
-- etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
--cert=/etc/kubernetes/pki/etcd/server.crt \
snapshot save snapshot.db
```
15. Verifique o status do Snapshot (dentro do pod)
```bash
kubectl exec -it etcd-k8smaster01 -n kube-system \
-- etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--write-out=table \
snapshot status snapshot.db
```

## Limpando ambiente (caso seja necessário)
15. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f deployment-pods-q13.yaml
```
16. Valide que nenhum artefato está presente no namespace `q13-ns`
```bash
    kubectl get all
    ou
    kubectl get all -A | grep -i q13-ns
```