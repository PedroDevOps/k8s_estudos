# Questão 6

Criar um daemonset.

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
1. Crie o namespace `q6-ns`.
```bash
kcns q6-ns
ou
kubectl create namespace q6-ns
```
2. Mude o contexto para o namespace `q6-ns`, criado no passo anterior.
```bash
kctx q6-ns
ou
kubectl config set-context --current --namespace q6-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução
4. Crie o arquivo de daemon
```bash
    vim primeiro-daemonset.yaml
```
5. Com o seguinte conteúdo:
```bash
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: daemon-set-primeiro
spec:
  selector:
    matchLabels:
      system: Strigus
  template:
    metadata:
      labels:
        system: Strigus
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
  updateStrategy:
    type: RollingUpdate
```
6. Crie o pod a partir do manifesto.
```bash
   kubectl create -f primeiro-daemonset.yaml
```
7. Verifique se o damonset foi criado corretamente
```bash
    kubectl get daemonset
```
8. Verifique suas configurações
```bash
    kubectl describe ds daemon-set-primeiro
```   
9. Identifique aonde os pods foram criados
```bash
    kgp -o wide
```

## Testando a solução
10. Efetue modificaçõe no daemon que está sendo executado
```bash
    kubectl set image ds daemon-set-primeiro nginx=nginx:1.15.0
```
11. Verifique a versão incial do daemon antes da mudança de imagem
```bash
    kubectl rollout history ds daemon-set-primeiro --revision=1
```
12. Verifique o rollout após a mudança
```bash
    kubectl rollout history ds daemon-set-primeiro --revision=2
```

## Limpando ambiente (caso seja necessário)
13. Faça a limpesa do ambiente (Caso necessário)
```bash
    k delete -f primeiro-daemonset.yaml
```
14. Valide que nenhum artefato está presente no namespace `q6-ns`
```bash
    kubectl get all -A | grep -i q6-ns
```