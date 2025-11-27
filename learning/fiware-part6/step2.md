[Ir al paso 1](step1.md)

# 2-1 Comprobación antes de configurar la Registration

Ejecuta el siguiente comando contra OrionA y verifica que no se obtienen datos:

```
curl localhost:1026/v2/entities/Room1/attrs/pressure?type=Room -s -S -H 'Accept: application/json' | jq
```

# 2-2 Configuración de la Registration

Configura la Registration en OrionA con el siguiente comando:

```json
curl localhost:1026/v2/registrations -s -S -H 'Content-Type: application/json' -H 'Accept: application/json' -X POST -d @- <<EOF
{
  "description": "A registration to get info about pressure of Room1 from OrionB",
  "dataProvided": {
    "entities": [
      {
        "id": "Room1",
        "type": "Room"
      }
    ],
    "attrs": [
      "pressure"
    ]
  },
  "provider": {
    "http": {
      "url": "http://orionB:1027/v2"
    }
  }
}
EOF
```

# 2-3 Reenvío de OrionA a OrionB

Tras completar la configuración de la Registration, vuelve a ejecutar el siguiente comando contra OrionA:

```
curl localhost:1026/v2/entities/Room1/attrs/pressure?type=Room -s -S -H 'Accept: application/json' | jq
```

Verifica que la consulta se reenvía a OrionB y que se obtienen los datos.

![ResponseBody](./assets/6-4.png)

# 2-4 Eliminación de la Registration

Orion no implementa `PATCH /v2/registration/<id>`, por lo que no es posible actualizar una Registration directamente. Para modificarla hay que eliminarla y volver a crearla.

Pasos para eliminar una Registration:

1. Obtén el id de la Registration ejecutando:
```
curl localhost:1026/v2/registrations | jq
```

2. Asigna el id a una variable de entorno. Copia el id obtenido y pégalo después del = en el siguiente comando:
```
REGISTRATION_ID=id_obtenido
```

Con ese id puedes referirte a la Registration en `/v2/registrations/{id}` y eliminarla con DELETE.

3. Elimina la Registration:
```
curl localhost:1026/v2/registrations/${REGISTRATION_ID} -X DELETE
```

[Ir al paso 3](step3.md)