Parte 3 Función de Suscripción de FIWARE Orion.

# Orion

![Orion](./assets/3-0.png)

# 1-1 Inicio de la configuración

En esta ocasión vamos a iniciar la siguiente configuración.
※ Para más detalle sobre el inicio, consulta la sección [Parte 1](../fiware-part1/step1.md) o la sección [Parte 2](../fiware-part2/step1.md).

![Diagrama general](./assets/3-8.png)

Ejecuta el siguiente comando:

`./fiware-part3/setup.sh `

Cuando termine el proceso en la terminal, verifica el funcionamiento con el siguiente comando:

`curl localhost:1026/v2/entities | jq`

# 1-2 Función de Suscripción (Subscription) de FIWARE Orion

FIWARE Orion tiene una función que permite detectar cambios en los datos y notificar a un sistema específico.
Puedes configurar las notificaciones haciendo POST a `/v2/subscriptions/`.

El body del POST es el siguiente:

![SubscriptionBody](./assets/3-3.png)

En este ejemplo, cuando se actualiza el valor **pressure** de **Room1**, se enviará el atributo **pressure** mediante un POST HTTP a la URL de notificación indicada.

[Ir al paso 2](step2.md)