# Propuestas de Mejora al Proyecto Asignado (Data Warehouse SACMEX)

* **Proyecto:** Visualización y Análisis del Consumo de Agua en la CDMX (`Data_Warehouse_static`).
* **Autor:** Emmanuel Morales Gonzalez (Trabajo Individual).
* **Commit base:** `b9366f0014b5bfc401b927f318daab6f4bc2163`

---

## Propuesta 1: Ingesta de Telemetría y Monitoreo de Fugas en Red Primaria (Macromedición)

* **Enlace al Issue en GitHub:** [Pega aquí el enlace al Issue #1 que abras en tu repositorio de GitHub]
* **Motivación y Justificación Teórica:**  
  En el artículo de referencia sobre la gestión del agua en CDMX (Velázquez Arrieta et al., en prensa), los autores destacan que una de las principales limitaciones del almacén de datos es la latencia de actualización bimestral de SACMEX y la incapacidad de detectar pérdidas físicas de agua en tránsito. En la Ciudad de México se estima que entre el 35% y el 40% del caudal se pierde en fugas sobre la red primaria y secundaria. Incorporar datos provenientes de estaciones de telemetría y sensores de caudal permite transitar de un análisis histórico pasivo a un monitoreo cuasi-real de anomalías de presión y fugas.
* **Descripción Detallada:**  
  Se propone extender el esquema dimensional para recibir lecturas horarias de caudal y presión hidráulica emitidas por estaciones de macromedición sectoriales. El sistema calculará desviaciones entre el volumen inyectado a un sector hidrométrico y la sumatoria del consumo facturado reportado en `fact_consumo_agua`, permitiendo generar alertas automáticas de fugas o tomas clandestinas.
* **Impacto Esperado:**  
  Reducción del tiempo de respuesta ante rupturas de tuberías principales y mayor precisión en el balance hidráulico por demarcación territorial.
* **Cambios en el Modelo de Datos (EER / Relacional):**
  * **Nueva Dimensión `dim_sensor_macromedidor`:**
    * `id_sensor` (PK, INT)
    * `codigo_sector` (VARCHAR)
    * `id_ubicacion` (FK hacia `dim_ubicacion`, INT)
    * `modelo_hardware` (VARCHAR)
    * `fecha_instalacion` (DATE)
  * **Nueva Tabla de Hechos `fact_telemetria_presion`:**
    * `id_hecho_telemetria` (PK, BIGINT)
    * `id_sensor` (FK hacia `dim_sensor_macromedidor`, INT)
    * `id_tiempo` (FK hacia `dim_tiempo`, INT)
    * `timestamp_medicion` (TIMESTAMP)
    * `caudal_litros_segundo` (NUMERIC)
    * `presion_psi` (NUMERIC)
    * `alerta_fuga` (BOOLEAN)

---

## Propuesta 2: Estratificación Tarifaria Diferenciada y Elasticidad Económica del Consumo

* **Enlace al Issue en GitHub:** [Pega aquí el enlace al Issue #2 que abras en tu repositorio de GitHub]
* **Motivación y Justificación Teórica:**  
  Actualmente, el proyecto asignado cuenta con la dimensión `dim_indice_des` que agrupa niveles de marginación/desarrollo social de manera agregada, pero carece de visibilidad sobre los esquemas de subsidio, tipos de uso de predio (doméstico, mixto, comercial, industrial) y rangos tarifarios reales aplicados en el cobro. Los autores de los artículos señalan la necesidad de estudiar cómo las variaciones tarifarias y las políticas de cobro incentivan o desincentivan el ahorro de agua.
* **Descripción Detallada:**  
  Se propone modelar explícitamente el régimen tarifario y el tipo de predio dentro de la cadena analítica. Esto permitirá correlacionar no solo el volumen consumido, sino el costo económico facturado, los montos subsidiados por el gobierno de la ciudad y el comportamiento de pago en colonias con distintos niveles de ingreso.
* **Impacto Esperado:**  
  Capacidad analítica para que la Secretaría de Finanzas y SACMEX simulen el impacto distributivo de ajustes en las tarifas escalonadas y focalicen subsidios en áreas de alta vulnerabilidad económica.
* **Cambios en el Modelo de Datos (EER / Relacional):**
  * **Nueva Dimensión `dim_regimen_tarifario`:**
    * `id_regimen` (PK, INT)
    * `tipo_uso_suelo` (VARCHAR: 'Doméstico Bajo', 'Doméstico Medio', 'Comercial', 'Industrial')
    * `rango_consumo_m3` (VARCHAR)
    * `cuota_base` (NUMERIC)
    * `porcentaje_subsidio` (NUMERIC)
  * **Modificación a `fact_consumo_agua`:**
    * Adición de columna `id_regimen` (FK hacia `dim_regimen_tarifario`, INT)
    * Adición de columna `monto_facturado_mxn` (NUMERIC)
    * Adición de columna `monto_subsidio_mxn` (NUMERIC)

---

## Propuesta 3: Integración de Capas Cartográficas de Vulnerabilidad Hidrogeológica y Hundimiento

* **Enlace al Issue en GitHub:** [Pega aquí el enlace al Issue #3 que abras en tu repositorio de GitHub]
* **Motivación y Justificación Teórica:**  
  Siguiendo el enfoque del Artículo 1 (Villa Vargas et al., 2026) y el Artículo 2, los fenómenos geológicos y territoriales están estrechamente entrelazados con la infraestructura hídrica de la CDMX. La sobreexplotación de los mantos acuíferos es el causante directo de los hundimientos diferenciales en el subsuelo lacustre del Valle de México, lo que fractura tuberías de drenaje y agua potable. En el repositorio actual solo se contempla una vista geográfica elemental (`v_ubicacion_geom`).
* **Descripción Detallada:**  
  Incorporar una dimensión geoespacial especializada en geotecnia y subsidencia del suelo proveniente de estudios de radar interferométrico (InSAR) e información del Centro Nacional de Prevención de Desastres (CENAPRED). Esto permitirá cruzar en el Data Warehouse la tasa de hundimiento anual ($cm/año$) y el tipo de suelo de transición o lacustre con los reportes de desabasto y rotura de tuberías.
* **Impacto Esperado:**  
  Permitir a los planificadores urbanos priorizar el reemplazo de redes hidráulicas en colonias donde la combinación de hundimiento diferencial acelerado y alta demanda de agua genera un riesgo crítico de colapso de infraestructura.
* **Cambios en el Modelo de Datos (EER / Relacional):**
  * **Nueva Dimensión `dim_geotecnia_suelo`:**
    * `id_zona_geotecnica` (PK, INT)
    * `tipo_suelo` (VARCHAR: 'Lomas', 'Transición', 'Lago')
    * `tasa_hundimiento_anual_cm` (NUMERIC)
    * `vulnerabilidad_fractura` (VARCHAR: 'Baja', 'Media', 'Alta', 'Crítica')
    * `poligono_geotecnico` (GEOMETRY / PostGIS)
  * **Modificación / Enlace Relacional:**
    * Vincular `dim_ubicacion` con `dim_geotecnia_suelo` mediante una clave foránea `id_zona_geotecnica` (INT), enriqueciendo la vista `v_ubicacion_geom` con atributos geológicos para el visualizador Leaflet/OpenStreetMap.
    