En la Parte 8 aprenderemos sobre la recopilación de datos desde dispositivos utilizando IoTAgent.

# 1-1 Resumen de IoTAgent

![Resumen de IoTAgent](./assets/8-1.png)

# 1-2 Arranque de la configuración

En esta ocasión pondremos en marcha la siguiente configuración.

![Diagrama general](./assets/8-2.png)

Ejecuta el siguiente comando:

```
docker compose -f fiware-part8/assets/docker-compose.yml up -d
```

Cuando termine el proceso en la terminal, verifica que los servicios estén en ejecución con:

```
docker compose -f fiware-part8/assets/docker-compose.yml ps
```

Si en la lista aparecen **fiware-orion**, **db-mongo**, **fiware-iot-agent** y **dummy-device**, la puesta en marcha fue exitosa.

# 1-3 Funcionalidad de FIWARE IoTAgent

IoTAgent convierte los protocolos específicos de los dispositivos IoT al [NGSIv2](../fiware-part2/step2.md) (modelo de intercambio de datos estándar de FIWARE).

Es necesario cuando se quiere conectar dispositivos IoT a FIWARE.  
※ Si el dispositivo IoT soporta la API NGSI de forma nativa, no se requiere IoTAgent.

### Ejemplos de protocolos específicos de dispositivos IoT

| Protocolo | Descripción |
| - | - |
| MQTT | Protocolo basado en mensajería Publish/Subscribe que permite comunicación asíncrona uno-a-muchos. Es sencillo y ligero, adecuado para IoT. |
| Ultralight 2.0 | Protocolo ligero basado en texto diseñado para comunicarse con dispositivos con ancho de banda o memoria limitados. |

![Ejemplos de protocolos](./assets/8-3.png)

[Ir al paso 2](step2.md)