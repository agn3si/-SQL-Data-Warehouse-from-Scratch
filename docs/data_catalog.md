# Catálogo de datos de la capa Gold

## Descripción general

La capa Gold es la representación de los datos a nivel de negocio, estructurada para casos de uso analíticos y de reportes. Está formada por **tablas de dimensión** y **tablas de hechos** con métricas específicas del negocio (esquema estrella).

---

### 1. gold.dim_customers

- **Propósito:** guarda los datos de los clientes, enriquecidos con información demográfica y geográfica.
- **Columnas:**

| Columna         | Tipo de dato | Descripción                                                                                   |
|-----------------|--------------|-----------------------------------------------------------------------------------------------|
| customer_key    | INT          | Llave sustituta que identifica de forma única cada registro de cliente en la dimensión.       |
| customer_id     | INT          | Identificador numérico único asignado a cada cliente.                                         |
| customer_number | NVARCHAR(50) | Identificador alfanumérico del cliente, usado para seguimiento y referencia.                  |
| first_name      | NVARCHAR(50) | Nombre del cliente, tal como está registrado en el sistema.                                   |
| last_name       | NVARCHAR(50) | Apellido del cliente.                                                                         |
| country         | NVARCHAR(50) | País de residencia del cliente (por ejemplo, 'Australia').                                    |
| marital_status  | NVARCHAR(50) | Estado civil del cliente (por ejemplo, 'Married', 'Single').                                  |
| gender          | NVARCHAR(50) | Género del cliente (por ejemplo, 'Male', 'Female', 'n/a').                                    |
| birthdate       | DATE         | Fecha de nacimiento del cliente, en formato AAAA-MM-DD (por ejemplo, 1971-10-06).             |
| create_date     | DATE         | Fecha en que se creó el registro del cliente en el sistema.                                   |

---

### 2. gold.dim_products

- **Propósito:** contiene la información de los productos y sus atributos (solo productos actuales, sin historial).
- **Columnas:**

| Columna        | Tipo de dato | Descripción                                                                                          |
|----------------|--------------|------------------------------------------------------------------------------------------------------|
| product_key    | INT          | Llave sustituta que identifica de forma única cada registro de producto en la dimensión.             |
| product_id     | INT          | Identificador único asignado al producto para seguimiento y referencia interna.                      |
| product_number | NVARCHAR(50) | Código alfanumérico estructurado que representa el producto (por ejemplo, 'FR-R92B-58').             |
| product_name   | NVARCHAR(50) | Nombre descriptivo del producto, con detalles como tipo, color y tamaño.                             |
| category_id    | NVARCHAR(50) | Identificador único de la categoría del producto, que lo liga a su clasificación general (por ejemplo, 'CO_RF'). |
| category       | NVARCHAR(50) | Clasificación general del producto (por ejemplo, 'Bikes', 'Components').                             |
| subcategory    | NVARCHAR(50) | Clasificación más detallada del producto dentro de su categoría (por ejemplo, 'Road Frames').        |
| maintenance    | NVARCHAR(50) | Indica si el producto requiere mantenimiento (por ejemplo, 'Yes', 'No').                             |
| cost           | INT          | Costo o precio base del producto, en unidades monetarias.                                            |
| product_line   | NVARCHAR(50) | Línea o serie del producto (por ejemplo, 'Road', 'Mountain', 'Touring', 'Other Sales').              |
| start_date     | DATE         | Fecha en que el producto empezó a estar disponible para su venta o uso.                              |

---

### 3. gold.fact_sales

- **Propósito:** guarda los datos transaccionales de ventas para fines analíticos.
- **Regla de negocio:** `sales_amount = quantity * price`.
- **Columnas:**

| Columna       | Tipo de dato | Descripción                                                                                    |
|---------------|--------------|------------------------------------------------------------------------------------------------|
| order_number  | NVARCHAR(50) | Identificador alfanumérico único de cada orden de venta (por ejemplo, 'SO54496').              |
| product_key   | INT          | Llave sustituta que liga la orden con la dimensión de productos.                               |
| customer_key  | INT          | Llave sustituta que liga la orden con la dimensión de clientes.                                |
| order_date    | DATE         | Fecha en que se hizo la orden.                                                                 |
| shipping_date | DATE         | Fecha en que se envió la orden al cliente.                                                     |
| due_date      | DATE         | Fecha límite de pago de la orden.                                                              |
| sales_amount  | INT          | Valor monetario total de la venta de esa línea, en unidades monetarias enteras (por ejemplo, 25). |
| quantity      | INT          | Número de unidades del producto pedidas en esa línea (por ejemplo, 1).                         |
| price         | INT          | Precio por unidad del producto en esa línea, en unidades monetarias enteras (por ejemplo, 25). |
