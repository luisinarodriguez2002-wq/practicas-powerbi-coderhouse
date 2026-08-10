# practicas-powerbi-coderhouse
# Power BI — Conectividad y Transformación de Datos

## Descripción

En esta práctica se trabajó con un archivo de ventas exportado desde un sistema legacy. El objetivo fue transformar y preparar los datos mediante Power Query en Power BI para que puedan ser utilizados posteriormente en un modelo de análisis.

El proceso incluyó la importación del archivo, renombrado de columnas, corrección de tipos de datos, tratamiento de valores nulos y duplicados y separación de la información en dos tablas: Clientes y Ventas.

---

## Transformaciones realizadas

### 1. Importación de datos

Se importó el archivo `Ventas_export_legacy.xlsx` utilizando el conector de Excel de Power BI.

Los datos fueron cargados inicialmente en Power Query para realizar las transformaciones antes de incorporarlos al modelo.

### 2. Renombrado de columnas

Se reemplazaron los nombres técnicos provenientes del sistema original por nombres descriptivos en español y utilizando `snake_case`.

Por ejemplo:

- `COD_OP` → `codigo_operacion`
- `F_VTA` → `fecha_venta`
- `COD_CLI` → `codigo_cliente`
- `NOM_CLI` → `nombre_cliente`
- `PU_VTA` → `precio_unitario`
- `TOTAL_VTA` → `total_venta`
- `CANAL_VTA` → `canal_venta`

Esto facilita la interpretación de los datos y mejora la legibilidad y mantenimiento del modelo.

### 3. Corrección de tipos de datos

Se asignaron tipos de datos según el significado de cada columna:

- Las fechas (`fecha_venta` y `fecha_alta_cliente`) fueron configuradas como **Fecha**.
- Los identificadores y cantidades fueron configurados como **Número entero**.
- Los precios, descuentos y totales fueron configurados como **Número decimal**.
- Los nombres, emails, teléfonos, ciudades, provincias, segmentos, monedas y canales fueron configurados como **Texto**.

El teléfono fue tratado como texto porque no representa un valor sobre el cual realizar operaciones matemáticas y puede contener formatos especiales o ceros iniciales.

### 4. Tratamiento de valores nulos

Se revisaron los valores nulos presentes en el archivo.

Los valores faltantes en `email_cliente` y `telefono_cliente` fueron conservados, ya que representan información de contacto que no fue registrada y reemplazarlos por datos ficticios podría alterar la información original.

En cambio, los valores nulos de `descuento_porcentaje` fueron reemplazados por `0`, interpretando que una ausencia de descuento representa que la operación no tuvo descuento.

### 5. Tratamiento de duplicados

Se eliminaron las filas completamente duplicadas de los datos originales.

Posteriormente, al crear la tabla de clientes, se eliminaron los registros duplicados utilizando `codigo_cliente`, ya que un mismo cliente puede aparecer varias veces en el archivo original debido a que puede realizar múltiples compras.

De esta manera, la tabla Clientes contiene un único registro por cliente.

---

## Separación de la información

Para normalizar la estructura de los datos se crearon dos tablas independientes mediante la duplicación de la consulta original en Power Query.

### Tabla Clientes

Contiene la información correspondiente a cada cliente:

- `codigo_cliente`
- `nombre_cliente`
- `email_cliente`
- `telefono_cliente`
- `ciudad_cliente`
- `provincia_cliente`
- `segmento_cliente`
- `fecha_alta_cliente`
- `cliente_activo`

Se utilizó `codigo_cliente` como identificador para evitar registros duplicados.

### Tabla Ventas

Contiene la información correspondiente a cada operación:

- `codigo_operacion`
- `fecha_venta`
- `codigo_cliente`
- `codigo_producto`
- `descripcion_producto`
- `rubro`
- `cantidad`
- `precio_unitario`
- `descuento_porcentaje`
- `total_venta`
- `moneda`
- `canal_venta`

Se mantuvo `codigo_cliente` en esta tabla porque permite relacionar cada venta con el cliente correspondiente.

---

## Resultado final

Luego de realizar las transformaciones, los datos quedaron organizados en dos tablas:

**Clientes:** información única de cada cliente.

**Ventas:** información de cada operación comercial, manteniendo el identificador del cliente para establecer posteriormente la relación entre ambas tablas.

El resultado final permite trabajar con datos más limpios, consistentes y descriptivos, preparados para las siguientes etapas de modelado y análisis en Power BI.
