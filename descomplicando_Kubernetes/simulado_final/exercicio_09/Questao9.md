# Questão 9

Organizar a saída do comando "kubectl get pods" pelo tamanho do capacity storage.

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
1. Crie o namespace `q9-ns`.
```bash
    kcns q9-ns
    ou
    kubectl create namespace q9-ns
```
2. Mude o contexto para o namespace `q9-ns`, criado no passo anterior.
```bash
    kctx q9-ns
    ou
    kubectl config set-context --current --namespace q9-ns
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
    sudo mkdir /opt/diversos
    sudo chmod 1777 /opt/diversos/
```
5. Adicione esse diretório no NFS Server
```bash
    sudo vim /etc/exports
    /opt/diversos *(rw,sync,no_root_squash,subtree_check)
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
    sudo touch /opt/diversos/COMECANCO
```   
9. Crie do manifesto yaml do PersistentVolume
```bash
    sudo vim pequeno-pv-q9.yaml
```
10. Altere o IP address do campo server para o IP address do node master
```bash
    apiVersion: v1
    kind: PersistentVolume
    metadata:
        name: pequeno-pv
    spec:
        capacity:
            storage: 1Gi
        accessModes:
        - ReadWriteMany
        persistentVolumeReclaimPolicy: Retain
    nfs:
        path: /opt/diversos
        server: x.x.x.x
        readOnly: false
```
11. Crie do manifesto yaml do PersistentVolume
```bash
    sudo vim medio-pv-q9.yaml
```
12. Altere o IP address do campo server para o IP address do node master
```bash
    apiVersion: v1
    kind: PersistentVolume
    metadata:
        name: medio-pv
    spec:
        capacity:
            storage: 2Gi
        accessModes:
        - ReadWriteMany
        persistentVolumeReclaimPolicy: Retain
    nfs:
        path: /opt/diversos
        server: x.x.x.x
        readOnly: false
```
13. Crie do manifesto yaml do PersistentVolume
```bash
    sudo vim grande-pv-q9.yaml
```
14. Altere o IP address do campo server para o IP address do node master
```bash
    apiVersion: v1
    kind: PersistentVolume
    metadata:
        name: grande-pv
    spec:
        capacity:
            storage: 4Gi
        accessModes:
        - ReadWriteMany
        persistentVolumeReclaimPolicy: Retain
    nfs:
        path: /opt/diversos
        server: x.x.x.x
        readOnly: false
```
15. Crie os Persistent Volumes 
```bash
    kubectl create -f pequeno-pv-q9.yaml
    kubectl create -f medio-pv-q9.yaml
    kubectl create -f grade-pv-q9.yaml
```
16. Visualize mais detalhes do PV recém criado para confirmar correta configuração:
```bash
    kubectl describe pv
```
17. Crie um PersitentVolumeClaim pequeno
```bash
    vim pequeno-pvc-q9.yaml
```
18. Com o seguinte conteúdo:
```bash
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: pequeno-pvc
    spec:
      accessModes:
      - ReadWriteMany
      resources:
        requests:
        storage: 200Mi
```
19. Crie um PersitentVolumeClaim médio
```bash
    vim medio-pvc-q9.yaml
```
20. Com o seguinte conteúdo:
```bash
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: medio-pvc
    spec:
      accessModes:
      - ReadWriteMany
      resources:
        requests:
        storage: 900Mi
```
21. Crie um PersitentVolumeClaim grande
```bash
    vim grande-pvc-q9.yaml
```
22. Com o seguinte conteúdo:
```bash
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: grande-pvc
    spec:
      accessModes:
      - ReadWriteMany
      resources:
        requests:
        storage: 1500Mi
```
23. Crie os PersitentVolumeClaim a partir dos manifestos.
```bash
    k create -f pequeno-pvc-q9.yaml
    k create -f medio-pvc-q9.yaml
    k create -f grande-pvc-q9.yaml
```
24. Liste os PersitentVolume
```bash
    k get pv
```
25. Liste os PersistentVolumeClaim
```bash
    k get pvc
```
26. Crie um deployment que irá consumir o pequeno PV e PVC:
```bash
    vim pequeno-deploy-nfs-pv-q9.yaml
```
27. 
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    run: nginx-pequeno
    name: nginx-pequeno
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      run: nginx-pequeno
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        run: nginx-pequeno
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
        volumeMounts:
        - name: pequeno-pv
          mountPath: /shared
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      volumes:
      - name: pequeno-pv
        persistentVolumeClaim:
          claimName: pequeno-pvc
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30    
```
28. Crie um deployment que irá consumir o medio PV e PVC:
```bash
    vim medio-deploy-nfs-pv-q9.yaml
```
29. 
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    run: nginx-medio
    name: nginx-medio
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      run: nginx-medio
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        run: nginx-medio
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
        volumeMounts:
        - name: medio-pv
          mountPath: /shared
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      volumes:
      - name: medio-pv
        persistentVolumeClaim:
          claimName: medio-pvc
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30    
```
30. Crie um deployment que irá consumir o grande PV e PVC:
```bash
    vim grande-deploy-nfs-pv-q9.yaml
```
31. 
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    run: nginx-grande
    name: nginx-grande
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      run: nginx-grande
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        run: nginx-grande
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
        volumeMounts:
        - name: grande-pv
          mountPath: /shared
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      volumes:
      - name: grande-pv
        persistentVolumeClaim:
          claimName: grande-pvc
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30    
```
32. Crie o deployment a partir dos manifesto.
```bash
    kubectl create -f pequeno-deploy-nfs-pv-q9.yaml
    kubectl create -f medio-deploy-nfs-pv-q9.yaml
    kubectl create -f grande-deploy-nfs-pv-q9.yaml
```
33. listar os arquivos dentro do path no contêiner 
```bash
    kubectl exec -ti nginx-pequeno-xxx -- ls /shared/
    kubectl exec -ti nginx-medio-xxxx -- ls /shared/
    kubectl exec -ti nginx-grande-xxx -- ls /shared/
```

34. listar os arquivos dentro do path no contêiner 
```bash
    kubectl get pods --sort-by=.spec.capacity.storage
    kubectl get pv --sort-by=.spec.capacity.storage
```

## Efetuando a Limpeza do ambiente (caso necessário)
35. 
```bash
    kubectl delete -f pequeno-deploy-nfs-pv-q9.yaml
    kubectl delete -f medio-deploy-nfs-pv-q9.yaml
    kubectl delete -f grande-deploy-nfs-pv-q9.yaml
    kubectl delete -f pequeno-pvc-q9.yaml
    kubectl delete -f medio-pvc-q9.yaml
    kubectl delete -f grande-pvc-q9.yaml
    kubectl delete -f pequeno-pv-q9.yaml
    kubectl delete -f medio-pv-q9.yaml
    kubectl delete -f grade-pv-q9.yaml

   
```
36. Valide que nenhum artefato está presente no namespace `q9-ns`
```bash
    kubectl get all -A | grep -i q9-ns
```