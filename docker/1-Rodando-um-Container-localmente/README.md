Rodando um Container Localmente
===============================

Para rodar um container localmente, deve-se utilizar o seguinte comando:

```PowerShell
docker run -d -p 3000:80 docker/getting-started
```

Este comando faz o dowload da imagem *docker/getting-started* do Docker HUB, roda ela localmente no docker em modo detached, mapeando a porta 3000 do seu computador para a porta 80 do container.

Explicando o comando:

- ```docker```: Roda o comando Docker
- ```run```: Subcomando para indicar que um container novo deve ser rodado
- ```-d```: Rodar em modo dettached (em background)
- ```-p 3000:80```: Mapeia a porta 3000 do seu computador para a porta 80 do container
- ```docker/getting-started```: Imagem a ser rodada

Listando os containers rodando
------------------------------

O subcomando ps lista os containers rodando:

```PowerShell
docker ps
```

Este comando retorna algo do tipo:

```text
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                  NAMES
df651be259f3        docker/getting-started   "/docker-entrypoint.…"   11 minutes ago      Up 11 minutes       0.0.0.0:3000->80/tcp   frosty_spence
```

Matando um container
--------------------

O subcomando stop para um container que esteja rodando, usando seu ID, mas o container continua registrado:

```PowerShell
docker stop df651be259f3
```

Se rodarmos ```docker ps``` novamente, veremos que o container não está mais rodando, mas rodando ```docker ps -a```, o container será listado.

Para apagarmos o container precisar rodar o comando: ```docker rm df651be259f3```, onde o segundo parametro é o ID do container.
