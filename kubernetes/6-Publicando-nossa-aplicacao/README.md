Publicando nossa aplicação
==========================

Para publicar a aplicação que criamos localmente no nosso cluster AKS, podemos gerar um YAML completo com todos os artefatos que precisamos, como abaixo (a separação de recursos é feita utilizando-se os caracteres ```---```):

```YAML
apiVersion: apps/v1 # Obrigatorio, versão da API do Kubernetes que contém esse recursos
kind: Deployment  # Tipo de objeto a ser criado
metadata:
  name: weather-forecast-deployment # Nome do deployment
  labels: # Labels a serem aplicados, podem ter quaisquer nomes e valores
    role: weather-forecast
spec: # contém as especificações do deployment pod, como os containers que irão rodar, quantidade de pods, etc..
  selector: # Indica como o Deployment vai saber quais pods gerenciar
    matchLabels: # Busca pods por labels
      app: weather-forecast # Pods que tenham o label app = getting-started
  replicas: 1 #roda uma replica da pod
  template: # Template da pod que será criada
    metadata:
      labels: # Labels. Devem ser iguais à MATCH EXPRESSION acima
        app: weather-forecast
    spec:
      containers: # Lista de containers da POD 
        - name: web # Nome do container dentro da POD
          image: meuacrunico.azurecr.io/my-container-app  # Imagem do Docker que será utilizado
          ports: # portas que a POD expões para o cluster. Essa porta não é acessível fora do cluster
            - name: web # Nome da porta
              containerPort: 80 # PORTA
              protocol: TCP # Tipo de protocolo (TCP ou UDP)
          resources: # Recursos que a POD usa (CPU, memória, etc...)
            limits: # Limite máximo de recursos
              cpu: 500m # 500 milis cpus (meia CPU) é o limite máximo que este container pode usar
              memory: 200Mi # 200 MB de memória é o  o limite máximo que este container pode usar
            requests: # limite mínimo que o container precisa
              cpu: 100m # 100 milis cpis (0,1 CPU) é o mínimo que este container precisa ter reservado
              memory: 100Mi # 100 MB de memória é o mínimimo que este container precisa ter reservado

---
apiVersion: v1 # Obrigatorio, versão da API do Kubernetes que contém esse recursos
kind: Service # Tipo de objeto a ser criado
metadata:
  name: weather-forecast-service  # Nome do service
spec:
  type: LoadBalancer # Tipo de Serviço. Os mais comuns são ClusterIP e LoadBalancer
  selector: # Indica quais pods serão roteadas por este service
    app: weather-forecast
  ports: # Portas que o serviço irá expor
    - protocol: TCP # Protocolo (TCP ou UDP)
      port: 80 # Porta que será exposta para o mundo
      targetPort: 80 # Porta da pod que será roteada

---
apiVersion: autoscaling/v2 # Obrigatorio, versão da API do Kubernetes que contém esse recursos
kind: HorizontalPodAutoscaler # Tipo de recurso (HPA)
metadata:
  name: weather-forecast-hpa #Nome do recurso
spec: # Especificação do recurso
  scaleTargetRef: # Referencia a objeto que será escalado
    apiVersion: apps/v1 # API do objeto a ser escalado
    kind: Deployment # Tipo do objeto a ser escalado
    name: weather-forecast-deployment # Nome do objeto a ser escalado 
  minReplicas: 1 # Numero minimo de replicas, deve ser o mesmo do numero de replicas do deployment
  maxReplicas: 5 # Numero máximo de replicas
  metrics: # Metrica usada para escalar
  - type: Resource # Tipo de métrica
    resource: # Recurso usado
      name: cpu # EScalar por CPU, quando a utilização média estiver acima de 50%
      target:
        type: Utilization
        averageUtilization: 50
```

Para aplicar esse arquivo no cluster, deve-se usar o comando apply, passando-se o arquivo e o nome do namespace onde será rodado.

```Powershell
kubectl apply -f aplicacao-completa.yaml --namespace=my-namespace
```

Para listar os recursos criado, pode-se usar os comandos:

```Powershell
kubectl get pod --namespace=my-namespace
kubectl get service --namespace=my-namespace
kubectl get deployment --namespace=my-namespace
kubectl get hpa --namespace=my-namespace
```

Com isso terminamos nosso tutorial básico sobre Kubernetes!

Para lipar tudo, podemos remover o namespace criado completamente, com o comando: ```kubectl delete namespace my-namespace``` . Isso remove todos os objetos do namespace.
