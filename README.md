# SSIS_ETL_Supermarket
# SSIS ETL: NA Supermarket to CL Data Warehouse

![Visual Studio](https://img.shields.io/badge/Visual_Studio-5C2D91?style=for-the-badge&logo=visual-studio&logoColor=white)
![SSIS](https://img.shields.io/badge/SSIS-Integration_Services-CC292B?style=for-the-badge&logo=microsoft-sql-server)
![SQL Server](https://img.shields.io/badge/SQL_Server-T--SQL-00ACD7?style=for-the-badge&logo=microsoft-sql-server)

## Resumen Técnico
Proyecto de integración de datos desarrollado en **Visual Studio con SQL Server Integration Services (SSIS)**. El pipeline extrae datos transaccionales (OLTP) de un origen norteamericano, aplica transformaciones de localización y los carga en un Data Warehouse centralizado estructurado bajo un **esquema Snowflake**.

## Arquitectura del Pipeline (SSIS)

El flujo está construido mediante paquetes `.dtsx` divididos en las siguientes etapas operativas:

* **Extracción:** Conexión mediante OLE DB/ADO.NET al origen transaccional. Volcado de datos crudos a una base de datos de *Staging*.
* **Transformación (Data Flow Tasks):**
  * **Data Conversion & Derived Columns:** Casting de tipos de datos, estandarización de fechas y cálculos métricos.
  * **Localización (US -> CL):** Transformación de divisa (USD a CLP), conversión de sistema imperial a métrico y ajuste de tasas de impuestos locales (IVA).
  * **Lookups & Merges:** Cruce de datos para la normalización de dimensiones jerárquicas.
* **Carga:** Mapeo de destinos mediante *OLE DB Destination* hacia las Dimensiones (SCD) y Tablas de Hechos del esquema Snowflake objetivo. Manejo de restricciones de clave foránea.

## Modelo de Datos (Target)
El Data Warehouse destino implementa un **esquema Snowflake (Copo de Nieve)** para optimizar el almacenamiento y normalizar jerarquías complejas (ej. `Dim_Categoria -> Dim_Subcategoria -> Dim_Producto`).

## Estructura de la Solución (Visual Studio)

* `\SSIS_ETL_SUPERMARKET` -> Proyecto principal de Integration Services.
  * `\Package.dtsx` -> Paquetes de ejecución del flujo de control y flujo de datos.
  * `\Project.params` -> Parámetros de configuración del proyecto.
  * `\Connection Managers` -> Administradores de conexión compartidos (Origen y Destino).
* `\SQL_Scripts` -> Scripts T-SQL (DDL) para la creación del área de Staging y el modelo Snowflake.

## Despliegue
Para abrir y ejecutar esta solución se requiere **Visual Studio** con la carga de trabajo de *Data Storage and Processing* y la extensión de **SQL Server Integration Services Projects** instalada.