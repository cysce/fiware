En la Parte 9 aprenderemos sobre la protección de acceso de los componentes de FIWARE.

# 1-1 Necesidad de proteger el acceso a los componentes FIWARE

Si no se aplica una protección de acceso adecuada a los componentes FIWARE, terceros no autorizados podrían acceder maliciosamente a servidores o sistemas (acceso no autorizado), lo que puede provocar filtraciones o alteraciones de información.

Para evitarlo, se utilizan componentes como Keyrock y Wilma para añadir autenticación y autorización a las aplicaciones y backend, de modo que solo usuarios autorizados puedan acceder a Orion y a otros servicios REST.

# 1-2 Resumen de Keyrock y Wilma

![Descripción general de Keyrock](./assets/9-1.png)

![Descripción general de Wilma](./assets/9-2.png)

# 1-3 Arranque de la configuración

En esta ocasión pondremos en marcha la siguiente configuración.

![Diagrama de configuración general](./assets/9-3.png)

Ejecuta el siguiente comando:

```
docker compose -f fiware-part9/assets/docker-compose.yml up -d
```

Cuando termine, verifica que los servicios estén en ejecución:

```
docker compose -f fiware-part9/assets/docker-compose.yml ps
```

Si en la lista aparecen **fiware-orion-proxy**, **fiware-orion**, **fiware-keyrock**, **db-mongo** y **db-mysql**, la puesta en marcha fue exitosa.

# 1-4 Configuración de Keyrock y Wilma

Para simplificar los pasos, en esta parte se omite la explicación de cómo registrar usuarios y aplicaciones mediante la interfaz GUI o la API de Keyrock.

Al arrancar los contenedores con docker-compose, se registran automáticamente los siguientes datos:

Administrador
| Nombre | E-mail | Contraseña |
| - | - | - |
| alice | `alice-the-admin@test.com` | test |

Aplicación
| Clave | Valor |
| - | - |
| Client ID | tutorial-dckr-site-0000-xpresswebapp |
| Client Secret | tutorial-dckr-site-0000-clientsecret |

[Ir al paso 2](step2.md)