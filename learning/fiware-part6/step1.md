En la Parte 6 aprenderemos sobre la funcionalidad Registration de FIWARE Orion.

# Resumen de Orion

![Descripción general de Orion](./assets/6-1.png)

# 1-1 Inicio de la configuración

En esta ocasión pondremos en marcha la siguiente configuración.

![Diagrama general](./assets/6-2.png)

Ejecuta el siguiente comando:

```
docker compose -f fiware-part6/assets/docker-compose.yml up -d
```

Cuando termine, verifica que los servicios estén en ejecución:

```
docker compose -f fiware-part6/assets/docker-compose.yml ps
```

Si en la lista aparecen **fiware-orionA**, **fiware-orionB**, **mongo-dbA**, **mongo-dbB**, la puesta en marcha fue exitosa.

Registra datos siguiendo estos pasos:

1. Registra datos en FIWARE OrionA:

```
curl localhost:1026/v2/entities -s -S -H 'Content-Type: application/json' -X POST -d @fiware-part6/assets/example-ngsi-room-orionA.json
```

2. Verifica la Entity registrada en OrionA:

```
curl localhost:1026/v2/entities | jq
```

3. Registra datos en FIWARE OrionB:

```
curl localhost:1027/v2/entities -s -S -H 'Content-Type: application/json' -X POST -d @fiware-part6/assets/example-ngsi-room-orionB.json
```

4. Verifica la Entity registrada en OrionB:

```
curl localhost:1027/v2/entities | jq
```

# 1-2 Flujo de comunicación de Registration

Al configurar una Registration en OrionA y ejecutar una consulta hacia la Entity sujeta a la Registration, la consulta se reenviará a OrionB. El flujo es el siguiente:

![Flujo de comunicación](./assets/6-6.png)

`/v2/registration`  
Se configura la Registration en OrionA.

`/v2/entities`  
Se ejecuta la consulta en OrionA.

`/v2/op/query`  
OrionA reenvía la consulta a OrionB.

# 1-3 Función Registration de FIWARE Orion

FIWARE Orion dispone de una funcionalidad para reenviar consultas y solicitudes de actualización. Al configurar el reenvío, Orion puede obtener datos de contexto que son proporcionados por otro sistema compatible con NGSI.

Motivos habituales para usar el reenvío:
- Permite obtener siempre los datos más recientes desde un servicio externo que mantiene la información.
- La estructura y gestión de los datos puede delegarse al sistema destino, reduciendo el coste de gestión de los datos en el Orion origen.

Se puede configurar el reenvío realizando un POST a `/v2/registrations`.

El body del POST tiene la siguiente estructura:

![RegistrationBody](./assets/6-3.png)

En este ejemplo, cuando OrionA reciba una consulta o solicitud de actualización sobre el atributo **pressure** de **Room1**, reenviará la consulta/solicitud a **OrionB**.

[Ir al paso 2](step2.md)