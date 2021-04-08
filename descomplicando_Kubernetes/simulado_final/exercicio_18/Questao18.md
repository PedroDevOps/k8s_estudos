# Questão 18

Subir um pod com afinidade de node.

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
1. Crie o namespace `q18-ns`.
```bash
kcns q18-ns
ou
kubectl create namespace q18-ns
```
2. Mude o contexto para o namespace `q18-ns`, criado no passo anterior.
```bash
kctx q18-ns
ou
kubectl config set-context --current --namespace q18-ns
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
    kubectl run -f https://k8s.io/examples/pods/pod-nginx.yaml --dry-run=client -o yaml > pod-nginx-q18-dry-run.yaml
```
5. Faça copias do arquivo para deixar o original como BACKUP
```bash
    cp pod-nginx-q18-dry-run.yaml pod-nginx-q18.yaml
```
6. 
```bash
    vi pod-nginx-q18.yaml
```

## Testando a solução
7. Liste todos os nós e seus labels
```bash
    kubectl get nodes --show-labels  
```  
8. Adicione o label ao nó
```bash
    # kubectl label nodes <your-node-name> disktype=ssd
    kubectl label nodes k8sworker03 diskytype=ssd
```
9. Verifique se o label foi aplicado corretamente
```bash
     kubectl get nodes k8sworker03 --show-labels
```
10. Crie um pod com a Afinidade ao nó com a label diskytype=ssd
```bash
   kubectl create -f pod-nginx-q18.yaml
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl label nodes k8sworker03 diskytype=ssd-
     kubectl delete -f pod-nginx-q18.yaml
```
13. Valide que nenhum artefato está presente no namespace `q18-ns`
```bash
    kubectl get all -A | grep -i q18-ns
```