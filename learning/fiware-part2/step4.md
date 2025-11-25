[Volver al paso 3](step3.md)

# 4-1 Eliminar una Entity

1. Puedes eliminar una Entity usuando el método DELETE sobre `/v2/entities/{id}`.

   `curl localhost:1026/v2/entities/Room2 -X DELETE`

2. Verifica que la Entity haya sido eliminada.

   `curl localhost:1026/v2/entities | jq`

## 4-2 Eliminar un Attribute

1. Puedes eliminar un atributo de una Entity usando el método DELETE sobre `/v2/entities/{id}/attrs/{attrName}`.

   `curl localhost:1026/v2/entities/Room1/attrs/pressure -X DELETE`

2. Verifica que el atributo haya sido eliminado.

   `curl localhost:1026/v2/entities | jq`

# 4-3 Detener y eliminar los contenedores
Vamos a detener y eliminar los contenedores que iniciamos.

1. Detén y elimina los contenedores con el siguiente comando:

   `docker compose -f fiware-part2/assets/docker-compose.yml down`

2. Cuando termines, verifica que los contenedores se han detenido y eliminado con el siguiente comando:

   `docker compose -f fiware-part2/assets/docker-compose.yml ps -a`

   Si no aparece nada en la lista, la operación fue exitosa.

[Finalizar](finish.md)
