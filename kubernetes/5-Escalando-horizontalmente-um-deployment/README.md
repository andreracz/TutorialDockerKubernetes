Escalando Horizontalmente um deployment
=======================================

Vamos agora mostrar como um HorizontalPodAutoscaler (HPA) pode ser utilizado para escalar o deployment automaticamente por métricas como CPU, memória, etc... 

A criação de Services é feita utilizando-se arquivos YAML, como o que está listado a seguir:

```YAML
apiVersion: autoscaling/v2beta2 # Obrigatorio, versão da API do Kubernetes que contém esse recursos
kind: HorizontalPodAutoscaler # Tipo de recurso (HPA)
metadata:
  name: pod-info-hpa #Nome do recurso
spec: # Especificação do recurso
  scaleTargetRef: # Referencia a objeto que será escalado
    apiVersion: apps/v1 # API do objeto a ser escalado
    kind: Deployment # Tipo do objeto a ser escalado
    name: pod-info-deployment # Nome do objeto a ser escalado
  minReplicas: 4 # Numero minimo de replicas, deve ser o mesmo do numero de replicas do deployment
  maxReplicas: 10 # Numero máximo de replicas
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
kubectl apply -f .\pod-info-hpa.yaml --namespace=my-namespace
```

Para listar o recurso criado, deve-se rodar o comando:

```Powershell
kubectl get hpa --namespace=my-namespace
```
