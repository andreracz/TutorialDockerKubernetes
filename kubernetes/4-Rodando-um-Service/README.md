Rodando um Service
==================

Um service expõe um conjunto de Pods ou deployment como um serviço único, seja para o Cluster ou para o mundo exterior.

A criação de Services é feita utilizando-se arquivos YAML, como o que está listado a seguir:

```YAML
apiVersion: v1 # Obrigatorio, versão da API do Kubernetes que contém esse recursos
kind: Service # Tipo de objeto a ser criado
metadata:
  name: pod-info-service  # Nome do service
spec:
  type: ClusterIP # Tipo de Serviço. Os mais comuns são ClusterIP e LoadBalancer
  selector: # Indica quais pods serão roteadas por este service
    app: pod-info
  ports: # Portas que o serviço irá expor
    - protocol: TCP # Protocolo (TCP ou UDP)
      port: 80 # Porta que será exposta para o mundo
      targetPort: 9898 # Porta da pod que será roteada
```

Para aplicar esse arquivo no cluster, deve-se usar o comando apply, passando-se o arquivo e o nome do namespace onde será rodado.

```Powershell
kubectl apply -f .\pod-info-service.yaml --namespace=my-namespace
```

Para listar os serviços, deve-se rodar o comando:

```Powershell
kubectl get service --namespace=my-namespace
```

Por ter o tipo ClusterIP, o serviço só está exposto para dentro do cluster:

```
NAME               TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
pod-info-service   ClusterIP   10.0.45.23   <none>        80/TCP    19s
```

O serviço está expondo a porta 80, porém para acessá-la de fora é necessário criar um port-forward, utilizando-se o comando:

```Powershell
kubectl port-forward service/pod-info-service 3004:80 --namespace=my-namespace
```

Assim, expomos a porta 80 do service como a porta 3003 do computador local.

Podemos também alterar o Service para que ele crie um Load Balancer em um IP Externo, assim não temos que fazer o port forward. Para isso, modificamos o Type para "LoadBalancer" e rodamos o commando apply:

```Powershell
kubectl apply -f .\pod-info-service-load-balancer.yaml --namespace=my-namespace
```

Listando novamente o serviço com o comando ```kubectl get service --namespace=my-namespace```, obtemos o IP Externo:

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)        AGE
pod-info-service   LoadBalancer   10.0.45.23   52.224.196.160   80:31710/TCP   12m
```

Podemos acessar pelo IP na porta 80.