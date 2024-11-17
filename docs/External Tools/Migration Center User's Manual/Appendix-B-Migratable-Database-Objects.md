# Appendix B: Migratable Database Objects

This section will provide guidlines and explanation in regards to the migratable database objects depending on the "build" step.

Objects in the source database that Migration Center does not migrate automatically must be converted manually by the user. Starting with Migration Center 7.11, the CREATE statement of an object is written to the two files below in the Build phase, so users can refer to these files for conversion.

- SrcDbObj_Create.sql
- BuildReport4Unsupported.html

### Altibase to Altibase

| Database Object Type   | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :--------------------- | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                  |             O              |              O              | The comments specified in the bales and columns are migrated as well. |
| Primary Key Constraint |             O              |              O              |                                                              |
| Unique Constraint      |             O              |              O              |                                                              |
| Check Constraint       |             O              |              O              |                                                              |
| Foreign Key Constraint |             O              |              O              |                                                              |
| Index                  |             O              |              O              |                                                              |
| Sequence               |             O              |              X              |                                                              |
| Queue                  |             O              |              X              |                                                              |
| Private Synonym        |         Partly yes         |              X              | Synonyms that refer to objects in other schemas are also migrated |
| Procedure              |         Partly yes         |              X              | The original DDL is performed without any additional conversion. |
| Function               |         Partly yes         |              X              | The original DDL is performed without any additional conversion. |
| Package                |         Partly yes         |              X              | The original DDL is performed without any additional conversion. |
| View                   |         Partly yes         |              X              | The original DDL is performed without any additional conversion. |
| Materialized View      |         Partly yes         |              X              | The original DDL is performed without any additional conversion. |
| Trigger                |         Partly yes         |              X              | The original DDL is performed without any additional conversion. |

### Altibase to Oracle

| Database Object Type   | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :--------------------- | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                  |             O              |              O              |                                                              |
| Primary Key Constraint |             O              |              O              |                                                              |
| Unique Constraint      |             O              |              O              |                                                              |
| Check Constraint       |             O              |              O              |                                                              |
| Foreign Key Constraint |             O              |              O              |                                                              |
| Index                  |             O              |              O              |                                                              |
| Sequence               |             O              |              X              |                                                              |
| Queue                  |             X              |              X              | It is automatically excluded since from the build phase since there are no convertible objects |
| Private Synonym        |         Partly Yes         |              X              | Synonyms that refer to objects in other schemas are also migrated. |
| Procedure              |         Partly Yes         |              X              | The original DDL is performed without any additional conversion. |
| Function               |         Partly Yes         |              X              | The original DDL is performed without any additional conversion. |
| Package                |         Partly Yes         |              X              | The original DDL is performed without any additional conversion. |
| View                   |         Partly Yes         |              X              | The original DDL is performed without any additional conversion. |
| Materialized View      |         Partly Yes         |              X              | The original DDL is performed without any additional conversion. Note that the primary key must exist in the base table for migration |
| Trigger                |         Partly Yes         |              X              | The original DDL is performed without any additional conversion. |

### CUBRID to Altibase

| Database Object Type            | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :------------------------------ | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                           |             O              |              O              | The comments specified in tables and columns are migrated as well |
| Primary Key Constraint          |             O              |              O              |                                                              |
| Unique Constraint               |             O              |              O              |                                                              |
| Foreign Key Constraint          |             O              |              O              |                                                              |
| Index                           |             O              |              O              | Reverse index and prefix length index of CUBRID are not supported in Altibase. Since the reverse index takes a method of reversely inserting key values when creating an index, it is not supported by Altibase migration whereas prefix length index takes a method of partially indexing specific key values and so, it is converted as a general index in Altibase when migrating. |
| auto_increment Column Attribute |             O              |              O              | it is migrated to the sequence.                              |
| Serial                          |             O              |              X              | It is migrated to the sequence.                              |
| Procedure                       |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Function                        |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| View                            |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Trigger                         |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |

### Informix to Altibase

