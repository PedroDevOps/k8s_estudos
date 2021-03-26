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

3. execute um dry run de criação de um pod para gerar o yaml e facilitar as demais configurações.
```bash
    kubectl run pod-q1 --image nginx --namespace q1-ns --dry-run=client -o yaml > pod-q2-dry-run.yaml
```
4.
5. 
6. 
7. 
8. 
9. 
10. Delete the Pod and the namespace.