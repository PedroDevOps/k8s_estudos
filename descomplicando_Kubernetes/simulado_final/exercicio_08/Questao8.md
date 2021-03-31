# Questão 8

Ver quais os pods que mais estão consumindo cpu através do kubectl top.

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
1. Crie o namespace `q8-ns`.
```bash
    kcns q8-ns
    ou
    kubectl create namespace q8-ns
```
2. Mude o contexto para o namespace `q8-ns`, criado no passo anterior.
```bash
    kctx q8-ns
    ou
    kubectl config set-context --current --namespace q8-ns
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
    k create -f deploy deploy-nginx --image nginx --replicas=5 --dry-run=client -o yaml > deploy-nginx-dry-run.yaml
```
5. 
```bash
    cp deploy-nginx-dry-run.yaml deploy-nginx.yaml
```
6. 
```bash
   k create -f deploy deploy-busybox --image nginx --replicas=3 --dry-run=client -o yaml > deploy-busynox-dry-run.yaml
```
7. 
```bash
    cp deploy-busybox-dry-run.yaml deploy-busybox.yaml
```

## Testando a solução
7. 
```bash
    kubectl create -f deploy-busybox.yaml
    kubectl create -f deploy-nginx.yaml
```
8. 
```bash
    kubectl top pod pod-name
```   
9. 
```bash
    kubectl top node master
```
10. 
```bash
    
```
11. 
```bash
   
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f deploy-nginx
     kubectl delete -f deploy-busybox
```
13. Valide que nenhum artefato está presente no namespace `q8-ns`
```bash
    kubectl get all -A | grep -i q8-ns
```