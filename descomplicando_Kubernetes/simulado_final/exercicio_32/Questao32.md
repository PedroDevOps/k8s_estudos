# Questão 32

Criar uma secret generic, outra from file e outra literal.

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
1. Crie o namespace `q32-ns`.
```bash
kcns q32-ns
ou
kubectl create namespace q32-ns
```
2. Mude o contexto para o namespace `q32-ns`, criado no passo anterior.
```bash
kctx q32-ns
ou
kubectl config set-context --current --namespace q32-ns
```
3. Confirme a mudança de contexto
```bash
kgtx
ou
kubectl config get-context
```

## Início da Solução
4. 
```bash
   
```
5. 
```bash
    
```
6. 
```bash
    
```
7. 
```bash

```

## Testando a solução
8. 
```bash
    
```
9. 
```bash
    
```
10. 
```bash
    
```
11. 
```bash
    
```

## Limpando ambiente (caso seja necessário)
12. Faça a limpesa do ambiente (Caso necessário)
```bash
     kubectl delete -f 
```
13. Valide que nenhum artefato está presente no namespace `q32-ns`
```bash
    kubectl get all -A | grep -i q32-ns
```

## Referencia
14. 