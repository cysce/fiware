[Volver al paso 2](step2.md)

Configuraciones diversas en Orion Subscription.

# 3-1 Especificación del objetivo de detección mediante idPattern

Puedes usar idPattern para especificar qué Entity deben detectar cambios.
idPattern utiliza expresiones regulares para dirigirse a Entity que coincidan.

![idPattern](./assets/3-6.png)

En este ejemplo, especificando `".*"` nos dirigimos a todos los ids.

En la Terminal original, ejecuta el siguiente comando:

```json
curl -v -X PATCH localhost:1026/v2/subscriptions/${SUBSCRIPTION_ID} -s -S -H 'Content-Type: application/json' -d @- <<EOF
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
      "attrs": ["pressure"]
    }
  },
  "notification": {
    "http": {
      "url": "http://accumulator:1028/accumulate"
    },
    "attrs": [
      "pressure"
    ]
  }
}
EOF
```

Prueba cambiando el valor de pressure:

`curl localhost:1026/v2/entities/Room1/attrs/pressure/value -s -S -H 'Content-Type: text/plain' -X PUT -d 730`

Abre **Terminal2** y revisa los logs.
Verás el resultado de la notificación recibida de la misma forma que antes.

Hay muchas otras condiciones y configuraciones disponibles para enviar notificaciones. Para más detalles, consulta la documentación oficial de [Subscriptions Operations](https://github.com/telefonicaid/fiware-orion/blob/master/doc/manuals/orion-api.md#subscriptions-operations).

# 3-2 Detección y eliminación de contenedores

Detén y elimina los contenedores que iniciaste.

1. Ejecuta el siguiente comando para detener y eliminar los contenedores:

   `docker compose -f fiware-part3/assets/docker-compose.yml down`

2. Una vez commpletado, verfica que los contenedores se hayan detenido y eliminado con el siguiente comando:

   `docker compose -f fiware-part3/assets/docker-compose.yml ps -a`

   Si la lista no muestra nada, significa que fue exitoso.

[Finalizar](finish.md)
