# Questão 1

Criar um pod com um volume não persistente.

## Criando um Pod com um volume não persistente

1. Crie o namespace `q1_ns`.
```bash
kubectl create namespace q1_ns
```
2. Dentro do namespace `q1_ns` crie um pod com os seguintes atributos:
* name = pod_q1
* image = nginx

3. execute um dry run de criação de um pod para gerar o yaml e facilitar as demais configurações.
```bash
    kubectl run pod_q1 --image=nginx -namespace=q1_ns --dry-run=client -o yaml > pod.yaml
```
4.
5. 
6. 
7. 
8. 
9. 
10. Delete the Pod and the namespace.