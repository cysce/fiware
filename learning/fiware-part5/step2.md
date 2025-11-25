[SIr al paso 1](step1.md)

# 2-1 Registro de Entity especificando Fiware-Service

En Orion se puede establecer el nombre del tenant mediante la cabecera HTTP "Fiware-Service".

Comprueba la Entity Room1 que vamos a registrar:

`cat fiware-part5/assets/example-ngsi-room1.json`

Registra la Entity Room1 en **tenant_a** con el siguiente comando:

`curl localhost:1026/v2/entities -s -S -H 'Fiware-Service: tenant_a' -H 'Content-Type: application/json' -X POST -d @fiware-part5/assets/example-ngsi-room1.json`

Obtén la lista de Entities con el siguiente comando:

`curl localhost:1026/v2/entities | jq`

En el tenant por defecto no se obtendrá nada.

Obtén las Entities especificando Fiware-Service con el siguiente comando:

`curl localhost:1026/v2/entities -H 'Fiware-Service: tenant_a' | jq`

Las actualizaciones o eliminaciones de Entities también se pueden realizar especificando Fiware-Service.
Además, muchas otras APIs y componentes (por ejemplo la Subscription API) también soportan Fiware-Service.


# 2-2 Sobre el scope con Fiware-ServicePath

Además de la multitenencia mediante Fiware-Service, se puede especificar un scope mediante Fiware-ServicePath.

Puede usarse para separar áreas, regiones, etc.

El scope puede representarse como una estructura de árbol, por ejemplo:

Ejemplo: /tokyo/shinjuku/office


# 2-3 Registro de Entity especificando Fiware-ServicePath

En Orion se puede establecer el scope mediante la cabecera HTTP "Fiware-ServicePath".

`curl localhost:1026/v2/entities -s -S -H 'Fiware-ServicePath: /tokyo/shinjuku/office' -H 'Content-Type: application/json' -X POST -d @fiware-part5/assets/example-ngsi-room1.json`

Obtén la lista de Entities con el siguiente comando.  
En el scope por defecto (/) se obtendrán Entities de todos los scopes:

`curl localhost:1026/v2/entities | jq`

Ejecuta un comando especificando otro scope.  
Si el scope es distinto, no verás la Entity:

`curl localhost:1026/v2/entities -H 'Fiware-ServicePath: /tokyo/shibuya/office' | jq`

Puedes obtener las Entities del scope especificado con:

`curl localhost:1026/v2/entities -H 'Fiware-ServicePath: /tokyo/shinjuku/office' | jq`

Además, Fiware-ServicePath se puede usar conjuntamente con Fiware-Service.


# 2-4 Detención y eliminación de contenedores

Detén y elimina los contenedores que iniciaste.

1. Ejecuta el siguiente comando para detener y eliminar los contenedores:

   `docker compose -f fiware-part5/assets/docker-compose.yml down`

2. Una vez completado, verifica que los contenedores se hayan detenido y eliminado con el siguiente comando:

   `docker compose -f fiware-part5/assets/docker-compose.yml ps -a`

   Si la lista no muestra nada, significa que fue exitoso.

[Finalizar](finish.md)
