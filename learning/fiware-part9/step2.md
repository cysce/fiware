[Ir al paso 1](step1.md)

# 2-1 Comprobación de acceso

Ejecuta el siguiente comando para crear el token que se especificará en el encabezado Authorization.

※ El token que se crea es el valor de "ClientID:Client Secret" codificado en Base64

```
echo -n "tutorial-dckr-site-0000-xpresswebapp:tutorial-dckr-site-0000-clientsecret" | openssl base64 -A
```

Como vas a usar el token creado en los pasos siguientes, configura una variable de entorno con el valor mostrado por el comando anterior. Sustituye lo que hay después del = por el token obtenido:

```
TOKEN=valor_mostrado_por_echo
```

A continuación, ejecuta el siguiente comando para obtener un access_token desde Keyrock:

```
curl -iX POST \
  "http://localhost:3005/oauth2/token" \
  -H "Accept: application/json" \
  -H "Authorization: Basic ${TOKEN}" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data "username=alice-the-admin@test.com&password=test&grant_type=password"
```

![Access token](./assets/9-4.png)

Copia el access_token que aparece en la respuesta y guárdalo en una variable de entorno para usarlo en los siguientes pasos:

```
ACCESS_TOKEN=valor_del_access_token_en_la_respuesta
```

Comprueba el acceso a Orion usando el access_token:

1. Con el token correcto deberías poder obtener datos de Orion:

```
curl -X GET \
  http://localhost:1027/v2/entities \
  -H "X-Auth-Token: ${ACCESS_TOKEN}"
```

Si devuelve el arreglo JSON vacío `[]`, la comprobación fue exitosa.

2. Con un token incorrecto no se obtendrán datos; por ejemplo:

```
curl -X GET \
  http://localhost:1027/v2/entities \
  -H "X-Auth-Token: bad_token"
```

Verifica que se muestre `Invalid token: access token is invalid`.

# 2-2 Detención y eliminación de contenedores

Detén y elimina los contenedores iniciados.

1. Ejecuta el siguiente comando para detener y eliminar los contenedores:

   `docker compose -f fiware-part9/assets/docker-compose.yml down`

2. Una vez completado, verifica que los contenedores se hayan detenido y eliminado con:

   `docker compose -f fiware-part9/assets/docker-compose.yml ps -a`

Si la lista no muestra nada, la operación fue exitosa.

[Finalizar](./finish.md)