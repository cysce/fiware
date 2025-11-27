En la Parte 7 aprenderemos sobre la configuración de registros (logs) de FIWARE Orion.

# Resumen de Orion

![Descripción general de Orion](./assets/7-1.png)

# 1-1 Inicio de la configuración

En esta ocasión pondremos en marcha la siguiente configuración.

![Diagrama general](./assets/7-2.png)

Ejecuta el siguiente comando:

```
docker compose -f fiware-part7/assets/docker-compose.yml up -d
```

Cuando termine el proceso en la terminal, verifica que los servicios estén en ejecución con:

```
docker compose -f fiware-part7/assets/docker-compose.yml ps
```

Si en la lista aparecen **fiware-orion**, **db-mongo**, **fiware-cygnus**, **db-postgres**, la puesta en marcha fue exitosa.

# 1-2 Destino de los registros (logs)

La ubicación por defecto del archivo de registro es `/tmp/contextBroker.log`.

El directorio donde se guarda el archivo de log (por defecto `/tmp`) puede cambiarse usando el argumento de línea de comandos `-logDir`.

Sin embargo, si inicias Orion usando Docker (imagen fiware/orion), siguiendo las mejores prácticas de Docker los logs se envían a la salida estándar (stdout) y no se escriben en archivos dentro del contenedor.

# 1-3 Descripción general de los registros

El formato de los logs generados por FIWARE Orion está diseñado para su uso con herramientas como Splunk o Fluentd.

Cada línea del log está formada por varios campos clave-valor separados por la barra vertical (`|`).

![Logs](./assets/7-3.png)

Los campos de cada línea son los siguientes.

| Campo | Descripción |
| - | - |
| time | Marca temporal del momento en que se generó la línea del log (formato ISO8601). |
| lvl | Nivel del log: FATAL, ERROR, WARN, INFO, DEBUG, SUMMARY. SUMMARY es un nivel especial que solo aparece si se activa la opción `-logSummary`. |
| corr (correlator id) | ID asignado de forma única a cada línea de log. |
| trans (transaction id) | ID asignado de forma única a la transacción. Existen dos tipos de transacciones: la iniciada cuando un cliente externo llama a la API REST publicada por Orion y la iniciada cuando Orion envía una notificación (Subscription). |
| from | IP de origen de la solicitud HTTP asociada a la transacción. Si la solicitud incluye los encabezados `X-Forwarded-For` o `X-Real-IP`, esos valores se priorizan y se muestran aquí. |
| srv | Valor del encabezado `fiware-service`. |
| subsrv | Valor del encabezado `fiware-servicepath`. |
| comp (component) | En la versión actual de Orion este campo siempre contiene "orion". |
| op | Función del código fuente que generó el mensaje de log. |
| msg (message) | Mensaje de log real. |

[Ir al paso 2](step2.md)