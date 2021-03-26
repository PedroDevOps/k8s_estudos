# Questão 2

Criar um service/ep apontando para um pod.

## Criando um service/ep apontando para um pod.

1. Crie o namespace `q2-ns`.
```bash
kubectl create namespace q2-ns
```
2. Dentro do namespace `q2-ns` crie um pod com os seguintes atributos:
* name = pod-q2
* image = nginx

3. Execute um dry run de criação de um pod para gerar o yaml e facilitar as demais configurações.
```bash
    kubectl run pod-q2 --image nginx --n q1-ns --dry-run=client -o yaml > pod-q2-dry-run.yaml
```
4. Faça uma copia do pod-q2-dry-run.yaml com o nome pod-q2.yaml
```bash
    cp pod-q2-dry-run.yaml pod-q2.yaml
```
5. Execute o comando de criação do pod passando como parâmetro o arquivo pod-q2.yaml
```bash
    kubectl create -f pod-q2.yaml
```
6. Após criação, certifique que o pod escontrasse em estado de runnig.
```bash
    kubectl get pod pod-q2.yaml -n q2-ns
```
7. Valide que o pod foi criado com os valores corretos.
```bash
    kubectl describe pod-q2 -n q2-ns
```

## Espodo a porta do pod por meio de um service
8. Execute um dry run de expose de portas de um pod para gerar o yaml e facilitar a vizualização das configurações.
```bash
    kubectl expose pod pod-q2 -n q2-ns --dry-run=client -o yaml > pod-q2-service-dry-run.yaml
```


## Testando se criação ocorreu com sucesso

11. Verifique se o Pod está no estado de runnig e se encontra no namespace correto
```bash
    kubectl get pods --namespace q1-ns

