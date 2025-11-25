[Volver al paso 1](step1.md)

Accederemos a FIWARE Orion.
Orion se ejecuta por defecto en el puerto `1026`. En este caso, hemos realizado el reenvío de puertos para poder acceder también desde fuera del contenedor usando el mismo puerto `1026`.

![Diagrama de la arquitectura](./assets/2-1.png)

Para comprobar que todo funciona correctamente, vamos a acceder a `/version` y `/v2/entities`.

# 2-1 Obtener la información de versión de Orion

`curl http://localhost:1026/version`

Si obtienes la información de la versión Orion en formato JSON, la operación fue exitosa.

```shell
StatusCode        : 200
StatusDescription : OK
Content           : {
                    "orion" : {
                      "version" : "4.3.0",
                      "uptime" : "0 d, 0 h, 0 m, 14 s",
                      "git_hash" : "35fce6d1cf8d1078ce27e497968ebe10785cd44d",
                      "compile_time" : "Wed Oct 8 11:42:14 UTC 2025",
                      "compiled_by" : ...
RawContent        : HTTP/1.1 200 OK
                    Fiware-Correlator: f8a13bbe-a781-11f0-8924-0ee576b2e9c9
                    Content-Length: 374
                    Content-Type: application/json
                    Date: Sun, 12 Oct 2025 15:41:51 GMT

                    {
                    "orion" : {
                      "version" : "4.3.0...
Forms             : {}
Headers           : {[Fiware-Correlator, f8a13bbe-a781-11f0-8924-0ee576b2e9c9], [Content-Length, 374], [Content-Type,
                    application/json], [Date, Sun, 12 Oct 2025 15:41:51 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 374
```

# 2-2 Obtener la lista de Entidades de Orion

`curl http://localhost:1026/v2/entities`

Como aún no hay datos, si recibes en arreglo JSON vacío `[]`, la operación fue exitosa.

```shell
StatusCode        : 200
StatusDescription : OK
Content           : []
RawContent        : HTTP/1.1 200 OK
                    Fiware-Correlator: 4ded1732-a782-11f0-a8e6-0ee576b2e9c9
                    Content-Length: 2
                    Content-Type: application/json
                    Date: Sun, 12 Oct 2025 15:44:14 GMT

                    []
Forms             : {}
Headers           : {[Fiware-Correlator, 4ded1732-a782-11f0-a8e6-0ee576b2e9c9], [Content-Length, 2], [Content-Type,
                    application/json], [Date, Sun, 12 Oct 2025 15:44:14 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 2
```

# 2-3 Detener y eliminar los contenedores
Vamos a detener y eliminar los contenedores que iniciamos.

1. Detén y elimina los contenedores con los siguientes comandos:

   `docker stop $(docker ps -q)`

   `docker rm $(docker ps -q -a)`

2. Cuando finalice, verifica que los contenedores se han detenido y eliminado con el siguiente comando:

   `docker ps -a`

   Si no aparece nada en la lista, la operación fue exitosa

[Finalizar](finish.md)
