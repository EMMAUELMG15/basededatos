# 4.1. Descripción del problema y requisitos ampliados

El sistema tiene como objetivo llevar el control de las operaciones principales de una clínica u hospital de segundo nivel. Principalmente se busca administrar la información de los pacientes, médicos y enfermeros, así como las citas médicas, las notas de evolución y las recetas que se generan durante las consultas.

El sistema permitirá tener un expediente clínico electrónico de cada paciente, donde se pueda consultar su información y el seguimiento que ha tenido durante sus diferentes consultas. También se busca llevar un control de los medicamentos que son recetados y de los datos relacionados con cada tratamiento.

# 4.2. Conceptos del modelo extendido

## 1. Relaciones binarias y cardinalidades

### Relación 1: Registro y asignación de citas

**[PACIENTE] - [agenda] - [CITA_MEDICA]**

**Cardinalidad:** `(0,n) : (1,1)`

Un paciente puede estar registrado en el sistema aunque todavía no tenga ninguna cita médica, por eso el mínimo es 0. También puede tener varias citas a lo largo del tiempo, por lo que el máximo es n.

Por otro lado, cada cita médica debe pertenecer obligatoriamente a un solo paciente, por lo que la cita tiene una cardinalidad de `(1,1)`.

### Relación 2: Atención profesional de consultas

**[MEDICO] - [atiende] - [CITA_MEDICA]**

**Cardinalidad:** `(0,n) : (1,1)`

Un médico puede estar registrado y en determinado momento no tener ninguna cita asignada, por lo que el mínimo es 0. También puede atender varias citas durante su jornada.

Cada cita debe tener asignado un médico responsable de la consulta, por lo que una cita solamente puede estar relacionada con un médico.

### Relación 3: Emisión de prescripción farmacológica

**[CITA_MEDICA] - [genera] - [RECETA]**

**Cardinalidad:** `(0,1) : (1,1)`

No todas las consultas necesitan generar una receta, por ejemplo, una consulta de valoración o seguimiento puede terminar sin medicamentos. Por eso una cita puede tener 0 o 1 receta.

Cuando existe una receta, esta debe estar relacionada obligatoriamente con una sola cita médica.

---

## 2. Entidades débiles

### Entidad débil 1: `NOTA_EVOLUCION`

**Entidad fuerte de la que depende:** `CITA_MEDICA`

La nota de evolución depende de una cita médica, ya que registra información obtenida durante la consulta, como los hallazgos, signos vitales, diagnóstico y evolución del paciente.

No tendría sentido tener una nota de evolución sin una consulta a la cual pertenezca. Además, su identificación se realiza utilizando el `id_cita` junto con un número consecutivo.

Su clave primaria sería:

`{id_cita, num_consecutivo}`

Por lo tanto, tiene dependencia por existencia y por identificación.

### Entidad débil 2: `DETALLE_RECETA`

**Entidad fuerte de la que depende:** `RECETA`

El detalle de receta representa cada medicamento que se incluye dentro de una receta. En él se pueden guardar datos como el medicamento, dosis, vía de administración, frecuencia y duración.

Un detalle no puede existir por sí solo, ya que siempre debe pertenecer a una receta.

Su clave primaria sería:

`{folio_receta, renglon_id}`

Por lo tanto, depende de la receta para poder identificarse.

---

## 3. Generalización y especialización

La superclase será:

**`EMPLEADO_SALUD`**

Esta entidad tendrá los datos que son comunes para los diferentes trabajadores de salud:

* `id_empleado` (PK)
* `rfc`
* `nombre_completo`
* `correo_institucional`
* `telefono_contacto`

A partir de esta entidad se tienen dos subtipos:

### `MEDICO`

Atributos propios:

* `cedula_profesional`
* `especialidad_clinica`

### `ENFERMERO`

Atributos propios:

* `turno_laboral`
* `area_adscripcion`

La especialización será **disjunta (d)**, ya que un empleado no puede ser médico y enfermero al mismo tiempo dentro de la institución.

También será **total (t)**, porque todo empleado de salud registrado debe pertenecer a alguno de los dos subtipos.

---

