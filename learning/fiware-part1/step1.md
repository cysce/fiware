Parte 1 - Iniciar FIWARE Orion

# Descripción general de Orion

![Orion](./assets/1-4.png)

El objetivo es lograr la siguiente arquitectura.

![Diagrama general](./assets/1-1.png)

# 1-1 Imagenes Docker necesarias

Primero, extraiga las imágenes de Docker necesarias de Docker Hub y cree una red a la que puedan conectarse nuestros contenedores:

    ```shell
    docker pull mongo
    docker pull fiware/orion
    docker network create fiware-net
    ```

# 1-2 Inicio de MongoDB

Primero, vamos a iniciar MongoDB, que se utiliza como base de datos para Orion.

![MongoDB](./assets/1-2.png)

Para iniciar MongoDB utilizaremos [Docker](https://www.docker.com/).

1. Inicia MongoDB con el siguiente comando:

   ```shell
   docker run --name mongo-db -d --network=fiware-net mongo --bind_ip_all
   ```

2. Cuando finalice, verifica los contenedores en ejecución con el siguiente comando:

   ```shell
   docker ps
   ```

   Si ves `mongodb` en la lista, el inicio fue exitoso.

# 1-3 Inicio de Orion

A continuación, iniciaremos el propio Orion.

![Orion](./assets/1-3.png)

Al igual que con MongoDB, puedes iniciar Orion usando la imagen de FIWARE publicada en DockerHub.

1. Inicia Orion con el siguiente comando:

   ```shell
   docker run -d --name orion --network=fiware-net -p 1026:1026 fiware/orion -dbURI mongodb://mongo-db
   ```

2. Cuando finalice, verifica los contenedores en ejecución con el siguiente comando:

   ```shell
   docker ps
   ```

   Si ves `orion` y `mongo-db` en la lista, el inicio fue exitoso.


[Ir al paso 2](step2.md)
