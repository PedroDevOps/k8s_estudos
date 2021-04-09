# Questão 17

Adicionar um label no node.

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
1. Crie o namespace `q17-ns`.
```bash
kcns q17-ns
ou
kubectl create namespace q17-ns
```
2. Mude o contexto para o namespace `q17-ns`, criado no passo anterior.
```bash
kctx q17-ns
ou
kubectl config set-context --current --namespace q17-ns
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
    kubectl get nodes --show-labels
```
5. 
```bash
    # kubectl label nodes <your-node-name> disktype=ssd
    kubectl label nodes k8sworker03 diskytype=ssd-
```
6. 
```bash
   kubectl get nodes k8sworker03 --show-labels
```

## Testando a solução
7. 
```bash
   kubectl get nodes k8sworker03 --show-labels
```
8. 
```bash
    kubectl create -f https://k8s.io/examples/pods/pod-nginx.yaml --dry-run=client -o yaml > pod-nginx-q17-dry-run.yaml
```
9. Faça copias do arquivo para deixar o original como BACKUP
```bash
    cp pod-nginx-q17-dry-run.yaml pod-nginx-q17.yaml
```
10. 
```bash
    vi pod-nginx-q17.yaml
```
11. 
```bash
    kubectl create -f pod-nginx-q17.yaml
```
12. 
```bash
    kubectl get pod -o wide
```

## Referncias
8. https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes/

## Limpando ambiente (caso seja necessário)
9. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl label nodes k8sworker03 diskytype-
     kubectl delete -f pod-nginx-q17.yaml
```
10. Valide que nenhum artefato está presente no namespace `q17-ns`
```bash
    kubectl get all -A | grep -i q17-ns
```