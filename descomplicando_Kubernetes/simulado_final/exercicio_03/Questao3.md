# Questão 3



## Configuração Previa
0. alguns alias recomendados:
```bash
$ alias k=kubectl
$ alias kgp="k get pods"
$ alias kcns="k create ns"
$ alias kgns="k get ns"
$ alias kgtx="k config get-contexts"
$ alias kctx="k config set-context --current --namespace"
```
## Criação do Namespace e Definição Contexto 
1. Crie o namespace `q3-ns`.
```bash
kcns q3-ns
ou
kubectl create namespace q3-ns
```
2. Mude o contexto para o namespace `q3-ns`, criado no passo anterior.
```bash
kctx q3-ns
ou
kubectl config set-context --current --namespace q3-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução
4. Identifique o nome de cadas nós do cluster
```bash
    K get nodes
    ou
    Kubectl get nodes
```
5. Escolha 1 worker e identifique se algum Taint está sendo aplicado em seu respectivo nó
```bash
    k describe node xxxxxxx
    ou
    kubectl describe node xxxxxxx
```
6. 
```bash
   k
```
7. 
```bash
    k
```

## 
8. 
```bash
    k
```   
9. 
```bash
    k
```
## Testando a solução
10. 
```bash
    k
```
11. 
```bash
    k
```
12. 
```bash
    
```
