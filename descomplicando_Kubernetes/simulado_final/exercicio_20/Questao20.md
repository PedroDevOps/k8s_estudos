# Questão 20

Criar um cronjob.

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
1. Crie o namespace `q20-ns`.
```bash
kcns q20-ns
ou
kubectl create namespace q20-ns
```
2. Mude o contexto para o namespace `q20-ns`, criado no passo anterior.
```bash
kctx q20-ns
ou
kubectl config set-context --current --namespace q20-ns
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
    kubectl create -f https://k8s.io/examples/application/job/cronjob.yaml --dry-run=client -o yaml > cronjob-pod-q20-dry-run.yaml
    ou
    # kubectl create cronjob NAME --image=image --schedule='0/5 * * * ?' -- [COMMAND] [args...]
    kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *" -- date
```
5. 
```bash
    kubectl get cronjob my-job
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
13. Valide que nenhum artefato está presente no namespace `q20-ns`
```bash
    kubectl get all -A | grep -i q20-ns
```