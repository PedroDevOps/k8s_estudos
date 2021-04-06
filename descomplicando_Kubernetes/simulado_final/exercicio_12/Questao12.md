# Questão 12

Fazer a instalação do nginx em determinada versão, atualizar e depois realizar o rollback com o --record.

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
1. Crie o namespace `q12-ns`.
```bash
kcns q12-ns
ou
kubectl create namespace q12-ns
```
2. Mude o contexto para o namespace `q12-ns`, criado no passo anterior.
```bash
kctx q12-ns
ou
kubectl config set-context --current --namespace q12-ns
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
    kubectl create -f https://k8s.io/examples/controllers/nginx-deployment.yaml --dry-run=client -o yaml > deployment-pods-q12-dry-run.yaml
```
5. 
```bash
    cp deploymeny-pods-q12-dry-run.yaml deployment-pods-q3.yaml
    vi deployment-pods-q12.yaml  
```
6. 
```bash
   kubectl create -f deployment-pods-q12.yaml --record 
```

## Testando a solução
7. 
```bash
    kubectl get deployments
```
8. 
```bash
    kubectl rollout status deployment nginx-deployment
```
14. 
```bash
    kubectl describe deployments nginx-deployment
```   
9. 
```bash
    kubectl set image deployment nginx-deployment nginx=nginx:1.16.1 --record
```
14. 
```bash
    kubectl describe deployments nginx-deployment
```
10. 
```bash
    kubectl rollout history deployment nginx-deployment
```
11. 
```bash
    kubectl rollout undo deployment nginx-deployment --to-revision=1
```
12. 
```bash
    kubectl rollout status deployment nginx-deployment
```
13. 
```bash
    kubectl get deployments
```
14. 
```bash
    kubectl describe deployments nginx-deployment
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
    kubectl delete -f deployment-pods-q12.yaml
```
13. Valide que nenhum artefato está presente no namespace `q12-ns`
```bash
    kubectl get all -A | grep -i q12-ns
```