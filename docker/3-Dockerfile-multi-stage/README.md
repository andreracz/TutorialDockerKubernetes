Dockerfile Multi-Stage
======================

Para este tutorial, vamos mostrar como usar um Dockerfile com multiplos passos, para rodar um serviço .Net Core.

O projeto está criado no diretório ```my-container-app``` e os comandos abaixo devem ser rodados neste diretório.

Usamos Dockerfile multi-stage em geral quando temos versões diferentes de containers para construção da aplicação e para rodar ela,por exemplo para projetos .Net Core, usamos uma imagem com o SDK completo para fazer o build, e uma imagem com apenas o Runtime para fazer a execução. Com isso, nossa imagem de execução é menor e mais segura, pois não precisa ter o código fonte.

O Dockerfile que usaremos, está detalhado abaixo:

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-alpine AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# copy everything else and build app
COPY . ./
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-alpine AS runtime
WORKDIR /app
EXPOSE 80
COPY --from=build /app/out ./
ENTRYPOINT ["dotnet", "my-container-app.dll"]
```

Os comandos que utilizamos em nosso Dockerfile foram:

- Container de Build:
  - ```FROM mcr.microsoft.com/dotnet/core/sdk:3.1-alpine AS build```: Determina o container base, do qual o nosso deriva, e coloca o nome do stage como build. Esse container será usado somente para a construção, sendo depois descartado.
  - ```WORKDIR /app```: Muda o diretório de trabalho para o diretório /app do container.
  - ```COPY *.csproj ./```: Copia apenas o arquivo CSPROJ, para depois poder restaurar as dependencias. Apenas esse arquivo é copiado, pois conseguimos otimizar as versões intermediárias em cache, diminuindo o espaço em disco.
  - ```RUN dotnet restore```: Restaura as bibliotecas no container de build.
  - ```COPY . ./```: Copia o restante do código fonte.
  - ```RUN dotnet publish -c Release -o out```: Gera o build, no perfil Release
- Container de Runtime:
  - ```FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-alpine AS runtime```: Cria uma novo container, com apelido de "runtime", usando apenas o runtime do .Net.
  - ```WORKDIR /app```: Muda o diretório de trabalho para /app
  - ```EXPOSE 80```: Determina qual porta o Container expõe
  - ```COPY --from=build /app/out ./```: Copia os arquivos do diretório /app/out do container de build para o container de runtime
  - ```ENTRYPOINT ["dotnet", "my-container-app.dll"]```: Determina o comando que vai ser rodado quando o container subir, para rodar a aplicação.

Para fazer o build do container, utilizamos o comando:

```Powershell
docker build --tag my-container-app:1.0 .
```

Este comando faz faz o build do diretório corrente e nomeia o container com o nome: my-container-app, e a "versão" 1.0.

Após rodar, podemos usar o comando: ```docker image list``` para listar as imagens presentes no nosso registro local. Para executar a aplicação, rodarmos:```docker run -d -p 3000:80 my-container-app:1.0```

Para acessar, use a URL: http://localhost:3000/weatherforecast
