Rodando uma pod
===============

Uma Pod é um conjunto de containers, que rodam isolados no cluster. Os containers rodam em conjunto e tem acesso ao mesmo sistema de arquivos.

A criação de pods é feita utilizando-se arquivos YAML, como o que está listado a seguir:

```YAML
apiVersion: v1 # Obrigatorio, versão da API do Kubernetes que contém esse recursos
kind: Pod  # Tipo de objeto a ser criado
metadata:
  name: getting-started #Nome da pod
  labels: # Labels a serem aplicados, podem ter quaisquer nomes e valores
    role: myrole
spec: # contém as especificações da pod, como os containers que irão rodar, recursos, discos, etc...
  containers: # Lista de containers da POD
    - name: web # Nome do container dentro da POD
      image: docker/getting-started # Imagem do Docker que será utilizado
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
```

Para aplicar esse arquivo no cluster, deve-se usar o comando apply, passando-se o arquivo e o nome do namespace onde será rodado.

```Powershell
kubectl apply -f .\getting-started.yaml --namespace=my-namespace
```

Para listar as Pods, deve-se rodar o comando:

```Powershell
kubectl get pod --namespace=my-namespace
```

A Pod está expondo a porta 80 para o cluster, porém para acessá-la de fora é necessário criar um port-forward, utilizando-se o comando:

```Powershell
kubectl port-forward getting-started 3001:80 --namespace=my-namespace
```

Assim, expomos a porta 80 do container como a porta 3001 do computador local.

Por fim, para limpar o ambiente, podemos apagar a pod, com o comando:

```Powershell
kubectl delete pod getting-started --namespace=my-namespace
```
