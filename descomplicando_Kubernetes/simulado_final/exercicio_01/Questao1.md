# Questão 1

Criar um pod com um volume não persistente.

## Criando um Pod com um volume não persistente

1. Crie o namespace `q1-ns`.
```bash
kubectl create namespace q1-ns
```
2. Dentro do namespace `q1-ns` crie um pod com os seguintes atributos:
* name = pod-q1
* image = nginx

3. execute um dry run de criação de um pod para gerar o yaml e facilitar as demais configurações.
```bash
    kubectl run pod-q1 --image nginx --n q1-ns --dry-run=client -o yaml > pod-q1-dry-run.yaml
```
4. Faça uma copia do pod-q1-dry-run.yaml com o nome pod-q1.yaml
```bash
    cp pod-q1-dry-run.yaml pod-q1.yaml
```
5. Abra o arquivo pod-q1.yaml com vim
```bash
    vim pod-q1.yaml
```
6. Dentro de spec: --> containers: --> image: inclua a definição do atributo "volumeMounts"
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
7. Dentro de spec: --> containers: inclua a definição do atributo "volumes" e salve as modificações
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
8. Execute o comando de criação do pod passando como parâmetro o arquivo pod-q1.yaml
```bash
    kubectl create -f pod-q1.yaml
```
9. Após criação, certifique que o pod escontrasse em estado de runnig.
```bash
    kubectl get pod pod-q1.yaml -n q1-ns
```
10. Valide que o pod foi criado com os valores definidos nos passos 2.,6. e 7.
```bash
    kubectl describe pod-q1
```
## Testando se criação ocorreu com sucesso

11. Verifique se o Pod está no estado de runnig e se encontra no namespace correto
```bash
    kubectl get pods --namespace q1-ns
```
12. Acesse o container rodando no pod-q1 e crie um arquivo dentro da pasta onde foi montado o volume
```bash
    kubectl exec -ti pod-q1 -n q1-ns -c pod-q1 -- touch /data/funciona
```
13. Acesse o container rodando no pod-q1 e verifique se o arquivo criado no passo 12. está dentro da pasta onde foi montado o volume
```bash
    kubectl exec -ti pod-q1 -n q1-ns -c pod-q1 -- ls -l /data/
    ou
    kubectl exec -ti pod-q1 -n q1-ns -c pod-q1 sh
    cd data
    ls
```
14. Identifique em qual nó do cluster está rodando o pod-q1
```bash
    kubectl get pod -o wide -n q1-ns
```
15. Acesse remotamente o nó identificado no passo 14. e procure pela pasta onde foi montado o volume
```bash
    find /var/lib/kubelet/pods/ -iname volume-nao-persistente
``` 
16. Liste os arquivos da pasta encontrada no passo 15.
```bash
    ls no result (find /var/lib/kubelet/pods/ -iname volume-nao-persistente)
``` 
## Limpando ambiente (caso seja necessário)

17. Dentro do Master delete o pod-q1
```bash
    kubectl delete -f pod-q1.yaml
``` 
18. Verifique se a operação finalizou com sucesso
```bash
    kubectl get pod -n q1-ns
``` 
19. Acesse o nó identificado no passo 14. e certifique que o volume criado não está mais presente.
```bash
    find /var/lib/kubelet/pods/ -iname volume-nao-persistente
``` 
20. Delete o namespace
```bash
    kubectl delete -n q1-ns
```
