# Questão 14

Identificar quais pods fazem parte de determinado services.

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
1. Crie o namespace `q14-ns`.
```bash
kcns q14-ns
ou
kubectl create namespace q14-ns
```
2. Mude o contexto para o namespace `q14-ns`, criado no passo anterior.
```bash
kctx q14-ns
ou
kubectl config set-context --current --namespace q14-ns
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

## Testando a solução
7. 
```bash
    
```
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
     
```
13. Valide que nenhum artefato está presente no namespace `q14-ns`
```bash
    kubectl get all -A | grep -i q14-ns
```