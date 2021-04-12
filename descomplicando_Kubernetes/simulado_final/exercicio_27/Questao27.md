# Questão 27

Criar um volume Emptydir e compartilhar entre 2 containers.

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
1. Crie o namespace `q27-ns`.
```bash
kcns q27-ns
ou
kubectl create namespace q27-ns
```
2. Mude o contexto para o namespace `q27-ns`, criado no passo anterior.
```bash
kctx q27-ns
ou
kubectl config set-context --current --namespace q27-ns
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
    kubectl apply -f https://k8s.io/examples/pods/two-container-pod.yaml --dry-run=client -o yaml > pod-2containers-q27-dry-run.yaml
   
```
5. 
```bash
    cp pod-2containers-q27-dry-run.yaml pod-2containers-q27.yaml
```

6k. 
```bash
    kubectl get jobs
```

6. 
```bash
    # Replace "hello-4111706356" with the job name in your system
    pods=$(kubectl get pods --selector=job-name=hello-4111706356 --output=jsonpath={.items[*].metadata.name})
    ou
    kubectl get pods #pegar o nome do ultimo e utilizar ele no próximo comando
```

## Testando a solução
7. 
```bash
    kubectl logs $pods
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete cronjob my-job
```
13. Valide que nenhum artefato está presente no namespace `q27-ns`
```bash
    kubectl get all -A | grep -i q27-ns
```

## Referencia
14. 