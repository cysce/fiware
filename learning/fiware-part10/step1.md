En la parte 10 aprenderemos sobre la visualización de datos utilizando FIWARE Wirecloud.

# 1-1 Funcionalidades de FIWARE Wirecloud

![Funcionalidades de FIWARE Wirecloud](./assets/10-1.png)

# 1-2 Inicio de la configuración

En esta ocasión vamos a iniciar la siguiente configuración.

![Diagrama general](./assets/10-2.png)

Ejecuta el siguiente comando:

```
docker compose -f fiware-part10/assets/docker-compose.yml up -d
```

Cuando termine el proceso en la terminal, verifica el funcionamiento con el siguiente comando:

```
docker compose -f fiware-part10/assets/docker-compose.yml ps
```

Si en la lista aparecen **fiware-orion**, **postgres-db**, **mongo-db**, **fiware-wirecloud**, **ngsi-proxy**, **elasticsearch**, **memcached**, **nginx**, significa que fue exitoso.

# 1-3 Inyección de datos en FIWARE Orion

Ejecuta el siguiente comando para registrar datos en Orion:

```
curl -X POST localhost:1026/v2/entities -s -S -H 'Content-Type: application/json' -d @- <<EOF
{
  "id": "NuisanceWildlife1",
  "type": "NuisanceWildlife",
  "animalCategory": {
    "value": "beasts"
  },
  "animalName": {
    "value": "bear"
  },
  "location": {
    "value": "37.69, 140.9",
    "type": "geo:point"
  },
  "time": {
    "value": "2021-03-09T08:32:00+09:00"
  }
}
EOF
```

La Entity que registramos esta vez contiene información sobre daños causados por fauna silvestre (NuisanceWildlife), con atributos como el nombre del animal (animalName), la ubicación donde fue avistado (location), etc.

Ejecuta el siguiente comando para verificar los datos registrados:

```
curl localhost:1026/v2/entities | jq
```

# 1-4 Configuración de WireCloud

Ejecuta el siguiente comando para crear un superusuario en WireCloud:

```
docker compose -f fiware-part10/assets/docker-compose.yml exec wirecloud python manage.py createsuperuser
```

Ingresa la información del superusuario de forma interactiva:

```
Username (leave blank to use 'root'): nombre_usuario
Email address: correo_electrónico
Password: contraseña
Password (again): confirmar_contraseña
Superuser created successfully.
```

Utilizaremos WireCloud para construir una pantalla web de visualización. Accede a la pantalla de inicio de sesión e inicia sesión con el superusuario que acabas de crear (utiliza el nombre de usuario y contraseña que estableciste).

Sigue los siguientes pasos para acceder a la pantalla de inicio de sesión:

1. Haz click en la **pestaña de puertos**.

![Port](./assets/10-22.png)

2. Coloca el cursos sobre la fila del **puerto 80** y haz click en el icono del recuadro rojo que aparece.

![Browser](./assets/10-23.png)

3. Haz click en **Iniciar sesión** en la esquina superior derecha de la pantalla.

![WireCloud top](./assets/10-24.png)

4. Se mostrará la pantalla de inicio de sesión.

![WireCloud login](./assets/10-3.png)

[Ir al paso 2](step2.md)
