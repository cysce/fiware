[Volver al paso 2](step2.md)

Diferentes formas de obtener Entity

# 3-1 Obtener una Entity especificando el id

Puedes obtener una Entity específica usando `/v2/entities/{id}`.

`curl localhost:1026/v2/entities/Room1 | jq`

# 3-2 Obtener una Entity especificando el type

Puedes obtener entidades de un tipo específico usando `/v2/entities`.

`curl localhost:1026/v2/entities?type=Room | jq`

# 3-3 Obtener en formato Key Value

Usando la opción keyValues puedes obtener solo los valores de los atributos.

`curl localhost:1026/v2/entities/Room1?options=keyValues | jq`

# 3-4 Obtener en formato Values

De forma aún más compacta, usando la opción values puedes obtener solo un arreglo con los valores de los atributos.

`curl localhost:1026/v2/entities/Room1?options=values | jq`

Si especificas attrs, puedes indicar qué atributos obtener y su orden.

`curl 'localhost:1026/v2/entities/Room1?options=values&attrs=temperature,pressure' | jq`

# 3-5 Obtener atributos

Puedes obtener solo los atributos usando `/v2/entities/{id}/attrs/`.

`curl localhost:1026/v2/entities/Room1/attrs/ | jq`

También puedes obtener un solo attribute usando `/v2/entities/{id}/attrs/{attrsName}`.

`curl localhost:1026/v2/entities/Room1/attrs/temperature | jq`

# 3-6 Obtener el valor de un atributo

Puedes obtener solo el valor de un atributo usando `/v2/entities/{id}/attrs/{attrsName}/value`.

`curl localhost:1026/v2/entities/Room1/attrs/temperature/value`

# 3-7 Otras formas avanzadas de obtener Entity

## 3-7-1 Obtener Entity usando el idPattern

Usando la opción idPattern puedes filtrar entidades mediante expresiones regulares.
El siguiente comando obtiene entidades cuyo id comienza con Room seguido de un número entre 1 y 2.

`curl localhost:1026/v2/entities?idPattern=^Room[1-2] -g | jq`

## 3-7-2 Obtener entidades usando lenguaje de consulta

Con la opción q puedes filtrar entidades usando un lenguaje de consulta.
El siguiente comando obtiene entidades cuyo valor de temperature es mayor a 22.

`curl 'localhost:1026/v2/entities?q=temperature>22' | jq`

En la documentación de [Simple Query Language](https://github.com/telefonicaid/fiware-orion/blob/master/doc/manuals/orion-api.md#simple-query-language) puedes encontrar más ejemplos de uso.

[Ir al paso 4](step4.md)
