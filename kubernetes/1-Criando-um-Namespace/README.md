
Criando um namespace
====================

Um namespace é uma subdivisão no cluster kubernetes onde os outros objetos são deployados. Em geral, os objetos dentro de um mesmo namespace são acessíveis entre si.

Vamos mostrar duas formas de criar um namespace, uma usando somente linha de comando e outra usando arquivos YAML.

Para criar um namespace usando apenas a linha de comando, o comando é:

```Powershell
kubectl create namespace my-first-namespace
```

O comando acima cria um namespace chamado my-first-namespace, que pode ser listado com o comando:

```Powershell
kubectl get namespace
```

Para criar um Namespace utilizando arquivos YAML, o arquivo deve ter os seguintes dados, veja os comentários para saber o que cada campo significa:

```YAML
apiVersion: v1 # Obrigatorio, versão da API do Kubernetes que contém esse recurso
kind: Namespace # Tipo de objeto a ser criado
metadata:
  name: my-namespace #Nome do Namespace
  labels: # Labels a serem aplicados, podem ter quaisquer nomes e valores
    label1: a
    label2: b
```

Para criar o namespace, deve-se utilizar o seguinte comando:

```Powershell
kubectl apply -f .\my-namespace.yaml
```

O Comando apply cria ou altera um recurso, de acordo com o arquivo passado. Pode-se utilizar também o comando create, que apenas cria um objeto, não modificando caso ele exista.
