# Questão 7



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
1. Crie o namespace `q7-ns`.
```bash
kcns q7-ns
ou
kubectl create namespace q7-ns
```
2. Mude o contexto para o namespace `q7-ns`, criado no passo anterior.
```bash
kctx q7-ns
ou
kubectl config set-context --current --namespace q7-ns
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
    kubectl create deploy deploy-nginx --image nginx --replicas 5 --dry-run=client -o yaml > deploy-ngix-dry-run.yaml
```
5. 
```bash
    cp deploy-ngix-dry-run.yaml deploy-nginx.yaml
```
6. 
```bash
   k create -f deploy-nginx.yaml
```
7. 
```bash
    kgp -o wide
```

## 
8. 
```bash
    kubectl scale deployment deploy-nginx --replicas=10
```   
9. 
```bash
    kubectl rollout status deployment deploy-nginx
```
## Testando a solução
10. 
```bash
    kubectl scale deployment deploy-nginx --replicas=3
```
11. 
```bash
    watch kubectl get pods
```
12. 
```bash
    k delete -f deploy-ngix.yaml 
```
