En la Parte 4, aprenderemos a crear datos históricos utilizando FIWARE Cygnus.

# Descripción general de Cygnus

![Descripción general de Cygnus](./assets/4-0.png)

# 1-1 Inicio de la configuración

En esta ocasión, lanzaremos la siguiente configuración.

![Diagrama de configuración general](./assets/4-1.png)


En esta ocasión, se construirá lo siguiente utilizando Docker Compose.
※ Dado que en esta ocasión nos centramos en aprender FIWARE, se omitirá la explicación sobre [docker compose](https://docs.docker.jp/compose/toc.html).

* FIWARE Orion
* MongoDB
* FIWARE Cygnus
* PostgreSQL

Ejecute el siguiente comando.

`./fiware-part4/setup.sh `

Utilice el siguiente comando para comprobar el contenido iniciado por Docker Compose.

`cat fiware-part4/assets/docker-compose.yml`

Utilice el siguiente comando para verificar el inicio y los datos iniciales.

`curl localhost:1026/v2/entities | jq`

[Ir a al paso 2](step2.md)