| Database Object Type   | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :--------------------- | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                  |             O              |              O              | The comments specified in tables and columns are migrated as well. |
| Primary Key Constraint |             O              |              O              |                                                              |
| Unique Constraint      |             O              |              O              |                                                              |
| Check Constraint       |             O              |              O              |                                                              |
| Foreign Key Constraint |             O              |              O              |                                                              |
| Index                  |             O              |              O              |                                                              |
| Serial Column Type     |             O              |              O              | It is migrated to the sequence.                              |
| Sequence               |             O              |              X              |                                                              |
| Private Synonym        |         Partly yes         |              X              | Only synonyms that refer to objects in the same schema are migrated. |
| Procedure              |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Function               |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| View                   |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Trigger                |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |

### MySQL to Altibase

| Database Object Type            | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :------------------------------ | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                           |             O              |              O              | The comments specified in tables and columns are migrated as well. |
| Primary Key Constraint          |             O              |              O              |                                                              |
| Unique Constraint               |             O              |              O              |                                                              |
| Check Constraint                |             O              |              O              |                                                              |
| Foreign Key Constraint          |             O              |              O              |                                                              |
| Index                           |             O              |              O              |                                                              |
| auto_increment Column Attribute |             O              |              O              | It is migrated to the sequence.                              |
| Procedure                       |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Function                        |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| View                            |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Trigger                         |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |

### Oracle to Altibase

| Database Object Type   | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :--------------------- | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                  |             O              |              O              | To migrate a temporary table from an Oracle database (source database) to Altibase(destination database), a volatile tablespace is required in Altibase. This is because an Altibase temporary table can only be created in a volatile tablespace. Also, the comments specified in tables and columns are migrated as well. |
| Primary Key Constraint |             O              |              O              |                                                              |
| Unique Constraint      |             O              |              O              |                                                              |
| Check Constraint       |             O              |              O              |                                                              |
| Foreign Key Constraint |             O              |              O              |                                                              |
| Index                  |             O              |              O              |                                                              |
| Sequence               |             O              |              X              |                                                              |
| Private Synonym        |         Partly yes         |              X              | Only snonyms that refer to objects in the same schema are migrated. |
| Procedure              |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| Function               |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| Package                |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| View                   |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| Materialized View      |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| Trigger                |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |

### SQL Server to Altibase

| Database Object Type      | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :------------------------ | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                     |             O              |              O              | The comments specified in tables and columns are migrated as well. |
| Primary Key Constraint    |             O              |              O              |                                                              |
| Unique Constraint         |             O              |              O              |                                                              |
| Check Constraint          |             O              |              O              |                                                              |
| Foreign Key Constraint    |             O              |              O              |                                                              |
| Index                     |             O              |              O              |                                                              |
| Identity Column Attribute |             O              |              O              | It is migrated to the sequence                               |
| Sequence                  |             O              |              X              | SQL Server 2012 Support                                      |
| Private Synonym           |         Partly yes         |              X              | Only synonyms that refer to objects in the same schema are migrated. |
| Procedure                 |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Function                  |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| View                      |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Trigger                   |             X              |              X              | In the build phase, the object creation statements collected from the source database are recorded in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |

### TimesTen to Altibase

| Database Object Type   | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :--------------------- | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                  |             O              |              O              | To migrate a temporary table from an TimesTen database (source database) to Altibase(destination database), a volatile tablespace is required in Altibase. This is because an Altibase temporary table can only be created in a volatile tablespace.<br/>Also, the comments specified in tables and columns are migrated as well. |
| Primary Key Constraint |             O              |              O              |                                                              |
| Unique Constraint      |             O              |              O              |                                                              |
| Foreign Key Constraint |             O              |              O              |                                                              |
| Index                  |             O              |              O              | The index ordering(ASC/DESC) or size of TimesTen(the original database) is not provided. Thus, the index ordering is set as the default value(ASC), and the size is not displayed when migrating. Among the three indexes(hash, range, bitmap) provided by Times ten, the hash•range index is converted into B-tree index of Altibase and the bitmap index is skipped when migrating. Furthermore, if the indexed column has unique constraints or is the primary key, the target index migration fails since Altibase does not allow creating an index on those constrained column(s). |
| Sequence               |             O              |              X              |                                                              |
| Private Synonym        |         Partly yes         |              X              | Only synonyms that refer to objects in the same schema are migrated. |
| Procedure              |         Partly yes         |              X              | TimesTen 11.2 support                                        |
| Function               |         Partly yes         |              X              | TimesTen 11.2 support                                        |
| Package                |         Partly yes         |              X              | TimesTen 11.2 support                                        |
| View                   |         Partly yes         |              X              | TimesTen 11.2 support                                        |
| Materialized View      |         Partly yes         |              X              | TimesTen 11.2 support                                        |
| Trigger                |         Partly yes         |              X              | TimesTen 11.2 support                                        |

