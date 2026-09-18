# Ejercicio 1: Control de Versiones con Git y GitHub

## 1. ¿Qué es un sistema de control de versiones y qué problema resuelve?
Un Sistema de Control de Versiones (VCS) es un software encargado de registrar los cambios realizados sobre un conjunto de archivos a lo largo del tiempo, permitiendo recuperar versiones específicas en cualquier momento.

En un entorno colaborativo resuelve problemas críticos como:
- **Sobrescritura accidental:** Evita que el trabajo de un desarrollador sobreescriba el de otro cuando ambos modifican el mismo archivo.
- **Trazabilidad:** Permite identificar con exactitud quién hizo un cambio, cuándo y cuál fue la justificación detrás del mismo.
- **Historial recuperable:** Facilita regresar a un estado funcional si una nueva implementación rompe el código en producción.

## 2. Diferencia entre Git y GitHub
Git y GitHub no son lo mismo:
- **Git:** Es una herramienta de software local de control de versiones distribuido. Funciona directamente en el sistema operativo del desarrollador sin necesidad de conexión a internet para registrar cambios, ramas o fusiones.
- **GitHub:** Es una plataforma en la nube (servicio de alojamiento) para repositorios Git. Proporciona una interfaz gráfica, herramientas de revisión de código (Pull Requests), gestión de incidencias (Issues), integración continua y mecanismos de colaboración en equipo.

# 3. Glosario de Conceptos Clave
- **Repositorio:** Espacio de almacenamiento donde se guardan todos los archivos de un proyecto junto con su historial completo de cambios y metadatos (`.git`).
  * *Ejemplo:* La carpeta local `practica-1-basededatos` vinculada con la URL remota en GitHub.
- **Confirmación (Commit):** Instantánea (*snapshot*) del estado de los archivos en un instante determinado.
  * *Ejemplo:* Guardar el archivo `.gitignore` con el identificador `c76c051`.
- **Rama (Branch):** Línea independiente de desarrollo que diverge de la línea principal para trabajar en nuevas características sin alterar el código estable.
  * *Ejemplo:* La rama `feature/investigacion-git` separada de `main`.
- **Fusión (Merge):** Operación que integra el historial y cambios de una rama secundaria dentro de otra rama receptora (generalmente `main`).
  * *Ejemplo:* Unir los cambios aprobados de `feature/investigacion-git` en `main`.
- **Conflicto de fusión:** Situación producida cuando dos ramas modifican la misma línea de un archivo de manera distinta y Git no puede resolver automáticamente cuál versión conservar, requiriendo intervención manual.
  * *Ejemplo:* Dos desarrolladores editan el título del `README.md` simultáneamente con textos diferentes.
- **Pull Request (PR):** Propuesta formal en una plataforma como GitHub para fusionar los cambios de una rama hacia otra, habilitando un espacio para revisión de código, comentarios y validaciones automatizadas.
  * *Ejemplo:* Solicitar la revisión del texto de investigación antes de integrarlo a `main`.
- **Archivo .gitignore:** Archivo de texto plano que le indica a Git qué archivos o carpetas del directorio de trabajo deben ser omitidos del control de versiones.
  * *Ejemplo:* Ignorar `.env` o archivos de configuración local para proteger credenciales.
- **Archivo README:** Documento principal que describe el propósito del proyecto, instrucciones de instalación, autoría y contenido general.
  * *Ejemplo:* El archivo `README.md` con los datos del alumno y el índice de la práctica.

## 4. Flujo de trabajo basado en ramas y revisión por pares
Un flujo basado en ramas (*Feature Branch Workflow*) establece que la rama principal (`main`) solo debe contener código estable, probado y listo para producción. Todo desarrollo nuevo, corrección de errores o documentación debe aislarse en una rama específica.

La **revisión entre pares** (*Code Review*) antes de fusionar el código es indispensable porque:
1. **Detección temprana de errores:** Permite identificar inconsistencias de lógica, sintaxis o seguridad antes de que lleguen a la rama principal.
2. **Homogeneidad y buenas prácticas:** Asegura que el código respete los estándares del equipo.
3. **Transferencia de conocimiento:** Garantiza que más de un miembro del equipo comprenda la lógica implementada.