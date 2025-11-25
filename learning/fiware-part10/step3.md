[Ir al paso 2](step2.md)

# 3-1 Introducción de otros métodos de visualización

Como ejemplo de otros métodos de visualización, presentamos Knowage.

Knowage es un componente FIWARE para BI y análisis de datos. Realiza análisis de datos combinando datos de Orion, RDBMS y otros. Los resultados del análisis se pueden visualizar combinando componentes mediante operaciones de arrastrar y soltar.

![knowage](./assets/10-21.png)

- [Sitio oficial](https://www.knowage-suite.com/site/)
- [Documentación](https://knowage.readthedocs.io/en/latest/)

# 3-2 Detención y eliminación de contenedores

Detén y elimina los contenedores que iniciaste.

1. Ejecuta el siguiente comando para detener y eliminar los contenedores:

   `docker compose -f fiware-part10/assets/docker-compose.yml down`

2. Una vez completado, verifica que los contenedores se hayan detenido y eliminado con el siguiente comando:

   `docker compose -f fiware-part10/assets/docker-compose.yml ps -a`

Si la lista no muestra nada, significa que fue exitoso.

[Finalizar](./finish.md)
