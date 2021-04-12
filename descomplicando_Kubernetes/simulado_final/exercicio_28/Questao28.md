# Questão 28

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
1. Crie o namespace `q28-ns`.
```bash
kcns q28-ns
ou
kubectl create namespace q28-ns
```
2. Mude o contexto para o namespace `q28-ns`, criado no passo anterior.
```bash
kctx q28-ns
ou
kubectl config set-context --current --namespace q28-ns
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
    kubectl create -f https://k8s.io/examples/pods/commands.yaml --dry-run=client -o yaml > pod-command-q28-dry-run.yaml
```
5. 
```bash
    cp pod-command-q28-dry-run.yaml pod-command-q28.yaml
```
6. 
```bash
    vim pod-command-q28.yaml
```
6. 
```bash
    kubectl create -f pod-command-q28.yaml
```

## Testando a solução
7. 
```bash
    kubectl logs command-demo
```
7. 
```bash
    vim pod-command-q28.yaml
```
7. 
```bash
    kubectl logs command-demo
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete cronjob my-job
```
13. Valide que nenhum artefato está presente no namespace `q28-ns`
```bash
    kubectl get all -A | grep -i q28-ns
```

## Referencia
14. https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/