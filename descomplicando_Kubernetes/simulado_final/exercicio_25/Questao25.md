# Questão 25

Configurar resources limits no deployment.

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
1. Crie o namespace `q25-ns`.
```bash
kcns q25-ns
ou
kubectl create namespace q25-ns
```
2. Mude o contexto para o namespace `q25-ns`, criado no passo anterior.
```bash
kctx q25-ns
ou
kubectl config set-context --current --namespace q25-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução
4. Criar um manifesto via --dry-run para um deployent com 5 réplicas
```bash
    kubectl create deploy deploy-nginx --image nginx --replicas 5 --dry-run=client -o yaml > deploy-nginx-q25-dry-run.yaml
```
5. Copiar para outro arquivo .yaml
```bash
    cp deploy-nginx-q25-dry-run.yaml deploy-nginx-q25.yaml
```
5. 
```bash
    vi deploy-nginx-q25.yaml
```
6. 
```bash
resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```
6. Caso seja necessário altere o deploy-nginx.yaml, e depois execute-o
```bash
   k create -f deploy-nginx-q25.yaml
```
## Testando a solução
7. 
```bash
    kubectl describe deploy 
```
8. 
```bash
    
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete k -f deploy-nginx-q25.yaml
```
13. Valide que nenhum artefato está presente no namespace `q25-ns`
```bash
    kubectl get all -A | grep -i q25-ns
```

## Referencia
14. https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/