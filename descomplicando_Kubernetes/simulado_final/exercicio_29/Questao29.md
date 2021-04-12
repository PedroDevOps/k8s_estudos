# Questão 29

Customizar o parâmetro command do container.

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
1. Crie o namespace `q29-ns`.
```bash
kcns q29-ns
ou
kubectl create namespace q29-ns
```
2. Mude o contexto para o namespace `q29-ns`, criado no passo anterior.
```bash
kctx q29-ns
ou
kubectl config set-context --current --namespace q29-ns
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
    kubectl create -f https://k8s.io/examples/pods/pod-nginx.yaml --dry-run=client -o yaml > pod-nodeselector-q29-dry-run.yaml
```
5. 
```bash
    cp pod-nodeselector-dry-run.yaml pod-nodeselector.yaml
```
6. 
```bash
    vim pod-nodeselector.yaml
```

## Testando a solução
8. Liste todos os nós e seus labels
```bash
    kubectl get nodes --show-labels  
```  
9. Adicione o label ao nó
```bash
    # kubectl label nodes <your-node-name> disktype=ssd
    kubectl label nodes k8sworker02 disktype=ssd
```
10. Verifique se o label foi aplicado corretamente
```bash
     kubectl get nodes k8sworker02 --show-labels | grep disktype
```
11. 
```bash
    kubectl create -f pod-nodeselector-q29.yaml
```
12. 
```bash
    kubectl get pods
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f pod-nodeselector-q29.yaml
     kubectl label nodes k8sworker02 disktype-
```
13. Valide que nenhum artefato está presente no namespace `q29-ns`
```bash
    kubectl get all -A | grep -i q29-ns
```

## Referencia
14. https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/