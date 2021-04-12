# Questão 24

Declarar a variável no secret e passar para o container.


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
1. Crie o namespace `q24-ns`.
```bash
kcns q24-ns
ou
kubectl create namespace q24-ns
```
2. Mude o contexto para o namespace `q24-ns`, criado no passo anterior.
```bash
kctx q24-ns
ou
kubectl config set-context --current --namespace q24-ns
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
    kubectl create secret generic backend-user --from-literal=backend-username='Eu-admin?' --dry-run=client -o yaml > secret-q24-dry-run.yaml
```
5. 
```bash
    cp secret-q24-dry-run.yaml secret-q24.yaml
```
6. 
```bash
    vi secret-q24.yaml
```
7. 
```bash
    kubectl create -f secret-q24.yaml
```

## Testando a solução
8. 
```bash
    kubectl create -f https://k8s.io/examples/pods/inject/pod-single-secret-env-variable.yaml --dry-run=client -o yaml > pod-usando_secret-q24-dry-run.yaml
```
9. 
```bash
    cp pod-usando_secret-q24-dry-run.yaml pod-usando_secret-q24.yaml
```
11. 
```bash
    kubectl create -f pod-usando_secret-q24.yaml
```
10. 
```bash
    kubectl exec -it env-single-secret -- /bin/sh -c 'echo $SECRET_USERNAME'
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f pod-usando_secret-q24.yaml
     kubectl delete -f secret-q24.yaml
```
13. Valide que nenhum artefato está presente no namespace `q24-ns`
```bash
    kubectl get all -A | grep -i q24-ns
```

## Referencia
14. https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/