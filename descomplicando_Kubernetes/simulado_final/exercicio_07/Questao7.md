# Questão 7

Criar um deployment do nginx com 5 réplicas.

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
4. Criar um manifesto via --dry-run para um deployent com 5 réplicas
```bash
    kubectl create deploy deploy-nginx --image nginx --replicas 5 --dry-run=client -o yaml > deploy-ngix-dry-run.yaml
```
5. Copiar para outro arquivo .yaml
```bash
    cp deploy-ngix-dry-run.yaml deploy-nginx.yaml
```
6. Caso seja necessário altere o deploy-nginx.yaml, e depois execute-o
```bash
   k create -f deploy-nginx.yaml
```

## Testando a solução
7. Verifique em quais nós os containers foram alocados
```bash
    kgp -o wide
```
8. Faça um scale up
```bash
    kubectl scale deployment deploy-nginx --replicas=10
```   
9. Acompanhe o processo de rollout
```bash
    kubectl rollout status deployment deploy-nginx
```
10. Faça um scale down 
```bash
    kubectl scale deployment deploy-nginx --replicas=3
```
11. Acompanhe o processo com watch
```bash
    watch kubectl get pods
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
    k delete -f deploy-ngix.yaml 
```
13. Valide que nenhum artefato está presente no namespace `q7-ns`
```bash
    kubectl get all -A | grep -i q7-ns
```