## 4. Otros conceptos utilizados

### Atributo compuesto

En `PACIENTE` se utilizará el atributo compuesto `direccion`, que estará formado por:

* `calle`
* `num_exterior`
* `colonia`
* `codigo_postal`
* `alcaldia_municipio`

Esto permite dividir la dirección en diferentes partes en lugar de manejarla como un solo dato.

### Atributo multivaluado

También se tendrá el atributo `alergias_conocidas`, ya que un paciente puede tener ninguna, una o varias alergias.

Por esta razón se representa como un atributo multivaluado.

# 4.3. Notaciones utilizadas para los diagramas

Para realizar los diagramas se utilizará **draw.io**, ya que permite crear los dos tipos de diagramas solicitados y no requiere una suscripción.

Se realizarán dos diagramas:

### `eer-chen.png`

En este diagrama se utilizará la notación de Peter Chen.

Las entidades fuertes se representarán mediante rectángulos normales y las entidades débiles mediante rectángulos dobles.

Las relaciones se representarán mediante rombos y las relaciones identificadoras de las entidades débiles mediante rombos dobles.

Los atributos se representarán mediante óvalos. El atributo `alergias_conocidas` tendrá un óvalo doble debido a que es multivaluado y `direccion` tendrá diferentes ramas para representar sus componentes.

La generalización de `EMPLEADO_SALUD` se representará mediante un triángulo invertido conectado con `MEDICO` y `ENFERMERO`. Se indicará la especialización total con una doble línea y la letra `d` para indicar que es disjunta.

También se colocarán las cardinalidades `(mín,máx)` directamente sobre las relaciones.

### `eer-crows-feet.png`

En este diagrama se utilizará la notación Crow's Feet o pata de cuervo.

Cada entidad se representará como una tabla dividida en diferentes secciones, mostrando su nombre, sus claves primarias y sus atributos.

Las relaciones utilizarán los símbolos correspondientes para indicar si una relación es obligatoria u opcional y si puede existir uno o varios registros relacionados.

La especialización de `EMPLEADO_SALUD` se representará mediante las tablas `MEDICO` y `ENFERMERO`, utilizando `id_empleado` como clave primaria y también como clave foránea hacia `EMPLEADO_SALUD`.

# 4.4. Justificación del modelo y consultas

## Justificación de las decisiones tomadas

Las entidades débiles se utilizaron porque tanto las notas de evolución como los detalles de las recetas dependen de otra entidad para poder existir correctamente.

Por ejemplo, una nota de evolución necesita estar relacionada con una cita médica y un detalle de receta necesita pertenecer a una receta. Esto también ayuda a evitar que existan registros que no estén relacionados con una consulta o receta existente.

La especialización de `EMPLEADO_SALUD` permite separar las funciones de los médicos y enfermeros. De esta forma, cada uno puede tener información propia dependiendo de su función dentro del hospital.

La especialización es total porque todos los empleados registrados deben tener uno de estos dos roles y es disjunta porque un empleado no puede pertenecer a ambos al mismo tiempo.

La relación entre `CITA_MEDICA` y `RECETA` permite que una consulta pueda terminar sin medicamentos, pero también permite registrar una receta cuando el médico considere necesario utilizar un tratamiento.

## Consultas que se pueden realizar

### 1. Seguimiento de tratamientos

Se pueden consultar los médicos y sus especialidades que hayan registrado en sus notas de evolución situaciones como `"sin mejoría"` o `"recaída"` y relacionar esta información con los medicamentos que fueron recetados.

Esto permite conocer qué tratamientos fueron utilizados y cómo fue registrada la evolución del paciente.

### 2. Detección de posibles alergias

Se pueden comparar las alergias conocidas de un paciente con los medicamentos incluidos en los detalles de una receta.

De esta manera, el sistema puede ayudar a detectar posibles problemas antes de que se entregue el medicamento.

### 3. Consulta del historial de notas

También se puede obtener el historial de las notas de evolución de un paciente, ordenándolas por fecha y relacionándolas con la cita y el médico que realizó la consulta.

Esto permite tener un seguimiento más completo de la evolución del paciente.
