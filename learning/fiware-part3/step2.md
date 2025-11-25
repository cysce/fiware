[Ir al paso 1](step1.md)

# 2-1 Iniciar la aplicación de ejemplo para la notificación

Antes de configurar la Subscription, inicia la [aplicación de ejemplo que FIWARE publica](https://github.com/telefonicaid/fiware-orion/blob/master/scripts/accumulator-server.py) como destino de las notificaciones.

![Accumulate](./assets/3-4.png)

Para ejecutar la aplicación de ejemplo, abre una nueva Terminal.

![OpenTerminal](./assets/3-9.png)

En **Terminal2** ejecuta los siguientes comandos para iniciar la aplicación de ejemplo de FIWARE.

`docker exec -it accumulator bash`

`./accumulator-server.py --port 1028 --url /accumulate --pretty-print -v`

Este aplicación muestra en log la información que llega por HTTP. La usaremos para verificar el contenido de las notificaciones enviadas por Orion.


# 2-2 Configuración de la Subscription

Vuelve a la Terminal original.
Ejecuta el siguiente comando para crear la Subscription.

```json
curl -v localhost:1026/v2/subscriptions -s -S -H 'Content-Type: application/json' -X POST -d @- <<EOF
{
  "description": "A subscription to get info about Room1",
  "subject": {
    "entities": [
      {
        "id": "Room1",
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

Cambia el valor de pressure:

`curl localhost:1026/v2/entities/Room1/attrs/pressure/value -s -S -H 'Content-Type: text/plain' -X PUT -d 720`

Abre **Terminal2** y revisa los logs. Verás la notificación recibida, similar a la imagen a continuación.

![Result](./assets/3-5.png)

# 2-3 Comprobación de la Subscirption

Se puede obtener la lista de subscription con un GET `/v2/subscriptions`

`curl localhost:1026/v2/subscriptions | jq`

Verás que la subscription creada tiene un id asociado.

![subscriptionId](./assets/3-7.png)

En el siguiente paso actualizarás la subscription, así que guarda el id en una variable de entorno. Sustituye lo que hay después del = por el id obtenido:

```
SUBSCRIPTION_ID=obtén_el_id_de_la_subscription
```

Con ese id puedes usar `/v2/subscriptions/{id}` para actualizar con PATCH o eliminar con DELETE.

[Ir al paso 3](step3.md)