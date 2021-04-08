# Questão 15

Usar o nslookup e/ou outras ferramentas para pegar o dns do pod e do service.

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
1. Crie o namespace `q15-ns`.
```bash
    kcns q15-ns
    ou
    kubectl create namespace q15-ns
```
2. Mude o contexto para o namespace `q15-ns`, criado no passo anterior.
```bash
    kctx q15-ns
    ou
    kubectl config set-context --current --namespace q15-ns
```
3. Confirme a mudança de contexto
```bash
    kgtx
    ou
    kubectl config get-context
```

## Início da Solução
4. Crie arquivos yaml para um deployment e um pod usando a image do nginx
```bash
    kubectl create -f https://k8s.io/examples/admin/dns/dnsutils.yaml --dry-run=client -o yaml > dnsutils-pod-q15-dry-run.yaml
    kubectl create -f https://k8s.io/examples/controllers/nginx-deployment.yaml --dry-run=client -o yaml > deployment-pods-q15-dry-run.yaml
    kubectl run pod-nginx-q15 --image nginx --dry-run=client -o yaml > pod-nginx-q15-dry-run.yaml
```
5. Faça copias dos arquivos para deixar os originais como BACKUP
```bash
    cp dnsutils-pod-q15-dry-run.yaml dnsutils-pod-q15.yaml
    cp pod-nginx-q15-dry-run.yaml pod-nginx-q15.yaml
    cp deployment-pods-q15-dry-run.yaml deployment-pods-q15.yaml
```
6. Edite-os (Caso Necessário) e remova o namespace default
```bash
    vi dnsutils-pod-q15.yaml
    vi pod-nginx-q15.yaml
    vi deployment-pods-q15.yaml
```

## Testando a solução
8. Faça a criação do deployment e do pod
```bash
    kubectl create -f dnsutils-pod-q15.yaml
    kubectl create -f deployment-pods-q15.yaml
    kubectl create -f pod-nginx-q15.yaml
```
9. Crie arquivos yaml para um service
```bash
    kubectl expose pod dnsutils --port=80 --name svc-dnsutils-q15 --dry-run=client -o yaml > service-dnsutils-q15-dry-run.yaml
    kubectl expose pod pod-nginx-q15 --port=80 --name svc-pod-nginx-q15 --dry-run=client -o yaml > service-nginx-q15-dry-run.yaml
    kubectl expose deploy nginx-deployment --name svc-deploy-nginx-q15 --dry-run=client -o yaml > service-deploy-nginx-q15-dry-run.yaml
```
10. Faça copia do arquivo para deixar o original como BACKUP
```bash
    cp service-dnsutils-q15-dry-run.yaml service-dnsutils-q15.yaml
    cp service-deploy-nginx-q15-dry-run.yaml service-deploy-nginx-q15.yaml
    cp service-nginx-q15-dry-run.yaml service-nginx-q15.yaml
```
11. Edite-o (Caso Necessário) e remova o namespace default
```bash
    vim service-dnsutils-q15.yaml
    vim service-nginx-q15.yaml
    vim service-deploy-nginx-q15.yaml
```
11. Edite-o (Caso Necessário) e remova o namespace default
```bash
    kubectl create -f service-dnsutils-q15.yaml
    kubectl create -f service-nginx-q15.yaml
    kubectl create -f service-deploy-nginx-q15.yaml
```

kubectl exec -i -t dnsutils -- nslookup <$POD>.<$NAMESPACE>.pod.cluster.local
kubectl exec -i -t dnsutils -- nslookup 
12. 
```bash
    # kubectl exec -i -t dnsutils -- nslookup <service-name>.<namespace>
    kubectl exec -i -t dnsutils -- nslookup kubernetes.default
```   
11. 
```bash
    # kubectl exec -it dnsutils -- nslookup <service-name>
    kubectl exec -it dnsutils -- nslookup svc-dnsutils-q15
    kubectl exec -it dnsutils -- nslookup svc-pod-nginx-q15
    kubectl exec -it dnsutils -- nslookup svc-deploy-nginx-q15
```
## Troubleshoting (caso necessário)
10. 
```bash
    kubectl exec -ti dnsutils -- cat /etc/resolv.conf
```
12. Check if the DNS pod is running
```bash
    kubectl get pods --namespace=kube-system -l k8s-app=kube-dns
```
13. Check for errors in the DNS pod
```bash
    kubectl logs --namespace=kube-system -l k8s-app=kube-dns
```
15. 
```bash
    kubectl get svc --namespace=kube-system
```
15. 
```bash
    kubectl get endpoints kube-dns --namespace=kube-system
```

## Limpando ambiente (caso seja necessário)
16. Faça a limpesa do ambiente (Caso necessário)
```bash
    kubectl delete -f service-dnsutils-q15.yaml
    kubectl delete -f service-nginx-q15.yaml
    kubectl delete -f service-deploy-nginx-q15.yaml
    kubectl delete -f dnsutils-pod-q15.yaml
    kubectl delete -f deployment-pods-q15.yaml
    kubectl delete -f pod-nginx-q15.yaml
```
17. Valide que nenhum artefato está presente no namespace `q15-ns`
```bash
    kubectl get all
    ou
    kubectl get all -A | grep -i q15-ns
```