Rodando um Deployment
=====================

Um deployment roda um conjunto (cluster) de pods, mantendo sempre o número de cópias (replicas) necessários para rodar, conforme especificado.

A criação de pods é feita utilizando-se arquivos YAML, como o que está listado a seguir:

```YAML
apiVersion: apps/v1 # Obrigatorio, versão da API do Kubernetes que contém esse recursos
kind: Deployment  # Tipo de objeto a ser criado
metadata:
  name: pod-info-deployment #Nome do deployment
  labels: # Labels a serem aplicados, podem ter quaisquer nomes e valores
    role: pod-info
spec: # contém as especificações do deployment pod, como os containers que irão rodar, quantidade de pods, etc..
  selector: # Indica como o Deployment vai saber quais pods gerenciar
    matchLabels: # Busca pods por labels
      app: pod-info # Pods que tenham o label app = getting-started
  replicas: 2 #roda duas replicas da pod
  template: # Template da pod que será criada 
    metadata: 
      labels: # Labels. Devem ser iguais à MATCH EXPRESSION acima
        app: pod-info
    spec:
      containers: # Lista de containers da POD 
        - name: web # Nome do container dentro da POD
          image: stefanprodan/podinfo  # Imagem do Docker que será utilizado
          ports: # portas que a POD expões para o cluster. Essa porta não é acessível fora do cluster
            - name: web # Nome da porta
              containerPort: 9898 # PORTA
              protocol: TCP # Tipo de protocolo (TCP ou UDP)
          resources: # Recursos que a POD usa (CPU, memória, etc...)
            limits: # Limite máximo de recursos
              cpu: 500m # 500 milis cpus (meia CPU) é o limite máximo que este container pode usar
              memory: 200Mi # 200 MB de memória é o  o limite máximo que este container pode usar
            requests: # limite mínimo que o container precisa
              cpu: 100m # 100 milis cpis (0,1 CPU) é o mínimo que este container precisa ter reservado
              memory: 100Mi # 100 MB de memória é o mínimimo que este container precisa ter reservado
```

Para aplicar esse arquivo no cluster, deve-se usar o comando apply, passando-se o arquivo e o nome do namespace onde será rodado.

```Powershell
kubectl apply -f .\pod-info-deployment.yaml --namespace=my-namespace
```

Para listar as Pods, deve-se rodar o comando:

```Powershell
kubectl get pod --namespace=my-namespace
```

Podemos ver, que estamos rodando duas instancias da pod, por exemplo:

```
NAME                                   READY   STATUS    RESTARTS   AGE
pod-info-deployment-7945c979c7-bm8r9   1/1     Running   0          23s
pod-info-deployment-7945c979c7-gt4q5   1/1     Running   0          21s
```

Ambas as Pods estão expondo a porta 9898 para o cluster, porém para acessá-la de fora é necessário criar um port-forward, utilizando-se os comandos em duas janelas separadas:

```Powershell
kubectl port-forward pod-info-deployment-7945c979c7-bm8r9 3002:9898 --namespace=my-namespace
kubectl port-forward pod-info-deployment-7945c979c7-gt4q5 3003:9898 --namespace=my-namespace
```

Assim, expomos a porta 9898 do container como a porta 3002 do computador local.

Podemos também alterar diretamente o número de pods de um deployment, com o comando:

```Powershell
kubectl scale --replicas=4 deployment/pod-info-deployment --namespace=my-namespace
```

Se apagarmos uma pod, podemos ver que o Kubernetes automaticamente sobe mais um, por exemplo:

```Powershell
kubectl delete pod pod-info-deployment-7945c979c7-bm8r9 --namespace=my-namespace
```

Vamos deixar o nosso deployment rodando para podermos utilizar no próximo exemplo.