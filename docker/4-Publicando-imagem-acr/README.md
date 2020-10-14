Publicando uma imagem no ACR
============================

Após gerar a imagem local, podemos publicar ela no nosso ACR, para que outras pessoas possam utilizá-la, para isso, dever-se rodar o comando:

```docker tag my-container-app:1.0 meuacrunico.azurecr.io/my-container-app```

Esse comando tageia o container com o endereço do nosso ACR (meuacrunico.azurecr.io). Ele também coloca a tag latest, pois não colocamos versão.

Listando as imagens com ```docker image list``` obtemos algo do tipo:

```txt
REPOSITORY                                TAG                 IMAGE ID            CREATED             SIZE
my-container-app                          1.0                 13078efe0b7d        46 hours ago        105MB
meuacrunico.azurecr.io/my-container-app   latest              13078efe0b7d        46 hours ago        105MB
meu-container                             1.0                 fe8a3acc83f6        2 days ago          133MB
<none>                                    <none>              d52c90084304        2 days ago          133MB
docker/getting-started                    latest              1f32459ef038        2 months ago        26.8MB
```

Para enviar a imagem para o servidor, temos que rodar:

```docker push meuacrunico.azurecr.io/my-container-app```

Pronto! a imagem está no servidor e pode ser utilizada por quem tem acesso a ela!
