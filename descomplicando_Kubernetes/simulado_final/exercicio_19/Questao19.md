# Questão 19

Ajustar o nome de uma imagem com nome errado de um deployment

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
1. Crie o namespace `q19-ns`.
```bash
kcns q19-ns
ou
kubectl create namespace q19-ns
```
2. Mude o contexto para o namespace `q19-ns`, criado no passo anterior.
```bash
kctx q19-ns
ou
kubectl config set-context --current --namespace q19-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```
## Início da Solução
4. Criar um arquivo yaml para o deployment 
```bash
    kubectl create -f https://k8s.io/examples/controllers/nginx-deployment.yaml --dry-run=client -o yaml > deployment-pods-q19-dry-run.yaml
```
5. Modifica-lo caso necessário
```bash
    cp deployment-pods-q19-dry-run.yaml deployment-pods-q19.yaml
    # Mude o nome da Imagem para um que não existe
    vi deployment-pods-q19.yaml
```
6. Criar o deployment com a versão desejada do nginx
```bash
   kubectl create -f deployment-pods-q19.yaml --record 
```

## Testando a solução
7. Verificar o estado do deployment
```bash
    kubectl get deployments
```
8. Faça a edição do nome da imagem no deployment
```bash
    kubectl edit deployments.apps nginx-deployment --record
```   
9. Acompanhar o status do rollout
```bash
    kubectl rollout status deployment nginx-deployment
```
10. Listar o historico de mudanças
```bash
    kubectl rollout history deployment nginx-deployment
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f deployment-pods-q19.yaml
```
13. Valide que nenhum artefato está presente no namespace `q19-ns`
```bash
    kubectl get all -A | grep -i q19-ns
```

## Referencia

14. https://kubernetes.io/docs/concepts/workloads/controllers/deployment/