# Questão 22

Declarar a variável NGINX_PORT no env do container

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
1. Crie o namespace `q22-ns`.
```bash
kcns q22-ns
ou
kubectl create namespace q22-ns
```
2. Mude o contexto para o namespace `q22-ns`, criado no passo anterior.
```bash
kctx q22-ns
ou
kubectl config set-context --current --namespace q22-ns
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
    kubectl create -f https://k8s.io/examples/pods/init-containers.yaml --dry-run=client -o yaml > nginx-pod-q22-dry-run.yaml
```
5. 
```bash
    cp nginx-pod-q22-dry-run.yaml nginx-pod-q22.yaml
```
6. 
```bash
    vi nginx-pod-q22.yaml
```
7. 
```bash
(...)
spec:
  containers:
  - image: nginx
    name: nginx
    env:
    - name: NGINX_PORT
      value: "8000"
(...)
```
8. 
```bash
   kubectl create -f nginx-pod-q22.yaml
```

## Testando a solução
9. 
```bash
    kubectl exec -it init-demo -- /bin/bash
```
10. 
```bash
   echo $NGINX_PORT
```

## Limpando ambiente (caso seja necessário)
11. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f nginx-pod-q22.yaml
```
12. Valide que nenhum artefato está presente no namespace `q22-ns`
```bash
    kubectl get all -A | grep -i q22-ns
```