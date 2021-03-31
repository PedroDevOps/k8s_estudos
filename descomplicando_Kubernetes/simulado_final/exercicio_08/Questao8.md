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
4. Crie um deployment usando a image do nginx
```bash
    k create deploy deploy-nginx-q8 --image nginx --replicas=5 --dry-run=client -o yaml > deploy-nginx-q8-dry-run.yaml
```
5. 
```bash
    cp deploy-nginx-q8-dry-run.yaml deploy-nginx-q8.yaml
```
6. Crie um deployment usando a image do busybox
```bash
   k create deploy deploy-busybox-q8 --image nginx --replicas=3 --dry-run=client -o yaml > deploy-busybox-q8-dry-run.yaml
```
7. 
```bash
    cp deploy-busybox-q8-dry-run.yaml deploy-busybox-q8.yaml
```

## Testando a solução
8. Faça a criação dos dois deployments
```bash
    kubectl create -f deploy-busybox-q8.yaml
    kubectl create -f deploy-nginx-q8.yaml
```
9. Faça a criação de um servidor de metricas para o kubernetes
```bash
    kubectl create -f https://raw.githubusercontent.com/pythianarora/total-practice/master/sample-kubernetes-code/metrics-server.yaml
    
```   
10. Verifique o consumo de recursos de algum nó específico
```bash
    kubectl top node k8sworker01
```
11. Verifique o consumo de recursos de todos os nós
```bash
    kubectl top nodes
```
12. Verifique o consumo de recursos de todos os pods
```bash
   kubectl top pod
```

## Limpando ambiente (caso seja necessário)
13. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f deploy-nginx-q8.yaml
     kubectl delete -f deploy-busybox-q8.yaml
     kubectl delete -f https://raw.githubusercontent.com/pythianarora/total-practice/master/sample-kubernetes-code/metrics-server.yaml
```
14. Valide que nenhum artefato está presente no namespace `q8-ns`
```bash
    kubectl get all -A | grep -i q8-ns
```