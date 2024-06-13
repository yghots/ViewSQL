![loli](https://miro.medium.com/v2/resize:fit:1200/1*9h9YpiykxheK8j8qh4gM4w.png)

# Documentación de Librería de Procedimientos Almacenados ViewSQL

## Visión General
Esta librería contiene una colección de procedimientos almacenados diseñados para facilitar varias tareas de gestión de bases de datos. Cada procedimiento sirve un propósito específico, que van desde ver estructuras de tablas hasta obtener ubicaciones físicas de archivos de la base de datos. A continuación, se describe en detalle cada procedimiento incluido en la librería junto con ejemplos de uso.

---

## Procedimientos Almacenados y Ejemplos

1. **ViewTablesForancesKeys**

   - *Descripción*: Recupera información sobre las claves foráneas en una base de datos especificada.
   - *Parámetros*: `IN database_name VARCHAR(128)`: El nombre de la base de datos a consultar.
   - *Salida*: Lista los nombres de las tablas, nombres de las columnas, nombres de las restricciones, nombres de las tablas referenciadas y una bandera que indica si una tabla usa una tabla referenciada.
   - *Ejemplo*:
     ```
     CALL ViewTablesForancesKeys('mi_base_de_datos');
     ```
   - *Explicación*: Este comando llama al procedimiento ViewTablesForancesKeys y pasa el nombre de la base de datos 'mi_base_de_datos' como parámetro. El resultado será una lista de todas las claves foráneas en la base de datos especificada.

---

2. **viewTable**

   - *Descripción*: Selecciona todas las filas de una tabla especificada.
   - *Parámetros*: `IN table_name VARCHAR(128)`: El nombre de la tabla a consultar.
   - *Salida*: Todas las filas y columnas de la tabla especificada.
   - *Ejemplo*:
     ```
     CALL viewTable('mi_tabla');
     ```
   - *Explicación*: Este comando llama al procedimiento viewTable y pasa el nombre de la tabla 'mi_tabla' como parámetro. El resultado será todas las filas y columnas de la tabla especificada.

---

3. **viewTables**

   - *Descripción*: Lista todas las tablas base en una base de datos especificada.
   - *Parámetros*: `IN database_name VARCHAR(128)`: El nombre de la base de datos a consultar.
   - *Salida*: Nombres de todas las tablas base en la base de datos especificada.
   - *Ejemplo*:
     ```
     CALL viewTables('mi_base_de_datos');
     ```
   - *Explicación*: Este comando llama al procedimiento viewTables y pasa el nombre de la base de datos 'mi_base_de_datos' como parámetro. El resultado será una lista de todas las tablas base en la base de datos especificada.

---

4. **viewTablesPrimaryKeys**

   - *Descripción*: Recupera información sobre las claves primarias en una base de datos especificada.
   - *Parámetros*: `IN database_name VARCHAR(128)`: El nombre de la base de datos a consultar.
   - *Salida*: Lista los nombres de las tablas y los nombres de las columnas que tienen claves primarias.
   - *Ejemplo*:
     ```
     CALL viewTablesPrimaryKeys('mi_base_de_datos');
     ```
   - *Explicación*: Este comando llama al procedimiento viewTablesPrimaryKeys y pasa el nombre de la base de datos 'mi_base_de_datos' como parámetro. El resultado será una lista de todas las tablas y sus columnas que tienen claves primarias en la base de datos especificada.

---

5. **viewProcedures**

   - *Descripción*: Lista todos los procedimientos almacenados en una base de datos especificada.
   - *Parámetros*: `IN database_name VARCHAR(128)`: El nombre de la base de datos a consultar.
   - *Salida*: Nombres de todos los procedimientos almacenados en la base de datos especificada.
   - *Ejemplo*:
     ```
     CALL viewProcedures('mi_base_de_datos');
     ```
   - *Explicación*: Este comando llama al procedimiento viewProcedures y pasa el nombre de la base de datos 'mi_base_de_datos' como parámetro. El resultado será una lista de todos los procedimientos almacenados en la base de datos especificada.

---

6. **viewTableColumns**

   - *Descripción*: Describe las columnas de una tabla especificada.
   - *Parámetros*: `IN table_name VARCHAR(128)`: El nombre de la tabla a describir.
   - *Salida*: Descripción de las columnas incluyendo nombre del campo, tipo, capacidad de ser nulo, clave, valor por defecto e información extra.
   - *Ejemplo*:
     ```
     CALL viewTableColumns('mi_tabla');
     ```
   - *Explicación*: Este comando llama al procedimiento viewTableColumns y pasa el nombre de la tabla 'mi_tabla' como parámetro. El resultado será una descripción detallada de las columnas de la tabla especificada.

---

7. **viewTableRelations**

   - *Descripción*: Recupera información sobre las relaciones de claves foráneas donde la tabla especificada es la tabla referenciada.
   - *Parámetros*: `IN table_name VARCHAR(128)`: El nombre de la tabla a consultar.
   - *Salida*: Lista los nombres de las restricciones, nombres de las tablas, nombres de las columnas, nombres de las tablas referenciadas y nombres de las columnas referenciadas.
   - *Ejemplo*:
     ```
     CALL viewTableRelations('mi_tabla');
     ```
   - *Explicación*: Este comando llama al procedimiento viewTableRelations y pasa el nombre de la tabla 'mi_tabla' como parámetro. El resultado será una lista de todas las relaciones de claves foráneas donde la tabla especificada es la tabla referenciada.

---

8. **viewViews**

   - *Descripción*: Lista todas las vistas en una base de datos especificada.
   - *Parámetros*: `IN database_name VARCHAR(128)`: El nombre de la base de datos a consultar.
   - *Salida*: Nombres de todas las vistas en la base de datos especificada.
   - *Ejemplo*:
     ```
     CALL viewViews('mi_base_de_datos');
     ```
   - *Explicación*: Este comando llama al procedimiento viewViews y pasa el nombre de la base de datos 'mi_base_de_datos' como parámetro. El resultado será una lista de todas las vistas en la base de datos especificada.

---

9. **viewTableSizes**

   - *Descripción*: Recupera los tamaños de todas las tablas en una base de datos especificada.
   - *Parámetros*: `IN database_name VARCHAR(128)`: El nombre de la base de datos a consultar.
   - *Salida*: Lista los nombres de las tablas y sus tamaños en megabytes (MB).
   - *Ejemplo*:
     ```
     CALL viewTableSizes('mi_base_de_datos');
     ```
   - *Explicación*: Este comando llama al procedimiento viewTableSizes y pasa el nombre de la base de datos 'mi_base_de_datos' como parámetro. El resultado será una lista de todas las tablas en la base de datos especificada junto con sus tamaños en megabytes.

---

10. **viewTriggers**

   - *Descripción*: Lista todos los triggers en una base de datos especificada.
   - *Parámetros*: `IN database_name VARCHAR(128)`: El nombre de la base de datos a consultar.
   - *Salida*: Lista los nombres de los triggers, manipulaciones de eventos, tablas de objetos de eventos y declaraciones de acción.
   - *Ejemplo*:
     ```
     CALL viewTriggers('mi_base_de_datos');
     ```
   - *Explicación*: Este comando llama al procedimiento viewTriggers y pasa el nombre de la base de datos 'mi_base_de_datos' como parámetro. El resultado será una lista de todos los triggers en la base de datos especificada, incluyendo detalles sobre sus eventos y acciones.

---

11. **viewEvents**

   - *Descripción*: Lista todos los eventos en una base de datos especificada.
   - *Parámetros*: `IN database_name VARCHAR(128)`: El nombre de la base de datos a consultar.
   - *Salida*: Lista los nombres de los eventos, tipos de eventos, tiempos de ejecución, valores de intervalo, campos de intervalo y estados.
   - *Ejemplo*:
     ```
     CALL viewEvents('mi_base_de_datos');
     ```
   - *Explicación*: Este comando llama al procedimiento viewEvents y pasa el nombre de la base de datos 'mi_base_de_datos' como parámetro. El resultado será una lista de todos los eventos en la base de datos especificada, incluyendo detalles sobre sus tiempos de ejecución y estados.

---

12. **viewTableIndexes**

   - *Descripción*: Muestra los índices de una tabla especificada.
   - *Parámetros*: `IN table_name VARCHAR(128)`: El nombre de la tabla a consultar.
   - *Salida*: Lista todos los índices para la tabla especificada.
   - *Ejemplo*:
     ```
     CALL viewTableIndexes('mi_tabla');
     ```
   - *Explicación*: Este comando llama al procedimiento viewTableIndexes y pasa el nombre de la tabla 'mi_tabla' como parámetro. El resultado será una lista de todos los índices para la tabla especificada.

---

13. **viewDatabaseFiles**

   - *Descripción*: Recupera metadatos sobre las tablas en una base de datos especificada, incluyendo tipo de motor, número de filas, longitud de datos y más.
   - *Parámetros*: `IN database_name VARCHAR(128)`: El nombre de la base de datos a consultar.
   - *Salida*: Lista metadatos sobre las tablas incluyendo esquema, nombre de la tabla, motor, filas, longitud de datos, longitud de índice, espacio libre de datos, valor de autoincremento, tiempo de creación, tiempo de actualización, tiempo de chequeo y formato de la tabla.
   - *Ejemplo*:
     ```
     CALL viewDatabaseFiles('mi_base_de_datos');
     ```
   - *Explicación*: Este comando llama al procedimiento viewDatabaseFiles y pasa el nombre de la base de datos 'mi_base_de_datos' como parámetro. El resultado será una lista de metadatos sobre las tablas en la base de datos especificada.

---

14. **viewTableConstraints**

   - *Descripción*: Recupera información sobre las restricciones de una tabla especificada.
   - *Parámetros*: `IN table_name VARCHAR(128)`: El nombre de la tabla a consultar.
   - *Salida*: Lista los nombres de las restricciones, tipos de restricciones, nombres de las columnas y otros detalles relacionados con las restricciones.
   - *Ejemplo*:
     ```
     CALL viewTableConstraints('mi_tabla');
     ```
   - *Explicación*: Este comando llama al procedimiento viewTableConstraints y pasa el nombre de la tabla 'mi_tabla' como parámetro. El resultado será una lista de todas las restricciones de la tabla especificada, incluyendo detalles sobre sus tipos y las columnas que abarcan.

