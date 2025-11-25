En la parte 5, aprenderemos sobre Fiware-Service y Fiware-ServicePath.

# 1-1 Inicio de la configuración

En esta ocasión, lanzaremos la siguiente configuración.

![Diagrama de configuración general](./assets/1-1.png)


En esta ocasión, se creará lo siguiente utilizando Docker Compose.
※ Dado que el objetivo principal en esta ocasión es aprender FIWARE, se omitirá la explicación de [Docker Compose](https://docs.docker.jp/compose/toc.html).

* FIWARE Orion
* MongoDB

Ejecute el siguiente comando.

`./fiware-part5/setup.sh `

# 1-2 Multitenencia a través de los servicios FIWARE

FIWARE cuenta con capacidades multitenencia que separan lógicamente las bases de datos.  
Esto facilita la implementación de políticas de autorización a través de componentes como el Marco de Seguridad FIWARE (proxy PEP, IDM, Control de Acceso).

![Multitenencia](./assets/1-2.png)
※ Aunque se omite en el diagrama, Fiware-Service y Entity se gestionan dentro de MongoDB.

Si los inquilinos están separados, se pueden registrar EntityID duplicados, como Room1 y Room2, y no se pueden recuperar de otros inquilinos.

[Ir al paso 2](step2.md)
