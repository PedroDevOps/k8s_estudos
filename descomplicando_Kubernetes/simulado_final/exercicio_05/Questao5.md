# Questão 5

Criar um initcontainer para executar uma tarefa necessária para a subida do container principal.

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
1. Crie o namespace `q5-ns`.
```bash
kcns q5-ns
ou
kubectl create namespace q5-ns
```
2. Mude o contexto para o namespace `q5-ns`, criado no passo anterior.
```bash
kctx q5-ns
ou
kubectl config set-context --current --namespace q5-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução
4. Crie o Pod a partir do manifesto:
```bash
    vim nginx-initcontainer.yaml
```
5. Com o seguinte conteúdo:
```bash
apiVersion: v1
kind: Pod
metadata:
  name: init-demo
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - name: workdir
      mountPath: /usr/share/nginx/html
  initContainers:
  - name: install
    image: busybox
    command: ['wget','-O','/work-dir/index.html','http://linuxtips.io']
    volumeMounts:
    - name: workdir
      mountPath: "/work-dir"
  dnsPolicy: Default
  volumes:
  - name: workdir
    emptyDir: {}
```
6. Crie o pod a partir do manifesto.
```bash
     kubectl create -f nginx-initcontainer.yaml
```
7. Visualize o conteúdo de um arquivo dentro de um contêiner do Pod.
```bash
    kubectl exec -ti init-demo -- cat /usr/share/nginx/html/index.html
```

## 
8. Coletando os logs do contêiner init:
```bash
    kubectl logs init-demo -c install
```   
9. 
```bash
    k
```
## Testando a solução
10. Removendo o Pod a partir do manifesto:
```bash
    kubectl delete -f nginx-initcontainer.yaml
```
11. 
```bash
    k
```
12. 
```bash
    
```
