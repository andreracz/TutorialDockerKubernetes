Dockerfile simples
==================

Para este tutorial, vamos mostrar como cadastrar passo a passo um Dockerfile simples, para rodar um servidor web. Para isso, vamos utilizar o NGINX, servidor web muito popular.

Começamos entrando no [Docker Hub](https://hub.docker.com/) e procurando a imagem do NGINX. O nome da imagem base é [nginx](https://hub.docker.com/_/nginx).

O Dockerfile que usaremos, está detalhado abaixo:

```Dockerfile
FROM nginx
COPY index.html /usr/share/nginx/html/index.html
RUN echo Olá mundo
```

Os comandos que utilizamos em nosso Dockerfile foram:

- ```FROM nginx```: Determina o container base, do qual o nosso deriva.
- ```COPY index.html /usr/share/nginx/html/index.html```: Copia um arquivo do diretório atual para o diretório do container
- ```RUN echo Olá mundo```: Roda um comando dentro do container durante a construção da imagem. Em geral é utilizado para construir a aplicação dentro do container.

Para fazer o build do container, utilizamos o comando: 

```Powershell
docker build --tag meu-container:1.0 .
```

Este comando faz faz o build do diretório corrente e nomeia o container com o nome: meu-container, e a "versão" 1.0.

Após rodar, podemos usar o comando: ```docker image list``` para listar as imagens presentes no nosso registro local. Para executar a aplicação, rodarmos:```docker run -d -p 3000:80 meu-container:1.0```
