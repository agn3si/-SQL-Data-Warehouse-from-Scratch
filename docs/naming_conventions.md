# Convenciones de nombres

Este documento describe las convenciones de nombres usadas para los esquemas, tablas, vistas, columnas y demás objetos del data warehouse.

## Principios generales

- **Estilo de nombres**: usar snake_case, con letras minúsculas y guiones bajos (`_`) para separar las palabras.
- **Idioma**: usar inglés para todos los nombres.
- **Evitar palabras reservadas**: no usar palabras reservadas de SQL como nombres de objetos.

## Convenciones para tablas

### Reglas de Bronze

- Todos los nombres deben empezar con el nombre del sistema fuente, y el nombre de la tabla debe coincidir con su nombre original, sin renombrarla.
- **`<sistemafuente>_<entidad>`**
  - `<sistemafuente>`: nombre del sistema fuente (por ejemplo, `crm`, `erp`).
  - `<entidad>`: nombre exacto de la tabla en el sistema fuente.
  - Ejemplo: `crm_customer_info` → información de clientes del sistema CRM.

### Reglas de Silver

- Mismas reglas que bronze: todos los nombres empiezan con el nombre del sistema fuente y conservan su nombre original, sin renombrarlos.
- **`<sistemafuente>_<entidad>`**
  - Ejemplo: `crm_customer_info` → información de clientes del sistema CRM.

### Reglas de Gold

- Todas las tablas deben tener nombres significativos y alineados con el negocio, empezando con el prefijo de su categoría.
- **`<categoria>_<entidad>`**
  - `<categoria>`: describe el rol de la tabla, como `dim` (dimensión) o `fact` (tabla de hechos).
  - `<entidad>`: nombre descriptivo de la tabla, alineado con el dominio del negocio (por ejemplo, `customers`, `products`, `sales`).
  - Ejemplos:
    - `dim_customers` → tabla de dimensión con los datos de clientes.
    - `fact_sales` → tabla de hechos con las transacciones de ventas.

#### Glosario de prefijos de categoría

| Prefijo | Significado       | Ejemplo(s)                            |
|---------|-------------------|---------------------------------------|
| `dim_`  | Tabla de dimensión| `dim_customers`, `dim_products`       |
| `fact_` | Tabla de hechos   | `fact_sales`                          |
| `agg_`  | Tabla agregada    | `agg_customers`, `agg_sales_monthly`  |

## Convenciones para columnas

### Llaves sustitutas (surrogate keys)

- Todas las llaves primarias de las tablas de dimensión deben usar el sufijo `_key`.
- **`<nombre_tabla>_key`**
  - Ejemplo: `customer_key` → llave sustituta de la tabla `dim_customers`.

### Columnas técnicas

- Todas las columnas técnicas deben empezar con el prefijo `dwh_`, seguido de un nombre descriptivo que indique su propósito.
- **`dwh_<nombre_columna>`**
  - Ejemplo: `dwh_load_date` → columna generada por el sistema que guarda la fecha en que se cargó el registro.

## Procedimientos almacenados

- Todos los procedimientos almacenados que cargan datos deben seguir el patrón **`load_<capa>`**.
  - `<capa>`: la capa que se carga, como `bronze`, `silver` o `gold`.
  - Ejemplos:
    - `load_bronze` → procedimiento almacenado que carga la capa Bronze.
    - `load_silver` → procedimiento almacenado que carga la capa Silver.
