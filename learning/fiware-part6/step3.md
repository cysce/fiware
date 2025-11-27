[Ir al paso 2](step2.md)

Aprenderemos sobre varias configuraciones en Registration de Orion.

# 3-1 Especificación del objetivo de reenvío mediante idPattern

Puedes usar idPattern para especificar qué entidades serán objeto del reenvío.  
idPattern emplea expresiones regulares para apuntar a las entidades que coincidan.

![idPattern](./assets/6-5.png)

En este ejemplo, al especificar `".*"` se seleccionan todos los ids.

# 3-2 Comprobación antes de configurar la Registration

Ejecuta el siguiente comando contra OrionA y comprueba que no se obtienen datos:

```
curl localhost:1026/v2/entities/Room1/attrs/pressure?type=Room -s -S -H 'Accept: application/json' | jq
```

# 3-3 Configuración de la Registration

Configura la Registration en OrionA con el siguiente comando:

```json
curl localhost:1026/v2/registrations -s -S -H 'Content-Type: application/json' -H 'Accept: application/json' -X POST -d @- <<EOF
{
  "description": "A registration to get info about pressure of all entities from OrionB",
  "dataProvided": {
    "entities": [
      {
        "idPattern": ".*",
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

# 3-4 Reenvío de OrionA a OrionB

Tras completar la configuración de la Registration, vuelve a ejecutar el siguiente comando contra OrionA:

```
curl localhost:1026/v2/entities/Room1/attrs/pressure?type=Room -s -S -H 'Accept: application/json' | jq
```

Verifica que la consulta se reenvía a OrionB y que se obtienen los datos.

![ResponseBody](./assets/6-4.png)

La información sobre Registrations está documentada en la sección oficial de [Registration Operations](https://github.com/telefonicaid/fiware-orion/blob/master/doc/manuals/orion-api.md#registration-operations).

# 3-5 Detención y eliminación de contenedores

Detén y elimina los contenedores que iniciaste.

1. Ejecuta el siguiente comando para detener y eliminar los contenedores:

   `docker compose -f fiware-part6/assets/docker-compose.yml down`

2. Una vez completado, verifica que los contenedores se hayan detenido y eliminado con el siguiente comando:

   `docker compose -f fiware-part6/assets/docker-compose.yml ps -a`

   Si la lista no muestra nada, significa que fue exitoso.

[Finalizar](finish.md)