# Reporte de Levantamiento del Proyecto Asignado

* **Proyecto asignado:** Consumo de agua en la Ciudad de México (Almacén dimensional sobre datos abiertos de SACMEX).
* **Repositorio original:** https://github.com/gabrielhuav/Data_Warehouse_static
* **Fork del equipo:** [Registrar aquí la URL de tu fork en GitHub]
* **Commit ID (HEAD verificado):** `b9366f0014b5bfc401b927f318daab6f4bc2163`

---

## 1. Requisitos del Sistema y Prerrequisitos

Para la ejecución local del proyecto se requirió:
* **Sistema Operativo:** Windows 11 con terminal PowerShell.
* **Docker Desktop:** Versión moderna con soporte para Docker Compose y WSL2 Engine.
* **Visual Studio Code:** Con terminal integrada y extensión Live Server / visualizador estático.
* **Cliente PostgreSQL (`psql`):** Embebido en la imagen del contenedor oficial de PostgreSQL 16.

---

## 2. Pasos Exactos de Ejecución

1. **Clonación del repositorio asignado:**
   Se clonó el fork en un directorio de trabajo independiente de la máquina local:
   ```bash
   git clone [https://github.com/TU_USUARIO/Data_Warehouse_static.git](https://github.com/TU_USUARIO/Data_Warehouse_static.git)
   cd Data_Warehouse_static
   ```

2. **Inspección del entorno dimensional:**
   Se identificó la presencia de un entorno contenedorizado en la carpeta `warehouse/`:
   * Archivo de orquestación: `warehouse/compose.yml`
   * Definición de imagen: `warehouse/Dockerfile`
   * Scripts de carga y DDL: subdirectorios `ddl/`, `etl/`, `scripts/`

3. **Levantamiento de la base de datos:**
   Se ejecutó Docker Compose para construir y levantar el contenedor del Data Warehouse:
   ```powershell
   cd warehouse
   docker compose up --build
   ```
   El contenedor `data_warehouse_cdmx` ejecutó de forma automatizada las sentencias DDL y los scripts de inicialización, abriendo el servicio en el puerto `5433` del host local (`5432` interno).

4. **Levantamiento de la interfaz web:**
   Desde la raíz del proyecto se abrió `index.html` sirviendo los recursos estáticos localmente mediante un servidor HTTP local para verificar los componentes visuales (`mapa.html`, `grafo.html`, visualizaciones cartográficas).

5. **Conexión interactiva y consulta a la Base de Datos:**
   Se accedió al motor PostgreSQL dentro del contenedor en ejecución mediante `docker exec`:
   ```powershell
   docker exec -it data_warehouse_cdmx psql -U postgres -d data_warehouse
   ```
   Se inspeccionaron las relaciones creadas con `\dt` y se ejecutó la consulta de comprobación analítica:
   ```sql
   SELECT * FROM fact_consumo_agua LIMIT 5;
   ```

---

## 3. Problemas Encontrados y Soluciones Aplicadas

### Problema 1: Incompatibilidad de argumentos en PowerShell (`mkdir -p`)
* **Síntoma:** Al preparar las carpetas de la práctica con la sintaxis de Unix/Linux (`mkdir -p carpeta1 carpeta2/sub`), PowerShell arrojó: `InvalidArgument: ParameterBindingException / PositionalParameterNotFound`.
* **Causa:** El alias `mkdir` en PowerShell invoca el cmdlet `New-Item`, el cual no acepta múltiples rutas de destino combinadas con el parámetro posicional `-p`.
* **Solución:** Se invocó de forma declarativa `New-Item -ItemType Directory -Force -Path "<directorio>"` para cada ruta requerida.

### Problema 2: Demonio de Docker no accesible (`npipe / dockerDesktopLinuxEngine`)
* **Síntoma:** Al intentar ejecutar `docker compose up`, la terminal devolvió el error:
  `failed to connect to the docker API at npipe:////./pipe/dockerDesktopLinuxEngine; check if the path is correct and if the daemon is running`.
* **Causa:** El servicio Docker Desktop no se encontraba en ejecución o su motor WSL2 no había inicializado.
* **Solución:** Se arrancó el aplicativo Docker Desktop en Windows y se esperó a la confirmación de estado *"Engine running"*. Al reintentar `docker compose up`, la construcción procedió correctamente.

### Problema 3: Separación sintáctica en el cliente interactivo `psql`
* **Síntoma:** Al pegar conjuntamente el meta-comando `\dt` y la sentencia `SELECT`, el parser de `psql` interpretó las palabras clave SQL como argumentos de búsqueda de relaciones para `\dt`, devolviendo `Did not find any relation named "SELECT"`.
* **Causa:** Los meta-comandos de `psql` iniciados con barra invertida (`\`) consumen el resto de la línea física como argumentos propios.
* **Solución:** Se ejecutó `\dt` de manera aislada en su propia línea para obtener la lista de tablas y posteriormente se introdujo la consulta SQL delimitada por `;`.

---

## 4. Evidencias

Las capturas de pantalla que respaldan este procedimiento se encuentran en la carpeta `evidencias/`:
* `arranque_terminal.png`: Logs de arranque y verificación de PostgreSQL listo para aceptar conexiones.
* `consulta_bd.png`: Inspección del catálogo relacional y salida tabular de la consulta a la tabla de hechos.
* `interfaz_web.png`: Interfaz del sistema cargada localmente.