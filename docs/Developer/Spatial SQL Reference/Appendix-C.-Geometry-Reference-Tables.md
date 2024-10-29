# Appendix C. Geometry Reference Tables

This Appendix discusses how to install and use the SPATIAL_REF_SYS and GEOMETRY_COLUMNS meta tables, which satisfy the OGC standard, and additionally outlines the limitations related to their use.

### Geometry Reference Tables 

The geometry reference tables are used to manage Spatial Reference IDs (SRIDs) and the Spatial Reference System (SRS).

#### Limitation

- The GEOMETRY_COLUMNS and SYSTEM_.SYS_GEOMETRIES tables are generic meta tables and are used only for reference.

#### GEOMETRY_COLUMNS

The GEOMETRY_COLUMNS meta table is used to define and manage SRIDs (Spatial Reference IDs) for GEOMETRY type columns.

This table is a synonym of the SYSTEM_.SYS_GEOMETRY_COLUMNS\_ meta table.

| Column name     | Type         | Description          |
|-----------------|--------------|----------------------|
| F_TABLE_SCHEMA    | VARCHAR(128) | This is the name of the owner of the table. |
| F_TABLE_NAME      | VARCHAR(128) | This is the name of the table. |
| F_GEOMETRY_COLUMN | VARCHAR(128) | This is the name of the GEOMETRY type column. |
| COORD_DIMENSION | INTERGER     | This is the number of dimensions of the GEOMETRY type object stored in the GEOMETRY type column. |
| SRID            | INTERGER     | This is the Spatial Reference Identifier. |

#### SPATIAL_REF_SYS

The SPATIAL_REF_SYS meta table is used to manage information about Spatial Reference Identifiers (SRIDs) and the corresponding Spatial Reference System (SRS).

This table is a synonym of the SYSTEM\_.USER_SRS meta table.

To read Spatial Reference System meta data into SPATIAL_REF_SYS table, the ADD_SPATIAL_REF_SYS and DELETE_SPATIAL_REF_SYS procedures must be used.

| Column name | Type           | Description                                          |
|-------------|----------------|------------------------------------------------------|
| SRID        | INTEGER        | This is an internally used Spatial Reference Identifier. |
| AUTH_NAME   | VARCHAR(256)    | This is a standard name.                     |
| AUTH_SRID   | INTERGER       | This is a standard Spatial Reference Identifier. |
| SRTEXT      | VARCHAR (2048) | This is a description of the Spatial Reference System in OGC-WKT form. |
| PROJ4TEXT   | VARCHAR (2048) | This is information for use with PROJ4. |

### Related Stored Procedures		

#### ADD_SPATIAL_REF_SYS

##### Syntax

```
SYS_SPATIAL.ADD_SPATIAL_REF_SYS( SRID in integer,
                                 AUTH_NAME in varchar(256),
                                 AUTH_SRID in integer,
                                 SRTEXT in varchar(2048),
                                 PROJ4TEXT in varchar(2048) );
```

##### Description	

This registers Spatial Reference System meta data in the SPATIAL_REF_SYS_BASE table.

##### Parameters

| Name      | I/O | Data Type | Description          |
|---------------|-------------|-------------------|--------|
| SRID      | IN | INTEGER      | This is an ID in the database of the Spatial Reference System. |
| AUTH_NAME  | IN| VARCHAR(256) | This is the name of the standard used in the Spatial Reference System. |
| AUTH_SRID | IN | INTEGER    | This is an ID of the Spatial Reference System as defined by the standard. |
| SRTEXT          | IN     | VARCHAR(2048)  | This is Well-Known Text expression of Spatial Reference System. |
| PROJ4TEXT          | IN    |VARCHAR(2048) | This is information used for PROJ4. |

##### Return Value

Because this is a stored procedure, no result is returned.

##### Exception

No exception


#### DELETE_SPATIAL_REF_SYS

##### Syntax

```
SYS_SPATIAL.DELETE_SPATIAL_REF_SYS( SRID in integer,
                                    AUTH_NAME in varchar(256) );
```

##### Description

This procedure deletes the meta table of the geometry column registered in the GEOMETRY_COLUMNS_BASE table.

##### Parameters

| Name      | I / O | Data Type | Description          |
|---------------|-------------|-------------------|--------|
| SRID      | IN | INTEGER      | This is an ID in the database of the Spatial Reference System. |
| AUTH_NAME  | IN| VARCHAR(256) | This is a name of the standard used in the Spatial Reference System. |

##### Return Type

Because this is a stored procedure, no result is returned.

##### Exceptiong	

No exception
