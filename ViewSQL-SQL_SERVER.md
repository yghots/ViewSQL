
### ViewSQL adaptado a SQL SERVER
---

#### Cómo instalar ViewSQL y importarlo a tu proyecto

Para utilizar ViewSQL en tu proyecto, sigue estos pasos:

1. Selecciona el proyecto en el que deseas utilizar ViewSQL.

2. Dentro del proyecto, crea un query o script SQL y copia el código proporcionado a continuación. Luego, ejecuta el query para importar ViewSQL a tu proyecto.

---

#### Codigo ViewSQL

```
CREATE PROCEDURE ViewTablesForeignKeys
    @database_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX);
    SET @sql = N'
    SELECT 
        s.name AS SchemaName,
        fk.name AS ForeignKeyName,
        tp.name AS ParentTable,
        cp.name AS ParentColumn,
        tr.name AS ReferencedTable,
        cr.name AS ReferencedColumn
    FROM ' + QUOTENAME(@database_name) + '.sys.foreign_keys AS fk
    INNER JOIN ' + QUOTENAME(@database_name) + '.sys.foreign_key_columns AS fkc
        ON fk.object_id = fkc.constraint_object_id
    INNER JOIN ' + QUOTENAME(@database_name) + '.sys.tables AS tp
        ON fk.parent_object_id = tp.object_id
    INNER JOIN ' + QUOTENAME(@database_name) + '.sys.columns AS cp
        ON fkc.parent_object_id = cp.object_id AND fkc.parent_column_id = cp.column_id
    INNER JOIN ' + QUOTENAME(@database_name) + '.sys.tables AS tr
        ON fkc.referenced_object_id = tr.object_id
    INNER JOIN ' + QUOTENAME(@database_name) + '.sys.columns AS cr
        ON fkc.referenced_object_id = cr.object_id AND fkc.referenced_column_id = cr.column_id
    INNER JOIN ' + QUOTENAME(@database_name) + '.sys.schemas AS s
        ON tp.schema_id = s.schema_id
    ORDER BY SchemaName, ParentTable, ForeignKeyName;';
    
    EXEC sp_executesql @sql;
END;
GO


-- Procedure to view table data
CREATE PROCEDURE viewTable @table_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX)
    SET @sql = 'SELECT * FROM ' + QUOTENAME(@table_name) + ';'
    
    EXEC sp_executesql @sql
END
GO

-- Procedure to view tables in a database
CREATE PROCEDURE viewTables @database_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX)
    SET @sql = '
    SELECT 
        TABLE_NAME
    FROM 
        INFORMATION_SCHEMA.TABLES
    WHERE 
        TABLE_TYPE = ''BASE TABLE'' AND
        TABLE_CATALOG = ''' + @database_name + ''';'
    
    EXEC sp_executesql @sql
END
GO

CREATE PROCEDURE viewTablesPrimaryKeys
    @database_name VARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX);
    SET @sql = N'
    SELECT 
        KU.TABLE_NAME, 
        KU.COLUMN_NAME
    FROM ' + QUOTENAME(@database_name) + '.INFORMATION_SCHEMA.TABLE_CONSTRAINTS AS TC
    INNER JOIN ' + QUOTENAME(@database_name) + '.INFORMATION_SCHEMA.KEY_COLUMN_USAGE AS KU
        ON TC.CONSTRAINT_NAME = KU.CONSTRAINT_NAME
    WHERE 
        TC.CONSTRAINT_TYPE = ''PRIMARY KEY'' 
        AND TC.TABLE_SCHEMA = SCHEMA_NAME();';
    
    EXEC sp_executesql @sql;
END;
GO


-- Procedure to view stored procedures
CREATE PROCEDURE viewProcedures @database_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX)
    SET @sql = '
    SELECT 
        ROUTINE_NAME
    FROM 
        INFORMATION_SCHEMA.ROUTINES
    WHERE 
        ROUTINE_TYPE = ''PROCEDURE'' AND
        ROUTINE_CATALOG = ''' + @database_name + ''';'
    
    EXEC sp_executesql @sql
END
GO

-- Procedure to view columns of a table
CREATE PROCEDURE viewTableColumns @table_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX)
    SET @sql = 'EXEC sp_columns ' + QUOTENAME(@table_name) + ';'
    
    EXEC sp_executesql @sql
END
GO

-- Procedure to view foreign key relationships involving a table
CREATE PROCEDURE viewTableRelations @table_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX)
    SET @sql = '
    SELECT 
        CONSTRAINT_NAME = fk.name, 
        TABLE_NAME = OBJECT_NAME(fkc.parent_object_id), 
        COLUMN_NAME = COL_NAME(fkc.parent_object_id, fkc.parent_column_id), 
        REFERENCED_TABLE_NAME = OBJECT_NAME(fkc.referenced_object_id), 
        REFERENCED_COLUMN_NAME = COL_NAME(fkc.referenced_object_id, fkc.referenced_column_id)
    FROM 
        sys.foreign_key_columns AS fkc
        JOIN sys.foreign_keys AS fk ON fkc.constraint_object_id = fk.object_id
    WHERE 
        OBJECT_NAME(fkc.referenced_object_id) = ''' + @table_name + ''';'
    
    EXEC sp_executesql @sql
END
GO

-- Procedure to view views in a database
CREATE PROCEDURE viewViews @database_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX)
    SET @sql = '
    SELECT 
        TABLE_NAME
    FROM 
        INFORMATION_SCHEMA.VIEWS
    WHERE 
        TABLE_CATALOG = ''' + @database_name + ''';'
    
    EXEC sp_executesql @sql
END
GO

CREATE PROCEDURE viewTableSizes
    @database_name VARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX);
    SET @sql = N'
    SELECT 
        t.NAME AS TableName,
        s.Name AS SchemaName,
        p.rows AS RowCounts,
        SUM(a.total_pages) * 8 AS TotalSpaceKB, 
        SUM(a.used_pages) * 8 AS UsedSpaceKB, 
        (SUM(a.total_pages) - SUM(a.used_pages)) * 8 AS UnusedSpaceKB
    FROM 
        ' + QUOTENAME(@database_name) + '.sys.tables t
    INNER JOIN      
        ' + QUOTENAME(@database_name) + '.sys.indexes i ON t.OBJECT_ID = i.object_id
    INNER JOIN 
        ' + QUOTENAME(@database_name) + '.sys.partitions p ON i.object_id = p.OBJECT_ID AND i.index_id = p.index_id
    INNER JOIN 
        ' + QUOTENAME(@database_name) + '.sys.allocation_units a ON p.partition_id = a.container_id
    LEFT OUTER JOIN 
        ' + QUOTENAME(@database_name) + '.sys.schemas s ON t.schema_id = s.schema_id
    WHERE 
        t.NAME NOT LIKE ''dt%'' 
        AND t.is_ms_shipped = 0
        AND i.OBJECT_ID > 255 
    GROUP BY 
        t.Name, s.Name, p.Rows
    ORDER BY 
        TotalSpaceKB DESC, t.Name;
    ';
    
    EXEC sp_executesql @sql;
END;
GO

CREATE PROCEDURE viewTriggers
    @database_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX);
    SET @sql = N'
    SELECT 
        s.name AS SchemaName,
        t.name AS TableName,
        tr.name AS TriggerName,
        tr.type_desc AS TriggerType,
        m.definition AS TriggerDefinition
    FROM ' + QUOTENAME(@database_name) + '.sys.triggers AS tr
    INNER JOIN ' + QUOTENAME(@database_name) + '.sys.tables AS t
        ON tr.parent_id = t.object_id
    INNER JOIN ' + QUOTENAME(@database_name) + '.sys.sql_modules AS m
        ON tr.object_id = m.object_id
    INNER JOIN ' + QUOTENAME(@database_name) + '.sys.schemas AS s
        ON t.schema_id = s.schema_id
    WHERE tr.is_ms_shipped = 0
    ORDER BY SchemaName, TableName, TriggerName;';
    
    EXEC sp_executesql @sql;
END;
GO

-- Procedure to view events
CREATE PROCEDURE viewEvents @database_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX);
    SET @sql = '
    SELECT 
        name AS EVENT_NAME,
        event_session_id AS EVENT_SESSION_ID,
        max_dispatch_latency AS MAX_DISPATCH_LATENCY,
        memory_partition_mode AS MEMORY_PARTITION_MODE,
        event_retention_mode AS EVENT_RETENTION_MODE,
        startup_state AS STARTUP_STATE
    FROM 
        sys.server_event_sessions
    WHERE 
        name LIKE ''' + @database_name + '%'';';
    
    EXEC sp_executesql @sql;
END;
GO



-- Procedure to view indexes of a table
CREATE PROCEDURE viewTableIndexes @table_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX)
    SET @sql = 'EXEC sp_helpindex ' + QUOTENAME(@table_name) + ';'
    
    EXEC sp_executesql @sql
END
GO

-- Procedure to view database files
CREATE PROCEDURE viewDatabaseFiles @database_name NVARCHAR(128)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX)
    SET @sql = '
    SELECT 
        FILE_ID = file_id,
        NAME,
        PHYSICAL_NAME = physical_name,
        SIZE_MB = size * 8.0 / 1024,
        MAX_SIZE_MB = CASE max_size 
                         WHEN -1 THEN ''Unlimited'' 
                         ELSE CAST(max_size * 8.0 / 1024 AS NVARCHAR(50))
                      END,
        GROWTH_MB = growth * 8.0 / 1024,
        FILE_TYPE = CASE type_desc 
                       WHEN ''ROWS'' THEN ''Data File'' 
                       ELSE ''Log File'' 
                    END
    FROM 
        sys.master_files
    WHERE 
        database_id = DB_ID(''' + @database_name + ''');'
    
    EXEC sp_executesql @sql
END
GO

-- Procedure to view physical location of database files
CREATE PROCEDURE viewDatabasePhysicalLocation
AS
BEGIN
    -- Get the location of the data directory
    SELECT 
        'Data Directory' AS [Variable],
        SERVERPROPERTY('DataRoot') AS [Value]

    UNION ALL
    
    -- Get the location of various log and config files
    SELECT 
        'Log File' AS [Variable],
        physical_name AS [Value]
    FROM 
        sys.master_files
    WHERE 
        type_desc = 'LOG'

    UNION ALL

    SELECT 
        'Error Log' AS [Variable],
        SERVERPROPERTY('ErrorLogFileName') AS [Value]

    UNION ALL

    SELECT 
        'Default Data' AS [Variable],
        SERVERPROPERTY('InstanceDefaultDataPath') AS [Value]

    UNION ALL

    SELECT 
        'Default Log' AS [Variable],
        SERVERPROPERTY('InstanceDefaultLogPath') AS [Value];
END
GO
 ```
