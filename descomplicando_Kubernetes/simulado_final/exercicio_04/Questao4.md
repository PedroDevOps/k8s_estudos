# Questão 4

Criar um PV Hostpath.

## Configuração Previa
0. Cadastre alguns alias recomendados:
```bash
 alias k=kubectl
 alias kgp="k get pods"
 alias kcns="k create ns"
 alias kgns="k get ns"
 alias kgtx="k config get-contexts"
 alias kctx="k config set-context --current --namespace"
 alias
```
## Criação do Namespace e Definição Contexto 
1. Crie o namespace `q4-ns`.
```bash
    kcns q4-ns
    ou
    kubectl create namespace q4-ns
```
2. Mude o contexto para o namespace `q4-ns`, criado no passo anterior.
```bash
    kctx q4-ns
    ou
    kubectl config set-context --current --namespace q4-ns
```
3. Confirme a mudança de contexto
```bash
    kgtx
    ou
    kubectl config get-context
```

## Início da Solução
4. Monte um diretório no node Master
```bash
    sudo mkdir /opt/dados
    sudo chmod 1777 /opt/dados/
```
5. Adicione esse diretório no NFS Server
```bash
    sudo vim /etc/exports
    /opt/dados *(rw,sync,no_root_squash,subtree_check)
```
6. Aplique a configuração do NFS no node master
```bash
   sudo exportfs -a
```
7. Agora faça o restart do serviço do NFS no node master
```bash
    sudo systemctl restart nfs-kernel-server
```
8. Crie um arquivo nesse diretório para teste futuro.
```bash
    sudo touch /opt/dados/FUNCIONA
```   
9. Crie do manifesto yaml do PersistentVolume
```bash
    sudo vim primeiro-pv.yaml
```
10. Altere o IP address do campo server para o IP address do node master
```bash
    apiVersion: v1
    kind: PersistentVolume
    metadata:
        name: primeiro-pv
    spec:
        capacity:
            storage: 1Gi
        accessModes:
        - ReadWriteMany
        persistentVolumeReclaimPolicy: Retain
    nfs:
        path: /opt/dados
        server: x.x.x.x
        readOnly: false
```
11. Crie o Persistent Volume 
```bash
    kubectl create -f primeiro-pv.yaml
```
12. Visualize mais detalhes do PV recém criado para confirmar correta configuração:
```bash
    kubectl describe pv primeiro-pv
```
13. Crie um PersitentVolumeClaim
```bash
    vim primeiro-pvc.yaml
```
14. Com o seguinte conteúdo:
```bash
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: primeiro-pvc
    spec:
      accessModes:
      - ReadWriteMany
      resources:
        requests:
        storage: 800Mi
```
15. Crie o PersitentVolumeClaim a partir do manifesto.
```bash
    k create -f primeiro-pvc.yaml
```
16. Liste o PersitentVolume
```bash
    k get pv
```
17. Liste o PersistentVolumeClaim
```bash
    k get pvc
```
18. Crie um deployment que irá consumir esse volume:
```bash
    vim nfs-pv.yaml
```
19. 
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    run: nginx
    name: nginx
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      run: nginx
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
        volumeMounts:
        - name: nfs-pv
          mountPath: /giropops
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      volumes:
      - name: nfs-pv
        persistentVolumeClaim:
          claimName: primeiro-pvc
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30    
```
20. Crie o deployment a partir do manifesto.
```bash
    kubectl create -f nfs-pv.yaml
```
21. listar os arquivos dentro do path no contêiner 
```bash
    kubectl exec -ti nginx-xxxxxxxxxxx -- ls /giropops/
```
## Efetuando a Limpeza do ambiente (caso necessário)
22. 
```bash
    k delete -f nfs-pv.yaml
    k delete -f primeiro-pvc.yaml
    k delete -f primeiro-pv.yaml 
```
23. Valide que nenhum artefato está presente no namespace `q4-ns`
```bash
    kubectl get all -A | grep -i q4-ns
```