### Tibero to Altibase

| Database Object Type   | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :--------------------- | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                  |             O              |              O              | To migrate a temporary table from an Tibero database (source database) to Altibase(destination database), a volatile tablespace is required in Altibase. This is because an Altibase temporary table can only be created in a volatile tablespace. <br/>Also, the comments specified in tables and columns are migrated as well. |
| Primary Key Constraint |             O              |              O              |                                                              |
| Unique Constraint      |             O              |              O              |                                                              |
| Check Constraint       |             O              |              O              |                                                              |
| Foreign Key Constraint |             O              |              O              |                                                              |
| Index                  |             O              |              O              | Indexes created automatically in Tibero's LOB type column are not supported by Altibase and can not be migrated. A list of nonmigratable indexes that are filtered during the build step can be found in the Missing tab of the Build Report. |
| Sequence               |             O              |              X              |                                                              |
| Private Synonym        |         Partly yes         |              X              | Only synonyms that refer to objects in the same schema are migrated. |
| Procedure              |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| Function               |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| Package                |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| View                   |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| Materialized View      |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |
| Trigger                |         Partly yes         |              X              | Converts object creation statements according to the rules defined in the PSM converter and attempts migration. |

> Note:  MigrationCenter uses the SQL parser for Oracle provided by Third Party to migrate Tibero's Procedure, Function, View, Materialized View, and Trigger objects. Therefore, objects created with Tibero native syntax that is incompatible with Oracle grammar can cause parsing errors during conversion. In this case, the user must manually translate the syntax.

### PostgreSQL to Altibase

The following table describes supported database objects, precautions, and unsupported objects when migrating from PostgreSQL to Altibase.

| Database Object Type   | Migratable in 'Build User' | Migratable in 'Build Table' | Remarks                                                      |
| :--------------------- | :------------------------: | :-------------------------: | :----------------------------------------------------------- |
| Table                  |             O              |              O              | the comments specified in columns are migrated. <br />Since the maximum number of columns that can be created in a table is 1,600 for PostgreSQL, and 1,024 for Altibase, must be careful when performing migration. |
| Primary Key Constraint |             O              |              O              |                                                              |
| Unique Constraint      |             O              |              O              |                                                              |
| Check Constraint       |             O              |              O              |                                                              |
| Foreign Key Constraint |             O              |              O              | CASCADE, NO ACTION, and SET NULL options are migrated by using the same options both source and destination databases.<br />When migrating, the RESTRICT option is deleted. This is because the operation of the RESTRICT option is the same as when there is no foreign key option in Altibase.<br />Since the SET DEFAULT option is not supported by Altibase, it is converted to SET NULL during migration. |
| Index                  |             O              |              O              | Among the various index types of PostgreSQL, only B-tree and R-tree supported by Altibase are subject to migration. |
| Sequence               |             O              |              X              | The default maximum value of 9223372036854775807 in the PostgreSQL sequence is coerced to the default maximum value of 9223372036854775806 in the Altibase sequence.<br/>If the cache size of the PostgreSQL sequence is 1, Altibase deletes the CACHE clause and creates it with Altibase's default cache size of 20.<br /><br />Sequences explicitly created by the user in 'Build Table' are excluded from the migration target, but sequences that be created for Serial data type of the migration target table column are migrated along with the table. |
| Function               |             X              |              X              | Migration is not supported. Record the object creation statements collected from PostgreSQL at the Build stage in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| View                   |             X              |              X              | Migration is not supported. Record the object creation statements collected from PostgreSQL at the Build stage in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Materialized View      |             X              |              X              | Migration is not supported. Record the object creation statements collected from PostgreSQL at the Build stage in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |
| Trigger                |             X              |              X              | Migration is not supported. Record the object creation statements collected from PostgreSQL at the Build stage in the SrcDbObj_Create.sql and BuildReport4Unsupported.html files. |

> Note: PostgreSQL objects not recorded in the above table(e.g., Exclusion constraints, Types, Enums, etc.) are excluded from the migration target because there are no objects corresponding to Altibase.

<br/>

