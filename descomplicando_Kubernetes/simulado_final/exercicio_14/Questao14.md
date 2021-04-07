# Questão 14

Identificar quais pods fazem parte de determinado services.

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
1. Crie o namespace `q14-ns`.
```bash
    kcns q14-ns
    ou
    kubectl create namespace q14-ns
```
2. Mude o contexto para o namespace `q14-ns`, criado no passo anterior.
```bash
    kctx q14-ns
    ou
    kubectl config set-context --current --namespace q14-ns
```
3. Confirme a mudança de contexto
```bash
    kgtx
    ou
    kubectl config get-context
```

## Início da Solução
4. Crie arquivos yaml para um deployment e um pod usando a image do nginx
```bash
    kubectl create -f https://k8s.io/examples/controllers/nginx-deployment.yaml --dry-run=client -o yaml > deployment-pods-q14-dry-run.yaml
    kubectl run pod-nginx-q14 --image nginx --dry-run=client -o yaml > pod-nginx-q14-dry-run.yaml
```
5. Faça copias dos arquivos para deixar os originais como BACKUP
```bash
    cp pod-nginx-q14-dry-run.yaml pod-nginx-q14.yaml
    cp deployment-pods-q14-dry-run.yaml deployment-pods-q14.yaml
```
6. Edite-os (Caso Necessário)
```bash
    vi pod-nginx-q14.yaml
    vi deployment-pods-q14.yaml
```

## Testando a solução
8. Faça a criação do deployment e do pod
```bash
    kubectl create -f deployment-pods-q14.yaml
    kubectl create -f pod-nginx-q14.yaml
```
9. Crie arquivos yaml para dois services
```bash
    kubectl expose pod pod-nginx-q14 --port=80 --dry-run=client -o yaml > service-nginx-q14-dry-run.yaml
    kubectl expose deploy nginx-deployment --dry-run=client -o yaml > service-deploy-nginx-q14-dry-run.yaml
```   
10. Faça copias dos arquivos para deixar os originais como BACKUP
```bash
    cp service-deploy-nginx-q14-dry-run.yaml service-deploy-nginx-q14.yaml
    cp service-nginx-q14-dry-run.yaml service-nginx-q14.yaml
```
11. Edite-os (Caso Necessário)
```bash
    vim service-nginx-q14.yaml
    vim service-deploy-nginx-q14.yaml
```
12. Crie os dois services
```bash
    kubectl create -f service-deploy-nginx-q14.yaml
    kubectl create -f service-nginx-q14.yaml
```
13. Verifique se os services foram criados com sucesso
```bash
    k describe svc service-nginx-q14.yaml
    k describe svc service-deploy-nginx-q14.yaml
```
14. Usando o selector (Modo Easy)
```bash
    kubectl get services -o=wide
    kubectl get pods --selector=run=pod-nginx-q14 -o=name 
    kubectl get pods --selector=app=nginx -o=name
```
15. Usando jsonpath (Modo Hard)
```bash
    kubectl get ep nginx-deployment -o=jsonpath='{.subsets[*].addresses[*].ip}' | tr ' ' '\n' | xargs -I % kubectl get pods -o=name --field-selector=status.podIP=%
```

## Limpando ambiente (caso seja necessário)
16. Faça a limpesa do ambiente (Caso necessário)
```bash
    kubectl delete -f service-deploy-nginx-q14.yaml
    kubectl delete -f service-nginx-q14.yaml
    kubectl delete -f deployment-pods-q14.yaml
    kubectl delete -f pod-nginx-q14.yaml

```
17. Valide que nenhum artefato está presente no namespace `q14-ns`
```bash
    kubectl get all
    ou
    kubectl get all -A | grep -i q14-ns
```