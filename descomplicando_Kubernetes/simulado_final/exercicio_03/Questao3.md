# Questão 3

Colocar um node para que não execute nenhum containers.

## Configuração Previa
0. alguns alias recomendados:
```bash
 alias k=kubectl
 alias kgp="k get pods"
 alias kcns="k create ns"
 alias kgns="k get ns"
 alias kgtx="k config get-contexts"
 alias kctx="k config set-context --current --namespace"
 alias
 complete -F __start_kubectl k
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
4. Identifique o nome de cada nó do cluster
```bash
    K get nodes
    ou
    Kubectl get nodes
```
5. Escolha 1 worker e identifique se algum Taint está sendo aplicado em seu respectivo nó
```bash
    k describe node xxxxxxx | grep -i taint
    ou
    kubectl describe node xxxxxxx | grep -i taint
```
6. Aplique o Taint no nó com o valor de NoSchedule
```bash
    k taint node xxxxxxx key1=value1:NoSchedule
```
7. Aplique o Taint no nó com o valor de NoExecute
```bash
    k taint node xxxxxxx key1=value1:NoExecute
```
8.  Valide que os Taints foram aplicados
```bash
    k describe node xxxxxxxx | grep -A 1 -i taint
```
## Testando a solução
9. Crie um deployment 
```bash
    k create deployment pods-q3-deployment --image nginx
    ou
    k create deployment pods-q3-deployment --image nginx --dry-run=client -o yaml > deployment-pods-q3.yaml
    k create -f deployment-pods-q3.yaml

```   
10. Efetue o scale para validar que nenhum pod será schedulado no Nó com Taint 
```bash
    k scale deployment pods-q3-deployment --replicas=3
    kgp -o wide

```
## Efetuando a Limpeza do ambiente (caso necessário)
11. Remova o Taint de NoSchedule
```bash
    k taint node xxxxxxx key1:NoSchedule-
```
12. Remova o Taint de NoExecute
```bash
    k taint node xxxxxxx key1:NoExecute-
```
13. Incluindo novamente o Taint no Master (caso tenha sido removido)
```bash
    kubectl taint node xxxmasterxxx node-role.kubernetes.io/master:NoSchedule
```
14. Remova os deployments
```bash
    k delete deployment pods-q3-deployment
```
15. Valide que nenhum artefato está presente no namespace `q3-ns`
```bash
    kubectl get all -A | grep -i q3-ns
```