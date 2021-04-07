# Questão 1

Criar um pod com um volume não persistente.

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
1. Crie o namespace `q1-ns`.
```bash
kcns q1-ns
ou
kubectl create namespace q1-ns
```
2. Mude o contexto para o namespace `q1-ns`, criado no passo anterior.
```bash
kctx q1-ns
ou
kubectl config set-context --current --namespace q1-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução
4. Crie um pod com os seguintes atributos:
* name = pod-q1
* image = nginx

5. execute um dry run de criação de um pod para gerar o yaml e facilitar as demais configurações.
```bash
    kubectl run pod-q1 --image nginx --dry-run=client -o yaml > pod-q1-dry-run.yaml
```
6. Faça uma copia do pod-q1-dry-run.yaml com o nome pod-q1.yaml
```bash
    cp pod-q1-dry-run.yaml pod-q1.yaml
```
7. Abra o arquivo pod-q1.yaml com vim
```bash
    vim pod-q1.yaml
```
8. Dentro de spec: --> containers: --> image: inclua a definição do atributo "volumeMounts"
```vim
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod-q1
  name: pod-q1
  namespace: q1-ns
spec:
  containers:
  - image: nginx
    name: pod-q1
    resources: {}
    volumeMounts:
    - mountPath: /data
      name: volume-nao-persistente
```
9. Dentro de spec: --> containers: inclua a definição do atributo "volumes" e salve as modificações
```vim
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod-q1
  name: pod-q1
  namespace: q1-ns
spec:
  containers:
  - image: nginx
    name: pod-q1
    resources: {}
    volumeMounts:
    - mountPath: /data
      name: volume-nao-persistente
  volumes:
    - name: volume-nao-persistente
      emptyDir: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```
10. Execute o comando de criação do pod passando como parâmetro o arquivo pod-q1.yaml
```bash
    kubectl create -f pod-q1.yaml
```
11. Após criação, certifique que o pod está em estado de runnig.
```bash
    kubectl get pod pod-q1.yaml
```
12. Valide que o pod foi criado com as configurações corretas.
```bash
    kubectl describe pod-q1
```

## Testando a solução
13. Verifique se o Pod está no estado de runnig e se encontra no namespace correto
```bash
    kubectl get pods
```
14. Acesse o container rodando no pod-q1 e crie um arquivo dentro da pasta onde foi montado o volume
```bash
    kubectl exec -ti pod-q1 -c pod-q1 -- touch /data/funciona
```
15. Acesse o container rodando no pod-q1 e verifique se o arquivo criado no passo 12. está dentro da pasta onde foi montado o volume
```bash
    kubectl exec -ti pod-q1 -c pod-q1 -- ls -l /data/
    ou
    kubectl exec -ti pod-q1 -c pod-q1 sh
    cd data
    ls
```
16. Identifique em qual nó do cluster está rodando o pod-q1
```bash
    kubectl get pod -o wide
```
17. Acesse remotamente o nó identificado no passo 16. e procure pela pasta onde foi montado o volume
```bash
    find /var/lib/kubelet/pods/ -iname volume-nao-persistente
``` 
18. Liste os arquivos da pasta encontrada no passo 17.
```bash
    ls no result (find /var/lib/kubelet/pods/ -iname volume-nao-persistente)
``` 

## Limpando ambiente (caso seja necessário)
19. Dentro do Master delete o pod-q1
```bash
    kubectl delete -f pod-q1.yaml
``` 
20. Verifique se a operação finalizou com sucesso
```bash
    kubectl get pod
``` 
21. Acesse o nó identificado no passo 16. e certifique que o volume criado não está mais presente.
```bash
    find /var/lib/kubelet/pods/ -iname volume-nao-persistente
``` 