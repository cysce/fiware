[Ia al paso 1](step1.md)

# 2-1 Creación de espacio de trabajo

Haz click en el **menú (hamburguesa)** y selecciona **＋ Nuevo espacio de trabajo**.

![New workspace](./assets/10-4.png)

Introduce el nombre del espacio de trabajo en **Nombre** y haz clic en el **botón de configurar**.

![New workspace](./assets/10-5.png)

# 2-2 Añadir widgets

A continuación añadiremos widgets.  
Los widgets son componentes necesarios para construir la pantalla en WireCloud; puedes subir widgets descargados externamente a WireCloud para usarlos.  
Esta vez utilizaremos los siguientes widgets. Descarga todos los widgets desde los enlaces a continuación:

- [ol3-map-widget](https://github.com/Wirecloud/ol3-map-widget) ([Descargar](https://github.com/Wirecloud/ol3-map-widget/releases/download/1.2.3/CoNWeT_ol3-map_1.2.3.wgt))
- [ngsi-source-operator](https://github.com/wirecloud-fiware/ngsi-source-operator) ([Descargar](https://github.com/wirecloud-fiware/ngsi-source-operator/releases/download/4.2.0/CoNWeT_ngsi-source_4.2.0.wgt))
- [ngsi-entity2poi-operator](https://github.com/wirecloud-fiware/ngsi-entity2poi-operator) ([Descargar](https://github.com/wirecloud-fiware/ngsi-entity2poi-operator/releases/download/v3.2.2/CoNWeT_ngsientity2poi_3.2.2.wgt))


Haz clic en el **botón Mis recursos**.

![Myresource](./assets/10-6.png)

Luego haz clic en el **botón Subir**.

![Upload](./assets/10-7.png)

Puedes añadir los widgets arrastrando y soltando los archivos descargados en la pantalla. Cuando los archivos estén añadidos, haz clic en el **botón Subir**.

![Upload](./assets/10-8.png)

Haz clic en el **botón volver** en la parte superior de la pantalla para regresar al espacio de trabajo.

![Return](./assets/10-9.png)

# 2-3 Crear la pantalla con el cableado (wiring)

Haz clic en el **botón Editar** para habilitar la edición de la pantalla, incluido el wiring.

![Edit](./assets/10-10.png)

A continuación haz clic en el **botón Wiring**.

![Wiring](./assets/10-11.png)

A continuación haz clic en el **botón de búsqueda de componentes**.

![Component search](./assets/10-12.png)

Haz clic en el botón **+** del OpenLayers Map para crear una caja naranja debajo y arrástrala al área vacía a la derecha.

![OpenLayers Map](./assets/10-13.png)

Cambia a la pestaña azul y selecciona **Operador**.

![Operator](./assets/10-14.png)

Del mismo modo, haz clic en **+** para NGSI Source y NGSI Entity To PoI, y arrastra las cajas verdes creadas al área vacía a la derecha.

![NGSI Source](./assets/10-15.png)

Conecta los widgets como se muestra a continuación.  
Para conectar, arrastra el conector azul sobresaliente hasta el conector de destino.

![Widget connection](./assets/10-16.png)

# 2-4 Configuración de publicación de puertos

Vuelve una vez a la pantalla de Codespaces para realizar la configuración necesaria para la actualización en tiempo real de los datos.  
※ Esta configuración debe realizarse antes de configurar NGSI Source.

Haz clic en la **pestaña Puertos**.

![Port](./assets/10-22.png)

En la fila del **puerto 8100**, haz clic derecho, sitúa el cursor sobre **alcance de visualización del puerto** y en el submenú selecciona **public**.

![Port](./assets/10-25.png)

# 2-5 Configuración de conexión con Orion

Vuelve a la pantalla de WireCloud, haz clic en el **menú (hamburguesa)** de NGSI Source y selecciona **Configuración**.

![Setting](./assets/10-17.png)

Introduce la configuración del operador como se muestra abajo y haz clic en **Configurar**.

![Operator setting](./assets/10-18.png)

Copia y pega los siguientes valores:

NGSI server URL: `http://orion:1026/`  
NGSI proxy URL: ※1 ※2  
NGSI entity types: `NuisanceWildlife`  
Monitored NGSI Attributes: `location`

※1 Para **NGSI proxy URL** pega el valor copiado desde la pantalla de Codespaces. Coloca el cursor sobre la fila del **puerto 8100**, haz clic en el icono del recuadro rojo y copia el valor mostrado.

※2 En entorno local, para **NGSI Proxy URL** especifica el nombre de host dentro de la red Docker. Como el mismo URL será utilizado por el navegador para comunicarse con el NGSI Proxy, es necesario hacer que el nombre de host de la red Docker apunte a `127.0.0.1` (por ejemplo, editando el archivo hosts). El procedimiento varía según el sistema operativo.

![Address copy](./assets/10-26.png)

A continuación se explica cada campo marcado en rojo en la configuración del operador:
| Campo | Descripción |
| - | - |
| NGSI server URL | URL de Orion desde la que se obtendrá la información de entidades. |
| NGSI proxy URL | URL del proxy que se utilizará para recibir las notificaciones de cambio. |
| NGSI entity types | Filtra las entidades que se obtendrán de Orion: introduce los tipos de entidad separados por comas. |
| Monitored NGSI Attributes | Atributos que se supervisarán para detectar actualizaciones en Orion. Si se establece, se creará automáticamente una suscripción en Orion. Si no se establece, no se creará la suscripción. |

Haz clic en el **botón volver** en la parte superior para regresar al espacio de trabajo.

![Operator setting](./assets/10-19.png)

# 2-6 Verificación de la visualización

Los datos de ejemplo que registramos se muestran en el mapa como marcadores. Al pulsar un marcador aparecerá un globo con la información detallada.

![Check visualization](./assets/10-20.png)

Cuando se actualice la Entity en Orion, la actualización se reflejará en tiempo real.

Simulando que el oso se ha movido, actualiza el valor location de la Entity.

Ejecuta el siguiente comando para actualizar location:

```
curl localhost:1026/v2/entities/NuisanceWildlife1/attrs -s -S -H 'Content-Type: application/json' -X PATCH -d @- <<EOF
{
  "location": {
    "value": "37.99, 140.3",
    "type": "geo:point"
  }
}
EOF
```

Ejecuta el siguiente comando para comprobar que location se ha actualizado:

```
curl localhost:1026/v2/entities | jq
```

![Check realtime update](./assets/10-27.png)

Comprueba que el marcador en el mapa se ha movido.

[Ir al paso 3](step3.md)
