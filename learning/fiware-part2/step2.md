[Volver al paso 1](step1.md)

Formato de los datos para operar con los datos en Orion

# 2-1 Sobre NGSI v2

FIWARE Orion gestiona los datos en un formato llamado NGSI v2.  

El modelo de datos de NGSIv2 es el siguiente:

![NGSIv2](./assets/2-0.png)

En NGSI, los datos se representan en formato JSON.
Veamos un ejemplo de datos.

`cat fiware-part2/assets/example-ngsi-room1.json`

Basándonos en el diagrama del modelo de datos, a continuación se muestra como ejemplo una entidad (Entity) de una habitación (Room) que contiene información de temperatura (temperature) y presión (pressure).

![NGSIv2](./assets/2-1.png)

En FIWARE Orion, un conjunto de información de contexto (por ejemplo, temperatura, presión, etc.) se denomina Entity.

![Entity](./assets/2-2.png)

# 2-2 Registro de una Entity

Vamos a registrar la Entity anterior en Orion usando el siguiente comando.
Para registrar una Entity, realiza un POST al endpoint `/v2/entities`. mediante HTTP.

1. Registro Room1

   `curl localhost:1026/v2/entities -s -S -H 'Content-Type: application/json' -X POST -d @fiware-part2/assets/example-ngsi-room1.json`

2. Verifica la Entity que acabas de registrar

   `curl localhost:1026/v2/entities | jq`


# 2-3 Actualización de atributos de una Entity

Sino necesita cambiar el id o el type de una Entity, solo puede actualizar los atributos.

Realizando un POST a `/v2/entities/{id}/attrs` puede actualizar múltiples atributos a la vez.

1. Actualiza los atributos de Room1.
   ```json
   curl localhost:1026/v2/entities/Room1/attrs -s -S -H 'Content-Type: application/json' -X PATCH -d @- <<EOF
   {
     "temperature": {
       "value": 26.5,
       "type": "Float"
     },
     "pressure": {
       "value": 763,
       "type": "Integer"
     }
   }
   EOF
   ```

2. Verifica que la Entity se haya actualizado

   `curl localhost:1026/v2/entities | jq`

# 2-4 Actualización solo del valor de un attribute

Realizando un PUT a `/v2/entities/{id}/attrs/{attrName}/value` se puede actualizar solo el valor de un attribute. Tener en cuenta que el Content-type debe ser text/plain

1. Actualiza el valor de Room1.

   `curl localhost:1026/v2/entities/Room1/attrs/temperature/value -s -S -H 'Content-Type: text/plain' -X PUT -d 28.5`

2. Verifica que la Entity se haya actualizado.

   `curl http://localhost:1026/v2/entities | jq`


# 2-5 Agregar una nueva Entity

Si hace un POST a `/v2/entities` con un id diferente al de las Entity ya registradas, se registrará como una nueva Entity.

Verifica la Entity de Room2 que vas a añadir con el siguiente comando:

`cat fiware-part2/assets/example-ngsi-room2.json`

```json
{
  "id": "Room2",
  "type": "Room",
  "temperature": {
     "value": 19.9,
     "type": "Float",
     "metadata": {}
  },
  "pressure": {
     "value": 719,
     "type": "Integer",
     "metadata": {}
  }
}
```

1. Registra Room2.

   `curl localhost:1026/v2/entities -s -S -H 'Content-Type: application/json' -X POST -d @fiware-part2/assets/example-ngsi-room2.json`

2. Verifica que la Entity Room2 Entity se haya registrado.

   `curl localhost:1026/v2/entities | jq`

   Si aparecen las Entity Room1 y Room2, la operación fue exitosa.


[Ir al paso 3](step3.md)