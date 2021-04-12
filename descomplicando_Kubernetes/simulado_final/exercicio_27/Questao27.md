# Questão 27

Criar um volume Emptydir e compartilhar entre 2 containers.

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
1. Crie o namespace `q27-ns`.
```bash
kcns q27-ns
ou
kubectl create namespace q27-ns
```
2. Mude o contexto para o namespace `q27-ns`, criado no passo anterior.
```bash
kctx q27-ns
ou
kubectl config set-context --current --namespace q27-ns
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
    kubectl create -f https://k8s.io/examples/pods/two-container-pod.yaml --dry-run=client -o yaml > pod-2containers-q27-dry-run.yaml
```
5. 
```bash
    cp pod-2containers-q27-dry-run.yaml pod-2containers-q27.yaml
```
6. 
```bash
    vi pod-2containers-q27.yaml
```
7. 
```bash
    kubectl create -f pod-2containers-q27.yaml
```

## Testando a solução
8. 
```bash
    kubectl get pods
```
9. 
```bash
    kubectl exec -it two-containers -c nginx-container -- /bin/bash
```
10. 
```bash
apt-get update
apt-get install curl procps
ps aux
```
11. 
```bash
curl localhost
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f pod-2containers-q27.yaml
```
13. Valide que nenhum artefato está presente no namespace `q27-ns`
```bash
    kubectl get all -A | grep -i q27-ns
```

## Referencia
14. https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/