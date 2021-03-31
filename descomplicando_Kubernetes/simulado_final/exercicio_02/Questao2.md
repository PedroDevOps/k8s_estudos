# Questão 2

Criar um service/ep apontando para um pod.

## Configuração Previa
0. alguns alias recomendados:
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
1. Crie o namespace `q2-ns`.
```bash
kcns q2-ns
ou
kubectl create namespace q2-ns
```
2. Mude o contexto para o namespace `q2-ns`, criado no passo anterior.
```bash
kctx q2-ns
ou
kubectl config set-context --current --namespace q2-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução

4. Crie um pod com os seguintes atributos:
* name = pod-q2
* image = nginx

5. Execute um dry run de criação de um pod para gerar o yaml e facilitar as demais configurações.
```bash
    kubectl run pod-q2 --image nginx --dry-run=client -o yaml > pod-q2-dry-run.yaml
```
6. Faça uma copia do pod-q2-dry-run.yaml com o nome pod-q2.yaml
```bash
    cp pod-q2-dry-run.yaml pod-q2.yaml
```
7. Execute o comando de criação do pod passando como parâmetro o arquivo pod-q2.yaml
```bash
    kubectl create -f pod-q2.yaml
```
8. Após criação, certifique que o pod está em estado de runnig.
```bash
    kubectl get pods
```
9. Valide que o pod foi criado com os valores corretos.
```bash
    kubectl describe pod-q2
```
10. Execute um dry run de expose de portas de um pod para gerar o yaml e facilitar a vizualização das configurações.
```bash
    kubectl expose pod pod-q2 --dry-run=client -o yaml > service-q2-dry-run.yaml
```
11. Faça uma copia do service-q2-dry-run com o nome service-q2.yaml
```bash
    cp service-q2-dry-run.yaml service-q2.yaml
```
12. Execute o comando de criação do service passando como parâmetro o arquivo service-q2.yaml
```bash
    kubectl create -f service-q2.yaml
```

## Testando a solução
13. Verifique se o Pod está no estado de runnig e se encontra no namespace correto
```bash
    kubectl get pods
```
14. Verifique se o Service está executando e se está no namespace correto
```bash
    kubectl get services
```
15. De dentro do cluster tente acessar o IP presente no clusterIP do service service-q2
```bash
    curl <IP do ClusterIP>
```

## Efetuando a Limpeza do ambiente (caso necessário)
16. 
```bash
    k delete -f pod-q2.yaml
    k delete -f service-q2.yaml
```
17. Valide que nenhum artefato está presente no namespace `q2-ns`
```bash
    kubectl get all -A | grep -i q2-ns
```