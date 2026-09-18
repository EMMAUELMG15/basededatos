# Ejercicio 5: Caso de Estudio - Sistema de Gestión de Inventario y Ventas para Tienda de Ropa

## 1. Planteamiento de la Problemática
Una boutique de ropa local experimenta problemas recurrentes en el control de su inventario, desabasto no detectado a tiempo, inconsistencias en los precios cobrados frente a los registrados en bodega y falta de control sobre las ventas realizadas por cada empleado. Se requiere el diseño de una base de datos relacional para centralizar el catálogo de prendas, categorización, gestión de proveedores, compras y transacciones de venta al cliente.

## 2. Entrevista Simulada con el Cliente
- **Pregunta 1:** ¿Qué datos necesita registrar indispensablemente de cada prenda?
  * *Respuesta:* Nombre, código de barras/SKU, talla, color, precio unitario de venta y cantidad en stock.
- **Pregunta 2:** ¿Una prenda pertenece a una o varias categorías?
  * *Respuesta:* Cada prenda pertenece estrictamente a una categoría principal (por ejemplo: pantalones, camisas, chamarras) para mantener orden en los estantes.
- **Pregunta 3:** ¿Cómo gestionan a los proveedores?
  * *Respuesta:* Compramos diferentes prendas a distintos proveedores. Necesitamos sus datos de contacto (RFC/ID, razón social, teléfono y correo) para contactarlos cuando se agota el inventario.
- **Pregunta 4:** ¿Cómo se estructura un ticket de compra o venta?
  * *Respuesta:* Una venta la realiza un empleado específico a un cliente en una fecha y hora determinada. Un mismo cliente puede llevarse varias prendas en una misma compra con sus respectivas cantidades y subtotales.

## 3. Requerimientos de Información y Funcionales
- **RF1 - Catálogo e Inventario:** Registrar prendas con precio, stock, color y talla vinculadas a su categoría y proveedor.
- **RF2 - Gestión de Actores:** Registrar clientes (nombre, teléfono, correo) y empleados (nombre, puesto, fecha de ingreso).
- **RF3 - Registro de Transacciones:** Emitir una venta con folio, fecha y total, desglosando cada ítem mediante un detalle de venta.
- **RF4 - Integridad:** Cada venta debe descontar stock y garantizar que una venta eliminada o anulada mantenga su histórico contable.

## 4. Definición de Entidades, Atributos y Claves
- **CATEGORIA:** `id_categoria` (PK), `nombre`, `descripcion`.
- **PROVEEDOR:** `id_proveedor` (PK), `nombre_contacto`, `telefono`, `correo`, `direccion`.
- **PRODUCTO:** `id_producto` (PK), `nombre`, `talla`, `color`, `precio`, `stock`, `id_categoria` (FK), `id_proveedor` (FK).
- **EMPLEADO:** `id_empleado` (PK), `nombre`, `puesto`, `telefono`.
- **CLIENTE:** `id_cliente` (PK), `nombre`, `telefono`, `correo`.
- **VENTA:** `id_venta` (PK), `fecha_hora`, `total`, `id_cliente` (FK), `id_empleado` (FK).
- **DETALLE_VENTA:** `id_detalle` (PK), `id_venta` (FK), `id_producto` (FK), `cantidad`, `precio_unitario`, `subtotal`.

## 5. Justificación del Modelo
- **Normalización y redundancia controlada:** Se desacopla `CATEGORIA` y `PROVEEDOR` de `PRODUCTO` para evitar actualizar el nombre del proveedor en múltiples registros de ropa.
- **Relación muchos a muchos resuelta:** La relación entre `VENTA` y `PRODUCTO` es de cardinalidad $M:N$ (un producto se vende en muchas compras y una compra contiene muchos productos). Se implementó la entidad débil/asociativa `DETALLE_VENTA` con clave foránea compuesta o ID propio para almacenar cantidad y precio congelado al momento de la venta.
- **Trazabilidad:** La vinculación de `CLIENTE` y `EMPLEADO` en `VENTA` permite auditar quién atendió la compra y asociar programas de fidelización al comprador.