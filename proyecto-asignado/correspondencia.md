# Modelo EER del Proyecto Asignado y Correspondencia con el Esquema Relacional

* **Proyecto:** Data Warehouse sobre consumo de agua en CDMX (SACMEX).
* **Repositorio:** `Data_Warehouse_static`
* **Commit analizado:** `b9366f0014b5bfc401b927f318daab6f4bc2163`

---

## 1. Tabla de Correspondencia entre Modelo EER y Esquema Físico (PostgreSQL)

| Elemento en Modelo EER | Concepto EER | Elemento en Base de Datos | Justificación y Regla Semántica |
| :--- | :--- | :--- | :--- |
| **`DIM_UBICACION`** | Entidad Fuerte | Tabla `dim_ubicacion` | Entidad independiente con clave primaria propia (`id_ubicacion`). Representa la unidad espacial territorial (alcaldía, colonia). |
| **`DIM_TIEMPO`** | Entidad Fuerte | Tabla `dim_tiempo` | Cataloga el ciclo cronológico bimestral y anual (`id_tiempo`). |
| **`DIM_INDICE_DES`** | Entidad Fuerte | Tabla `dim_indice_des` | Clasificación socioeconómica del territorio (`id_indice`, estrato de desarrollo). |
| **`FACT_CONSUMO_AGUA`** | Entidad / Hecho Relacional | Tabla `fact_consumo_agua` | Núcleo de medición con cardinalidad `(1, 1)` hacia cada dimensión referenciada. Contiene las métricas numéricas agregadas de volumen de agua consumido. |
| **`FACT_CLIMA`** | Entidad / Hecho Relacional | Tabla `fact_clima` | Almacena mediciones climáticas (temperatura, precipitación, días con olas de calor) compartiendo dimensiones espaciales y temporales. |
| **`DIM_UBICACION_ADYACENCIA`** | Relación M:N recursiva reificada | Tabla `dim_ubicacion_adyacencia` | Modela la relación topológica de vecindad territorial entre colonias/alcaldías. Posee clave primaria compuesta `{id_ubicacion_origen, id_ubicacion_vecino}`. |
| **VISTA GEOGRÁFICA** | Vista Semántica Derivada | Vista `v_ubicacion_geom` | Integración de atributos descriptivos con datos espaciales vectoriales para visualización cartográfica en el frontend sin duplicar almacenamiento. |

---

## 2. Cardinalidades del Esquema

1. **Ubicación vs. Hechos de Consumo:**
   * `[DIM_UBICACION] ──(0, n)── [FACT_CONSUMO_AGUA]` y `[FACT_CONSUMO_AGUA] ──(1, 1)── [DIM_UBICACION]`
   * Una ubicación territorial puede tener múltiples registros de consumo bimestral a lo largo de los años o ninguno en periodos no censados. Todo hecho de consumo está forzosamente ligado a una única demarcación territorial identificada.

2. **Tiempo vs. Hechos de Consumo:**
   * `[DIM_TIEMPO] ──(0, n)── [FACT_CONSUMO_AGUA]` y `[FACT_CONSUMO_AGUA] ──(1, 1)── [DIM_TIEMPO]`
   * Un bimestre puede tener asociados cientos de registros de consumo por colonia. Todo consumo se restringe estrictamente a un único periodo temporal.

3. **Adyacencia Espacial Recursiva:**
   * `[DIM_UBICACION] ──(0, n)── <limita_con> ──(0, n)── [DIM_UBICACION]`
   * Una colonia colinda con cero (isla territorial) o múltiples colonias adyacentes.

---

## 3. Justificación de Decisiones de Modelado

1. **Esquema Multidimensional Constelación (Fact Constellation):**
   * Se optó por separar `fact_consumo_agua` y `fact_clima` debido a que provienen de fuentes institucionales con diferente granularidad física (SACMEX para consumo por colonia y estaciones meteorológicas para clima regional). Compartir las dimensiones de tiempo y ubicación permite hacer análisis cruzado de impacto sin generar redundancia ni inconsistencias de normalización en una sola tabla de hechos.
2. **Reificación de la relación de adyacencia (`dim_ubicacion_adyacencia`):**
   * En lugar de una clave foránea simple, se modeló una tabla puente recursiva. Esto habilita consultas analíticas espaciales complejas (como propagación de escasez o análisis de zonas limítrofes con diferente estrato tarifario).
3. **Uso de vistas dedicadas (`v_ubicacion_geom`):**
   * Desacopla la lógica pesada de serialización geográfica vectorial de las consultas tabulares de agregación rápida ejecutadas por la interfaz web.
   