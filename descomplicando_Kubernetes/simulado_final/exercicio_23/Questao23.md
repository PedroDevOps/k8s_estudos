# Questão 23

Declarar a variável na configmap e passar para container.

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
1. Crie o namespace `q23-ns`.
```bash
kcns q23-ns
ou
kubectl create namespace q23-ns
```
2. Mude o contexto para o namespace `q23-ns`, criado no passo anterior.
```bash
kctx q23-ns
ou
kubectl config set-context --current --namespace q23-ns
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
    kubectl create configmap special-config --from-literal=special.how=very
```
5. 
```bash
    kubectl describe configmap special-config
```
5. 
```bash
    kubectl create -f https://kubernetes.io/examples/pods/pod-single-configmap-env-variable.yaml --dry-run=client -o yaml > pod-usando_configmap-q23-dry-run.yaml
```
6. 
```bash
    cp pod-usando_configmap-q23-dry-run.yaml pod-usando_configmap-q23.yaml
```
7. 
```bash
    vi pod-usando_configmap-q23.yaml
```

## Testando a solução
8. 
```bash
    kubectl create -f pod-usando_configmap-q23.yaml
```
9. 
```bash
    kubectl get pods
```
10. 
```bash
    kubectl describe pod
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f pod-usando_configmap-q23.yaml
     kubectl delete configmap special-config
     kubectl delete configmap kube-root-ca.crt
```
13. Valide que nenhum artefato está presente no namespace `q23-ns`
```bash
    kubectl get all -A | grep -i q23-ns
```

## Referencia

14. https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/