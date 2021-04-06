# Questão 12

Fazer a instalação do nginx em determinada versão, atualizar e depois realizar o rollback com o --record.

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
1. Crie o namespace `q12-ns`.
```bash
kcns q12-ns
ou
kubectl create namespace q12-ns
```
2. Mude o contexto para o namespace `q12-ns`, criado no passo anterior.
```bash
kctx q12-ns
ou
kubectl config set-context --current --namespace q12-ns
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
13. Valide que nenhum artefato está presente no namespace `q12-ns`
```bash
    kubectl get all -A | grep -i q12-ns
```