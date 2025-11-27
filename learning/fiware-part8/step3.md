[Ir al paso 2](step2.md)

# 3-1 Registro de los dispositivos IoT virtuales en IoTAgent

Si un dispositivo no está registrado en IoTAgent, IoTAgent no aceptará datos provenientes de él. Por ello, el dispositivo virtual no registrado en este momento (IoTDevice004) no mostrará cambios aunque se interactúe con él en la interfaz.

Para habilitar el uso del dispositivo virtual004, ejecuta los siguientes comandos para registrar los dispositivos en IoTAgent.  
※ Si ya has interactuado con los dispositivos en la interfaz, `door004`, `bell004`, `motion004` y `lamp004` pudieron haberse registrado automáticamente como dispositivos desconocidos. Por eso, en los comandos siguientes primero eliminamos el dispositivo y luego lo registramos de nuevo.

1. Eliminar smart door:
```
curl -iX DELETE 'http://localhost:4041/iot/devices/door004' -H 'fiware-service:  openiot' -H 'fiware-servicepath: /'
```

2. Registrar smart door:
```
curl -iX POST \
  'http://localhost:4041/iot/devices' \
  -H 'Content-Type: application/json' \
  -H 'fiware-service: openiot' \
  -H 'fiware-servicepath: /' \
  -d '{
  "devices": [
    {
      "device_id": "door004",
      "entity_name": "urn:ngsi-ld:Door:004",
      "entity_type": "Door",
      "protocol": "PDI-IoTA-UltraLight",
      "transport": "HTTP",
      "endpoint": "http://dummy-device:3001/iot/door004",
      "commands": [
        {"name": "unlock","type": "command"},
        {"name": "open","type": "command"},
        {"name": "close","type": "command"},
        {"name": "lock","type": "command"}
       ],
       "attributes": [
        {"object_id": "s", "name": "state", "type":"Text"}
       ]
    }
  ]
}'
```

3. Eliminar bell:
```
curl -iX DELETE 'http://localhost:4041/iot/devices/bell004' -H  'fiware-service:   openiot' -H 'fiware-servicepath: /'
```

4. Registrar bell:
```
curl -iX POST \
  'http://localhost:4041/iot/devices' \
  -H 'Content-Type: application/json' \
  -H 'fiware-service: openiot' \
  -H 'fiware-servicepath: /' \
  -d '{
  "devices": [
    {
      "device_id": "bell004",
      "entity_name": "urn:ngsi-ld:Bell:004",
      "entity_type": "Bell",
      "protocol": "PDI-IoTA-UltraLight",
      "transport": "HTTP",
      "endpoint": "http://dummy-device:3001/iot/bell004",
      "commands": [
        { "name": "ring", "type": "command" }
       ]
    }
  ]
}'
```

5. Eliminar motion sensor:
```
curl -iX DELETE 'http://localhost:4041/iot/devices/motion004' -H 'fiware-service:   openiot' -H 'fiware-servicepath: /'
```

6. Registrar motion sensor:
```
curl -iX POST \
  'http://localhost:4041/iot/devices' \
  -H 'Content-Type: application/json' \
  -H 'fiware-service: openiot' \
  -H 'fiware-servicepath: /' \
  -d '{
 "devices": [
   {
     "device_id":   "motion004",
     "entity_name": "urn:ngsi-ld:Motion:004",
     "entity_type": "Motion",
     "timezone":    "Europe/Berlin",
     "attributes": [
       { "object_id": "c", "name": "count", "type": "Integer" }
     ]
   }
 ]
}'
```

7. Eliminar smart lamp:
```
curl -iX DELETE 'http://localhost:4041/iot/devices/lamp004' -H 'fiware-service:   openiot' -H 'fiware-servicepath: /'
```

8. Registrar smart lamp:
```
curl -iX POST \
  'http://localhost:4041/iot/devices' \
  -H 'Content-Type: application/json' \
  -H 'fiware-service: openiot' \
  -H 'fiware-servicepath: /' \
  -d '{
  "devices": [
    {
      "device_id": "lamp004",
      "entity_name": "urn:ngsi-ld:Lamp:004",
      "entity_type": "Lamp",
      "protocol": "PDI-IoTA-UltraLight",
      "transport": "HTTP",
      "endpoint": "http://dummy-device:3001/iot/lamp004",
      "commands": [
        {"name": "on","type": "command"},
        {"name": "off","type": "command"}
       ],
       "attributes": [
        {"object_id": "s", "name": "state", "type":"Text"},
        {"object_id": "l", "name": "luminosity", "type":"Integer"}
       ]
    }
  ]
}'
```

Descripción de los principales campos en cada petición POST:

| Campo | Descripción |
| - | - |
| device_id | ID que identifica al dispositivo. |
| entity_name | ID de la Entity que se registrará en Orion. El `device_id` enviado por UL se mapeará a este `entity_name` (Entity ID). |
| entity_type | Tipo de Entity en Orion. |
| endpoint | Endpoint donde el dispositivo recibirá comandos. |
| commands | Operaciones que se pueden enviar al dispositivo. |
| attributes | Atributos que se registrarán en Orion. Los `object_id` enviados por UL (por ejemplo `s`, `l`) se mapearán a estos nombres de atributo (`state`, `luminosity`, etc.). |

# 3-2 Verificación del funcionamiento del dispositivo virtual004

Comprueba que los dispositivos registrados en 3-1 funcionan correctamente.

En la interfaz, localiza la tienda `urn:ngsi-ld:Store:004`. Desde el desplegable inferior izquierdo selecciona "Lamp On" y pulsa el botón Send.

![Lamp On](./assets/8-9.png)

Verifica que `lamp004` cambie a `On`.

![lamp001](./assets/8-10.png)

# 3-3 Contenido de la comunicación del dispositivo

Al interactuar en la interfaz, el dispositivo envía peticiones a IoTAgent. Aquí veremos el contenido manualmente enviando una petición directa a IoTAgent.

1. Envía directamente datos a IoTAgent:
```
curl -iX POST \
  'http://localhost:7896/iot/d?k=4jggokgpepnvsb2uv4s40d59ov&i=lamp004' \
  -H 'Content-Type: text/plain' \
  -d 's|off'
```

2. Obtén los datos desde Orion:
```
curl -X GET 'http://localhost:1026/v2/entities/urn:ngsi-ld:Lamp:004/attrs?attrs=state&type=Lamp' -H 'fiware-service: openiot' -H 'fiware-servicepath: /' | jq
```

![lamp004 result](./assets/8-11.png)

Gracias al registro de dispositivos en 3-1, el campo `s` se mapea a `state`. Usando Ultralight2.0, los identificadores cortos (`s`, `l`, etc.) reducen la cantidad de datos transmitidos entre el dispositivo y el IoTAgent.

※ Dado que los dispositivos virtuales envían periódicamente su estado a IoTAgent, justo después de ejecutar el comando `state` puede aparecer como `OFF`, pero se actualizará a `ON` tras un breve periodo.

# 3-4 Detención y eliminación de contenedores

Detén y elimina los contenedores iniciados.

1. Ejecuta:
```
docker compose -f fiware-part8/assets/docker-compose.yml down
```

2. Verifica que los contenedores se han detenido y eliminado:
```
docker compose -f fiware-part8/assets/docker-compose.yml ps -a
```
Si la lista no muestra nada, la operación fue exitosa.

[Finalizar](finish.md)