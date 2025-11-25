Parte 2 - Operaciones básicas de manipulación de datos en FIWARE Orion.

# Descripción general de Orion

![Orion](./assets/1-2.png)

# 1-1 Inicio de la configuración

Primero, vamos a crear la siguiente configuración.

![Diagrama general](./assets/1-1.png)


Utilizaremos docker compose para iniciar FIWARE Orion y MongoDB al mismo tiempo.


Ejecuta el siguiente comando:

```shell
docker compose -f fiware-part2/assets/docker-compose.yml up -d
```

Cuando finalice el proceso en la terminal, verifica que ambos servicios estén en funcionamiento con el siguiente comando:

```shell
docker compose -f fiware-part2/assets/docker-compose.yml ps
```

Si en la lista aparecen **fiware-orion** y **db-mongo**, la operación fue exitosa

# 1-2 Verificación del funcionamiento de FIWARE Orion

Para comprobar que FIWARE Orion está funcionando correctamente, ejecuta el siguiente comando:

`curl http://localhost:1026/v2/entities`

Como aún no hay datos, si reciben un arreglo JSON vacio `[]`, la operación fue exitosa.

```shell
StatusCode        : 200
StatusDescription : OK
Content           : []
RawContent        : HTTP/1.1 200 OK
                    Fiware-Correlator: bb62a60e-a789-11f0-9067-9ec881258cab
                    Content-Length: 2
                    Content-Type: application/json
                    Date: Sun, 12 Oct 2025 16:37:24 GMT

                    []
Forms             : {}
Headers           : {[Fiware-Correlator, bb62a60e-a789-11f0-9067-9ec881258cab], [Content-Length, 2], [Content-Type,
                    application/json], [Date, Sun, 12 Oct 2025 16:37:24 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 2
````

[Ir al paso 2](step2.md)