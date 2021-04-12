# Questão 30

Criar um cronjob.

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
1. Crie o namespace `q30-ns`.
```bash
kcns q30-ns
ou
kubectl create namespace q30-ns
```
2. Mude o contexto para o namespace `q30-ns`, criado no passo anterior.
```bash
kctx q30-ns
ou
kubectl config set-context --current --namespace q30-ns
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
   kubectl create -f https://k8s.io/examples/controllers/nginx-deployment.yaml --dry-run=client -o yaml > deploy-nginx-q30-dry-run.yaml
```
5. 
```bash
    cp deploy-nginx-q30-dry-run.yaml deploy-nginx-q30.yaml
```
6. 
```bash
    vi deploy-nginx-q30.yaml
```
7. 
```bash
    kubectl create -f deploy-nginx-q30.yaml --record 
```

## Testando a solução
8. 
```bash
    kubectl rollout pause deploy nginx-deployment
```
9. 
```bash
    kubectl set image deployment nginx-deployment nginx=nginx:1.16.1 --record
```
10. 
```bash
    kubectl rollout history deployment nginx-deployment
```
11. 
```bash
    kubectl set resources deployment nginx-deployment -c=nginx --limits=cpu=200m,memory=512Mi --record
```
12. 
```bash
    kubectl rollout history deployment nginx-deployment
```
13. 
```bash
    kubectl rollout resume deployment nginx-deployment
```
14. 
```bash
    kubectl get rs -w
```
15. 
```bash
    kubectl describe deploy nginx-deployment
```
16. 
```bash
    kubectl describe pod nginx- | grep image
```
16. 
```bash
    kubectl rollout history deployment nginx-deployment
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f deploy-nginx-q30.yaml
```
13. Valide que nenhum artefato está presente no namespace `q30-ns`
```bash
    kubectl get all -A | grep -i q30-ns
```

## Referencia
14. https://kubernetes.io/docs/concepts/workloads/controllers/_print/