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
## Referncias
8. https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes/

## Limpando ambiente (caso seja necessário)
9. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl label nodes k8sworker03 diskytype=ssd-
```
10. Valide que nenhum artefato está presente no namespace `q17-ns`
```bash
    kubectl get all -A | grep -i q17-ns
```