# Ejercicio 2: El Sistema Gestor en un Contenedor: Docker

## 1. Contenedores frente a Máquinas Virtuales
Un contenedor es una unidad estándar de software que empaqueta el código y todas sus dependencias para que una aplicación se ejecute de manera rápida y confiable en cualquier entorno informático. A diferencia de una máquina virtual (VM), los contenedores no virtualizan hardware completo ni ejecutan un sistema operativo invitado (Guest OS) independiente.

- **Tiempo de arranque:** Los contenedores inician en cuestión de milisegundos o pocos segundos, ya que solo levantan el proceso de la aplicación aprovechando el kernel del anfitrión. Las máquinas virtuales requieren varios minutos debido al proceso completo de inicialización del BIOS y arranque del sistema operativo invitado.
- **Tamaño y consumo de recursos:** Las imágenes de contenedores son ligeras (desde decenas de megabytes hasta pocos gigabytes) y comparten las librerías del kernel anfitrión. Las máquinas virtuales requieren gigabytes o decenas de gigabytes por imagen de disco al incluir un sistema operativo completo duplicado, consumiendo RAM y CPU fijos asignados por el hipervisor.
- **Aislamiento:** Las máquinas virtuales ofrecen un aislamiento estricto a nivel de hardware garantizado por el hipervisor tipo 1 o tipo 2. Los contenedores proporcionan aislamiento a nivel de proceso mediante características del kernel de Linux (*namespaces* para visibilidad y *cgroups* para límites de recursos), compartiendo el mismo núcleo del sistema operativo.

## 2. Glosario Técnico
- **Imagen:** Plantilla de solo lectura construida a base de capas superpuestas que contiene las instrucciones, binarios, librerías y configuraciones necesarias para ejecutar una aplicación.
- **Contenedor:** Instancia ejecutable e interactiva de una imagen. Añade una delgada capa de lectura/escritura sobre las capas inmutables de la imagen base.
- **Volumen:** Mecanismo preferido por Docker para persistir datos generados o utilizados por contenedores. Es un directorio o espacio de almacenamiento administrado directamente por el motor de Docker en el sistema de archivos del anfitrión, fuera del ciclo de vida del contenedor.
- **Puerto publicado (Port Forwarding):** Mapeo de red que vincula un puerto TCP/UDP del sistema anfitrión (host) con un puerto interno expuesto por el contenedor (por ejemplo, `5432:5432`), permitiendo que clientes externos fuera de la red virtual de Docker se comuniquen con el servicio.

## 3. Importancia de los Volúmenes y Persistencia de Datos
Por diseño, la capa de escritura de un contenedor es **efímera y volátil**. Esto significa que cualquier archivo creado o modificado dentro del contenedor se destruye automáticamente cuando este se elimina (`docker rm`).

En un Sistema Gestor de Bases de Datos (SGBD) como PostgreSQL, no declarar un volumen provocaría la pérdida irreversible de todas las tablas, esquemas y registros ante cualquier reinicio, fallo o actualización del contenedor. El volumen es indispensable porque desacopla el almacenamiento físico de la capa de cómputo del contenedor, garantizando la persistencia y durabilidad de los datos según las propiedades ACID.