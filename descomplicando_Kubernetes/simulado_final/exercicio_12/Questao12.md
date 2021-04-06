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
4. Criar um arquivo yaml para o deployment 
```bash
    kubectl create -f https://k8s.io/examples/controllers/nginx-deployment.yaml --dry-run=client -o yaml > deployment-pods-q12-dry-run.yaml
```
5. Modifica-lo caso necessário
```bash
    cp deploymeny-pods-q12-dry-run.yaml deployment-pods-q3.yaml
    vi deployment-pods-q12.yaml  
```
6. Criar o deployment com a versão desejada do nginx
```bash
   kubectl create -f deployment-pods-q12.yaml --record 
```

## Testando a solução
7. Verificar o estado do deployment
```bash
    kubectl get deployments
```
8. Acompanhar o status do rollout
```bash
    kubectl rollout status deployment nginx-deployment
```
9. Descrever o deployment e verificar a versão do nginx
```bash
    kubectl describe deployments nginx-deployment
```   
10. Atualizar a versão do nginx
```bash
    kubectl set image deployment nginx-deployment nginx=nginx:1.16.1 --record
```
11. Descrever o deployment e verificar a versão do nginx
```bash
    kubectl describe deployments nginx-deployment
```
12. Listar o historico de mudanças
```bash
    kubectl rollout history deployment nginx-deployment
```
13. Reverter o update de versão
```bash
    kubectl rollout undo deployment nginx-deployment --to-revision=1
```
14. Acompanhar o status do rollout
```bash
    kubectl rollout status deployment nginx-deployment
```
15. Verificar o estado do deployment
```bash
    kubectl get deployments
```
16. Descrever o deployment e verificar a versão do nginx
```bash
    kubectl describe deployments nginx-deployment
```

## Limpando ambiente (caso seja necessário)
17. Faça a limpesa do ambiente (Caso necessário)
```bash
    kubectl delete -f deployment-pods-q12.yaml
```
18. Valide que nenhum artefato está presente no namespace `q12-ns`
```bash
    kubectl get all -A | grep -i q12-ns
```