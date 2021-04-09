# Questão 21

Criar Pod com o parametro containerport.

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
1. Crie o namespace `q21-ns`.
```bash
kcns q21-ns
ou
kubectl create namespace q21-ns
```
2. Mude o contexto para o namespace `q21-ns`, criado no passo anterior.
```bash
kctx q21-ns
ou
kubectl config set-context --current --namespace q21-ns
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
    kubectl run pod-nginx-q21 --image nginx --port=80 --dry-run=client -o yaml > pod-nginx-q21-dry-run.yaml
```
5. 
```bash
    cp pod-nginx-q21-dry-run.yaml pod-nginx-q21.yaml
```
6. 
```bash
    vi pod-nginx-q21.yaml
```
6. 
```bash
   kubectl create -f  pod-nginx-q21.yaml
```

## Testando a solução
7. 
```bash
    kubectl expose pod pod-nginx-q21 --dry-run=cliente -o yaml > service-nginx-q21-dry-run.yaml
```
7. 
```bash
    cp service-nginx-q21-dry-run.yaml service-nginx-q21.yaml
```
7. 
```bash
    kubectl create -f service-nginx-q21.yaml
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
    kubectl delete -f service-nginx-q21.yaml
    kubectl delete -f pod-nginx-q21.yaml

```
13. Valide que nenhum artefato está presente no namespace `q21-ns`
```bash
    kubectl get all -A | grep -i q21-ns
```