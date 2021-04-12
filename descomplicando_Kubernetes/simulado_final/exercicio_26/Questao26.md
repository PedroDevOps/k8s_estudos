# Questão 26

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
1. Crie o namespace `q26-ns`.
```bash
kcns q26-ns
ou
kubectl create namespace q26-ns
```
2. Mude o contexto para o namespace `q26-ns`, criado no passo anterior.
```bash
kctx q26-ns
ou
kubectl config set-context --current --namespace q26-ns
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
    kubectl create deploy deploy-busybox --image busybox --replicas 5 --dry-run=client -o yaml > deploy-busybox-q26-dry-run.yaml
```
5. Copiar para outro arquivo .yaml
```bash
    cp deploy-busybox-q26-dry-run.yaml deploy-busybox-q26.yaml
```
5. 
```bash
    vi deploy-busybox-q26.yaml
```
6. 
```bash
  containers:
  - name: busybox
    image: busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
    readinessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```
6. Caso seja necessário altere o deploy-busybox.yaml, e depois execute-o
```bash
   k create -f deploy-busybox-q26.yaml
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
     kubectl delete -f deploy-busybox-q26.yaml
```
13. Valide que nenhum artefato está presente no namespace `q26-ns`
```bash
    kubectl get all -A | grep -i q26-ns
```

## Referencia
14. https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/