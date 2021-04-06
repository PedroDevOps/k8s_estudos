# Questão 11

Criar um secret e dois pods, um montando o secret em filesystem e outro como variável

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
1. Crie o namespace `q11-ns`.
```bash
kcns q11-ns
ou
kubectl create namespace q11-ns
```
2. Mude o contexto para o namespace `q11-ns`, criado no passo anterior.
```bash
kctx q11-ns
ou
kubectl config set-context --current --namespace q11-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução
4. Crie um arquivo qualquer para funcionar com secret
```bash
    echo -n "Arquivo criado para ser acessado via filesystem" > secret.txt
```
5. Crie um secret no kubernetes
```bash
    kubectl create secret generic my-secret --from-file=secret.txt
```
6. Confirme a criação do secret 
```bash
   k get secret
   k describe secret my-secret
```
7. Ger um arquivo yaml a partir do secret presente no kubernetes
```bash
    kubectl get secret my-secret -o yaml > secret_no_filesystem.yaml
```
8. Decodifique o valor presente no secret e valide-o
```bash
    echo 'QXJxdWl2byBjcmlhZG8gcGFyYSBzZXIgYWNlc3NhZG8gdmlhIGZpbGVzeXN0ZW0=' | base64 --decode 
```   

## Testando a solução
9. 
```bash
    kubectl run busy --image busybox --dry-run=client -o yaml > pod-secret-filesystem-q11-dry-run.yaml
```
10. 
```bash
    cp pod-secret-filesystem-q11-dry-run.yaml pod-secret-filesystem-q11.yaml
    vi pod-secret-filesystem-q11.yaml 
```
11. 
```bash
apiVersion: v1
kind: Pod
metadata:
  name: test-secret-filesystem
spec:
  containers:
  - image: busybox
    name: busy
    command:
      - sleep
      - "3600"
    volumeMounts:
    - mountPath: /tmp/secrets
      name: my-volume-secret
  volumes:
  - name: my-volume-secret
    secret:
      secretName: my-secret
```
12. 
```bash
     kubectl create -f pod-secret-filesystem-q11.yaml
```
12. 
```bash
    kubectl exec -ti test-secret-filesystem -- ls /tmp/secrets
```
12. 
```bash
    kubectl exec -ti test-secret-filesystem -- cat /tmp/secrets/secret.txt
```

12. 
```bash
    kubectl create secret generic my-literal-secret --from-literal user=pedrodevops --from-literal password=devops123# -o yaml > secret_na_variavel.yaml
```
12. 
```bash
     kubectl run teste-secret-var --image busybox --dry-run=client -o yaml > pod-secret-variavel-q11-dry-run.yaml
```
12. 
```bash
    cp pod-secret-variavel-q11-dry-run.yaml pod-secret-variavel-q11.yaml
    vi pod-secret-variavel-q11.yaml
```
13. 
```bash
apiVersion: v1
kind: Pod
metadata:
  name: test-secret-env
  namespace: default
spec:
  containers:
  - image: busybox
    name: busy-secret-env
    command:
      - sleep
      - "3600"
    env:
    - name: MEU_USERNAME
      valueFrom:
        secretKeyRef:
          name: my-literal-secret
          key: user
    - name: MEU_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-literal-secret
          key: password
```

12. 
```bash
    kubectl create -f pod-secret-variavel-q11.yaml
```
12. 
```bash
    kubectl exec test-secret-env -c busy-secret-env -it -- printenv
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f pod-secret-filesystem-q11.yaml
     kubectl delete -f pod-secret-variavel-q11.yaml
     kubectl delete secret my-secret
     kubectl delete secret my-literal-secret
```
13. Valide que nenhum artefato está presente no namespace `q11-ns`
```bash
    kubectl get all -A | grep -i q11-ns
```