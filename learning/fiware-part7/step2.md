[Ir al paso 1](step1.md)

# 2-1 Logs cuando Orion llama a la REST API que publica

Verificaremos la salida de logs durante la inyección de datos.

Ejecuta el siguiente comando para registrar datos:

```json
curl localhost:1026/v2/entities -s -S -H 'Content-Type: application/json' -X POST -d @- <<EOF
{
  "id": "Room1",
  "type": "Room",
  "temperature": {
     "value": 23.3,
     "type": "Float",
     "metadata": {}
  },
  "pressure": {
     "value": 711,
     "type": "Integer",
     "metadata": {}
  }
}
EOF
```

Ejecuta el siguiente comando para verificar los logs:

```
docker logs fiware-orion
```

![Logs durante la inyección de datos](./assets/7-4.png)

Se registra en el log que se recibió una solicitud POST a `/v2/entities`.  
También, el payload de la solicitud contiene los datos recibidos, y el código de respuesta contiene el código de estado devuelto por la API.

# 2-2 Logs cuando Orion envía notificaciones (Subscription)

Verificaremos la salida de logs durante el envío de notificaciones (Notification) a Cygnus.

Ejecuta el siguiente comando para registrar una Subscription:

```json
curl -v localhost:1026/v2/subscriptions -s -S -H 'Content-Type: application/json' -X POST -d @- <<EOF
{
  "description": "A subscription to get info about Room",
  "subject": {
    "entities": [
      {
        "idPattern": ".*",
        "type": "Room"
      }
    ],
    "condition": {
      "attrs": ["temperature"]
    }
  },
  "notification": {
    "http": {
      "url": "http://cygnus:5055/notify"
    },
    "attrs": [
      "temperature"
    ]
  }
}
EOF
```

Ejecuta el siguiente comando para cambiar el valor de temperature:

```
curl localhost:1026/v2/entities/Room1/attrs/temperature/value -s -S -H 'Content-Type: text/plain' -X PUT -d 29.5
```

Ejecuta el siguiente comando para verificar los logs:

```
docker logs fiware-orion
```

![Logs durante el envío de notificación a Cygnus](./assets/7-5.png)

Se registra en el log que se envió una solicitud POST a `cygnus:5055/notify`.  
También, el código de respuesta contiene el código de estado devuelto por el destino de la notificación.

# 2-3 Configuración de logs mediante argumentos de línea de comandos

Los parámetros que se pueden configurar mediante argumentos de línea de comandos son los siguientes:

| Argumento de línea de comandos | Descripción |
| - | - |
| **-logDir \<dir>** | Especifica el directorio de salida del archivo de log. |
| **-logAppend** | Si se especifica, se añade a un archivo de log existente en lugar de comenzar con un archivo vacío. |
| **-logLevel** | • NONE (no muestra ningún log, excepto mensajes de error fatal)<br>• FATAL (muestra solo mensajes de error fatal)<br>• ERROR (muestra mensajes de error fatal y error)<br>• WARN (muestra mensajes de error fatal, error y advertencia; configuración por defecto)<br>• INFO (muestra mensajes de error fatal, error, advertencia e información)<br>• DEBUG (muestra todos los mensajes)<br>[El nivel de log se puede cambiar en tiempo de ejecución usando la API de administración](https://fiware-orion.readthedocs.io/en/latest/admin/management_api.html). |
| **-logSummary** | Habilita el rastreo de resumen de logs. Para más detalles, consulta la [documentación de logs](https://fiware-orion.readthedocs.io/en/latest/admin/logs.html#summary-traces). |
| **-relogAlarms** | Muestra logs cada vez que se cumple la condición de activación. Para más detalles, consulta la [documentación de logs](https://fiware-orion.readthedocs.io/en/latest/admin/logs.html#alarms). |
| **-logForHumans** | Muestra logs formateados de forma legible para humanos. |

# 2-4 Detención y eliminación de contenedores

Detén y elimina los contenedores que iniciaste.

1. Ejecuta el siguiente comando para detener y eliminar los contenedores:

   `docker compose -f fiware-part7/assets/docker-compose.yml down`

2. Una vez completado, verifica que los contenedores se hayan detenido y eliminado con el siguiente comando:

   `docker compose -f fiware-part7/assets/docker-compose.yml ps -a`

   Si la lista no muestra nada, significa que fue exitoso.

[Finalizar](finish.md)