[Ir al paso 1](step1.md)

# 2-1 Configuración de IoTAgent

Ejecuta el siguiente comando para comprobar que IoTAgent esté funcionando correctamente:

```
curl -X GET 'http://localhost:4041/iot/about' | jq
```

![iot about](./assets/8-4.png)

IoTAgent actúa como middleware entre los dispositivos IoT y Orion, registrando en Orion la información de medidas enviada por los dispositivos. En este proceso, IoTAgent realiza la conversión del `device id` al `Entity ID`.

Si no se puede garantizar la unicidad del `device id` entre dispositivos, se pueden añadir las cabeceras `fiware-service` y `fiware-servicepath` para identificar mejor los dispositivos (por ejemplo, `fiware-service` para tenant o departamento, y `fiware-servicepath` para área o región).

Para más detalles sobre `fiware-service` y `fiware-servicepath` consulta la [Parte 5](../fiware-part5/step1.md).

Un ejemplo donde no se puede garantizar la unicidad del `device id` es cuando se usan dispositivos de distintos fabricantes: aunque cada fabricante pueda garantizar IDs únicos dentro de su rango, entre fabricantes podría haber colisiones.

Ejecuta el siguiente comando para configurar IoTAgent y permitir que acepte datos de los dispositivos:

```
curl -iX POST \
  'http://localhost:4041/iot/services' \
  -H 'Content-Type: application/json' \
  -H 'fiware-service: openiot' \
  -H 'fiware-servicepath: /' \
  -d '{
 "services": [
   {
     "apikey":      "4jggokgpepnvsb2uv4s40d59ov",
     "cbroker":     "http://orion:1026",
     "entity_type": "Thing",
     "resource":    "/iot/d"
   }
 ]
}'
```

# 2-2 Descripción de los dispositivos IoT virtuales

En esta parte usamos los siguientes dispositivos IoT virtuales:

| Nombre del dispositivo | Descripción |
| - | - |
| Smart Door (puerta inteligente) | Puerta electrónica que puede bloquearse/desbloquearse remotamente mediante comandos. |
| Bell (timbre) | Responde a comandos haciendo sonar el timbre. |
| Motion sensor (sensor de movimiento) | No responde a comandos, pero mide el número de clientes que pasan por la puerta. Si la puerta está desbloqueada, detecta movimiento y envía medidas al IoTAgent. |
| Smart Lamp (lámpara inteligente) | Se puede encender/apagar remotamente y registra la luminosidad. Contiene un sensor de movimiento interno que hace que la lámpara se oscurezca lentamente si no se detecta movimiento durante cierto tiempo. |

A continuación ejecuta el script para registrar en MongoDB, vía IoTAgent, los datos de los dispositivos anteriores:

```
./fiware-part8/setup.sh
```

### Cómo usar la interfaz GUI del dispositivo virtual

Accede a la pantalla de control de los dispositivos virtuales siguiendo estos pasos:

1. Haz clic en la pestaña "Ports" (Puertos).

![Port](./assets/8-12.png)

2. En la fila del puerto 3000, coloca el cursor y haz clic en el icono que aparece (marcado en rojo).

![Port](./assets/8-13.png)

3. Haz clic en "Device Monitor" en la pantalla.

![Port](./assets/8-14.png)

4. Se abrirá la pantalla de monitorización de dispositivos virtuales.

![Device Monitor](./assets/8-5.png)

Al abrir la pantalla verás los dispositivos por tienda. Selecciona la acción para un dispositivo desde el desplegable de la tienda y pulsa el botón "Send".

Nota: el dispositivo virtual 004 no está registrado en IoTAgent, por lo que no funcionará por ahora.

# 2-3 Actualización de datos usando los dispositivos virtuales

Usa los dispositivos virtuales 001–003 para actualizar datos. Como ejemplo, trabajaremos con la lámpara inteligente (lamp001).

En la interfaz, localiza la tienda con `urn:ngsi-ld:Store:001`. Desde el desplegable inferior izquierdo selecciona "Lamp On" y pulsa "Send" para encender la lámpara remotamente.

![Lamp On](./assets/8-6.png)

### Verificación del resultado de la actualización

Comprueba que `lamp001` está en estado `On`.

![lamp001](./assets/8-7.png)

Verifica que los datos medidos por el dispositivo se reflejan en Orion vía IoTAgent ejecutando:

```
curl -X GET 'http://localhost:1026/v2/entities/urn:ngsi-ld:Lamp:001/attrs?attrs=state&type=Lamp' -H 'fiware-service: openiot' -H 'fiware-servicepath: /' | jq
```

![lamp001 result](./assets/8-8.png)

En la salida verás que el `value` de `state` se ha actualizado a `ON`.

[Ir al paso 3](step3.md)