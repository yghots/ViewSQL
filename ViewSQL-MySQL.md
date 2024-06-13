
### ViewSQL adaptado a MySQL
---

#### Cómo instalar ViewSQL y importarlo a tu proyecto

Para utilizar ViewSQL en tu proyecto, sigue estos pasos:

1. Selecciona el proyecto en el que deseas utilizar ViewSQL.

2. Dentro del proyecto, crea un query o script SQL y copia el código proporcionado a continuación. Luego, ejecuta el query para importar ViewSQL a tu proyecto.

---

#### Codigo ViewSQL

```
DELIMITER //

CREATE PROCEDURE ViewTablesForancesKeys(IN database_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('
    SELECT 
        TABLE_NAME, 
        COLUMN_NAME, 
        CONSTRAINT_NAME, 
        REFERENCED_TABLE_NAME, 
        REFERENCED_COLUMN_NAME,
        CASE 
            WHEN REFERENCED_TABLE_NAME IS NOT NULL THEN ''Sí''
            ELSE ''No''
        END AS USE_REFERENCE_TABLE
    FROM 
        INFORMATION_SCHEMA.KEY_COLUMN_USAGE
    WHERE 
        TABLE_SCHEMA = ''', database_name, ''';');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;

DELIMITER //

CREATE PROCEDURE viewTable(IN table_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('SELECT * FROM ', table_name, ';');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;

DELIMITER //

CREATE PROCEDURE viewTables(IN database_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('
    SELECT 
        TABLE_NAME
    FROM 
        INFORMATION_SCHEMA.TABLES
    WHERE 
        TABLE_TYPE = ''BASE TABLE'' AND
        TABLE_SCHEMA = ''', database_name, ''';');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;

DELIMITER //

CREATE PROCEDURE viewTablesPrimaryKeys(IN database_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('
    SELECT 
        TABLE_NAME, 
        COLUMN_NAME
    FROM 
        INFORMATION_SCHEMA.KEY_COLUMN_USAGE
    WHERE 
        CONSTRAINT_NAME = ''PRIMARY'' AND
        TABLE_SCHEMA = ''', database_name, ''';');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;


DELIMITER //

CREATE PROCEDURE viewProcedures(IN database_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('
    SELECT 
        ROUTINE_NAME
    FROM 
        INFORMATION_SCHEMA.ROUTINES
    WHERE 
        ROUTINE_TYPE = ''PROCEDURE'' AND
        ROUTINE_SCHEMA = ''', database_name, ''';');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;


DELIMITER //

CREATE PROCEDURE viewTableColumns(IN table_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('DESCRIBE ', table_name, ';');
		
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;

	DELIMITER //

	CREATE PROCEDURE viewTableRelations(IN table_name VARCHAR(128))
	BEGIN
		SET @sql = CONCAT('
		SELECT 
			CONSTRAINT_NAME, 
			TABLE_NAME, 
			COLUMN_NAME, 
			REFERENCED_TABLE_NAME, 
			REFERENCED_COLUMN_NAME
		FROM 
			INFORMATION_SCHEMA.KEY_COLUMN_USAGE
		WHERE 
			REFERENCED_TABLE_NAME = ''', table_name, ''';');
		
		PREPARE stmt FROM @sql;
		EXECUTE stmt;
		DEALLOCATE PREPARE stmt;
	END //

	DELIMITER ;


DELIMITER //

CREATE PROCEDURE viewViews(IN database_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('
    SELECT 
        TABLE_NAME
    FROM 
        INFORMATION_SCHEMA.VIEWS
    WHERE 
        TABLE_SCHEMA = ''', database_name, ''';');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;


DELIMITER //

CREATE PROCEDURE viewTableSizes(IN database_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('
    SELECT 
        table_name AS "Table",
        round(((data_length + index_length) / 1024 / 1024), 2) AS "Size (MB)"
    FROM 
        information_schema.TABLES
    WHERE 
        table_schema = ''', database_name, '''
    ORDER BY 
        (data_length + index_length) DESC;');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;


DELIMITER //

CREATE PROCEDURE viewTriggers(IN database_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('
    SELECT 
        TRIGGER_NAME,
        EVENT_MANIPULATION,
        EVENT_OBJECT_TABLE,
        ACTION_STATEMENT
    FROM 
        INFORMATION_SCHEMA.TRIGGERS
    WHERE 
        TRIGGER_SCHEMA = ''', database_name, ''';');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;

DELIMITER //

CREATE PROCEDURE viewEvents(IN database_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('
    SELECT 
        EVENT_NAME,
        EVENT_TYPE,
        EXECUTE_AT,
        INTERVAL_VALUE,
        INTERVAL_FIELD,
        STATUS
    FROM 
        INFORMATION_SCHEMA.EVENTS
    WHERE 
        EVENT_SCHEMA = ''', database_name, ''';');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;

DELIMITER //

CREATE PROCEDURE viewTableIndexes(IN table_name VARCHAR(128))
BEGIN
    SET @sql = CONCAT('SHOW INDEXES FROM ', table_name, ';');
    
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;

-- Procedimiento para obtener la ubicación de los archivos de la base de datos desde INFORMATION_SCHEMA.TABLES
DELIMITER //

CREATE PROCEDURE viewDatabaseFiles(IN database_name VARCHAR(128))
BEGIN
    SELECT 
        TABLE_SCHEMA AS 'Database',
        TABLE_NAME AS 'Table',
        ENGINE AS 'Engine',
        TABLE_ROWS AS 'Rows',
        DATA_LENGTH AS 'Data Length',
        INDEX_LENGTH AS 'Index Length',
        DATA_FREE AS 'Data Free',
        AUTO_INCREMENT AS 'Auto Increment',
        CREATE_TIME AS 'Create Time',
        UPDATE_TIME AS 'Update Time',
        TABLE_COLLATION AS 'Collation'
    FROM 
        INFORMATION_SCHEMA.TABLES
    WHERE 
        TABLE_SCHEMA = database_name;
END //

DELIMITER ;

-- Procedimiento para obtener la ubicación física de los archivos de la base de datos
DELIMITER //

CREATE PROCEDURE viewDatabasePhysicalLocation()
BEGIN
    -- Obtener la ubicación del directorio de datos del servidor MySQL
    SELECT @@datadir AS 'Data Directory';
    
    -- Obtener la ubicación de los archivos de registro y otros archivos de configuración importantes
    SELECT 
        VARIABLE_NAME AS 'Variable',
        VARIABLE_VALUE AS 'Value'
    FROM 
        INFORMATION_SCHEMA.GLOBAL_VARIABLES
    WHERE 
        VARIABLE_NAME IN ('log_bin_basename', 'log_error', 'slow_query_log_file', 'innodb_data_home_dir', 'innodb_log_group_home_dir');
END //

DELIMITER ;
 ```
