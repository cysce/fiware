[Ir al paso 1](step1.md)

# 2-1 Notificaciones a Cygnus a través de la suscripción

El método para crear datos históricos en Cygnus se lleva a cabo mediante notificaciones a través de una Suscripción desde Orion.

Configure los siguientes ajustes de suscripción para enviar notificaciones al punto final `/notify` de Cygnus.  
`/notify` es el punto final que recibe las notificaciones de suscripción de Orion y crea una vista del historial.

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

Suponiendo que la temperatura cambió gradualmente, modificaremos el valor de la temperatura.

`curl localhost:1026/v2/entities/Room1/attrs/temperature/value -s -S -H 'Content-Type: text/plain' -X PUT -d 29.5`

`curl localhost:1026/v2/entities/Room1/attrs/temperature/value -s -S -H 'Content-Type: text/plain' -X PUT -d 30.0`

`curl localhost:1026/v2/entities/Room1/attrs/temperature/value -s -S -H 'Content-Type: text/plain' -X PUT -d 30.5`

`curl localhost:1026/v2/entities/Room1/attrs/temperature/value -s -S -H 'Content-Type: text/plain' -X PUT -d 30.2`

# 2-2 Revisión de datos históricos

Inicie el cliente PostgreSQL y conéctese a la base de datos.

`docker run -it --rm  --network assets_default jbergknoff/postgresql-client postgresql://postgres:password@postgres-db:5432/postgres`


Room1の履歴を確認してみます。
Comprobaré el historial del Room1.

`SELECT * FROM default_service.room1_room limit 10;`

Después de comprobar el historial en Room1, introduzca «exit» en el terminal para cerrar el cliente PostgreSQL.

# 2-3 Detener y eliminar contenedores

Detenga y elimine los contenedores en ejecución.

1. Detenga y elimine los contenedores con el siguiente comando:

   `docker compose -f fiware-part4/assets/docker-compose.yml down`

2. Una vez completado, compruebe que los contenedores se han detenido y eliminado con el siguiente comando:

   `docker compose -f fiware-part4/assets/docker-compose.yml ps -a`

   Si no aparece nada en la lista, la operación se ha realizado correctamente.

[Finalizar](finish.md)