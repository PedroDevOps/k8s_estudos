# Questão 10

Qual a quantidade de nodes que estão aceitando novos containers

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
1. Crie o namespace `q10-ns`.
```bash
kcns q10-ns
ou
kubectl create namespace q10-ns
```
2. Mude o contexto para o namespace `q10-ns`, criado no passo anterior.
```bash
kctx q10-ns
ou
kubectl config set-context --current --namespace q10-ns
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
    k describe nodes | grep -i taint
```