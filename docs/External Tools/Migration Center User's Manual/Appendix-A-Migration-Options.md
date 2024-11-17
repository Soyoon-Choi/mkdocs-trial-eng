# Appendix A: Migration Options

Migration Options affect the migration project. They can be edited by selecting the “Migration Option” menu item from the Migration menu in GUI mode. Migration Options can usually be edited right after the project is created. 

The primary option is the Migration Type: either “DB to DB” or “DB to File”. 

- DB to DB Migration Options

- DB to File Migration Options

### DB to DB Migration Options

Object and table data of the source database to be migrated are migrated directly to the database (Altibase) to be stored.

| Name                                         | Description                                                  |
| -------------------------------------------- | ------------------------------------------------------------ |
| Execution Thread                             | Specifies the maximum number of threads to be used when executing data migration. The default value is triple of the number of logical processors in the system running the Migration Center. The suggested range of this value is from 1 to triple of the number of logical processors in the system running the Migration Center. |
| Migration Target                             | Specifies the targets for migration: <br />\- Object & Data: Database objects and table data<br />\- Object: Database objects only |
| **Object Options**                           |                                                              |
| Foreign Key Migration                        | Specifies whether or not to include foreign key constraints in migration target. This option is set to 'No' by default. |
| PSM Migration                                | Specifies whether or not to include PSM objects such as procedures, functions, materialized views, views, typesets, and triggers in migration target. This option is set to 'No' by default.<br /> |
| Drop Existing Objects                        | Specifies whether or not to recreate the database object before migration. <br />When this option is set to 'Yes', DROP and CREATE will be executed on migration target objects in target database. When this option is set to 'No', migration will be executed without dropping the database objects. This option is set to 'No' by default. |
| Keep Partition Table                         | Specifies whether or not to maintain partitioned tables.<br />When this option is set to 'Yes', partitioned tables in the the source database will be migrated as partitioned tables. In this case, additional work is required for the partitioned tables in the reconcile stage's '5. Partitioned Table Conversion'. When this option is set to 'No', target partitioned tables will be migrated as non-partitioned tables. This option is set to 'No' by default. |
| Use Double-quoted Identifier                 | Specifies whether or not to use double quotation marks for schema and object names. This option is set to 'No' by default. |
| Remove FORCE from View DDL                   | Specifies whether or not to remove 'FORCE' keyword from the statement creating a view. |
| Postfix for reserved word                    | Specifies a user-defined word which is to be added to the database object name in the source database as a postfix when it conflicts with a reserved keyword of Altibase. The defualt value is _POC. |
| **Data Options**                             |                                                              |
| Batch Execution                              | Specifies whether or not to use batch insert in JDBC for higher performance. This option is set to 'Yes' by default. |
| Batch Size                                   | Specifies the batch size when batch insert in JDBC is used. The default value is 10,000. |
| Batch LOB type                               | Specifies whether or not to batch process BLOB and CLOB data types.<br />'Yes' means to allow batch processing. However, it should be noted that problems such as out of memory (OOM) may occur depending on the size of the LOB data. Also, an exception may be raised in TimesTen, which does not support the batch processing for LOB data types.'No' does not allow batch processing. This option is set to 'No' by default. |
| Log Insert-failed Data                       | Specifies whether or not to log insert-failed rows during data migration. This option is available only when the Batch Execution option is disabled. This option is set to 'No' by default. |
| File Encoding                                | Specifies the encoding character set to be used when logging the insert-failed data into files. This option is available only when the Log Insert-failed Data option is enabled. The default value is UTF8. |
| **Data Validation Options**                  |                                                              |
| Operation                                    | Specifies the operation to be executed in the data validation stage: <br />DIFF: Check data difference between the source and the target databases. <br/>FILESYNC: Apply the CSV file created as a result of DIFF operation to the target database. |
| Write to CSV                                 | Specifies whether or not to write the inconsistent data to the CSV file. |
| Include LOB                                  | Specifies whether or not to include LOB data when writing inconsistent data to the CSV file. |
| Data Sampling                                | Specifies whether or not to use data sampling feature.<br />When this option is set to 'Yes', to decrease the data validation time, only sample data will be validated. When this option is set to 'No', each data will be validated. This option is set to 'Yes' by default. |
| Percent Sampling (exact counting)            | Specifies the percentage of data to be sampled from target tables. This option is used only when the Exact Counting Method is selected in the build stage. |
| Record Count Sampling (approximate counting) | Specifies the number of records to be sampled from target tables. This option is used only if the Approximate Counting Method is selected in the build stage. |

### DB to File Migration Options

Object and table data of the source database to be migrated are stored as SQL script file, form file, and CSV type data file, respectively.

The stored files can be migrated to the database (Altibase) to be saved using iSQL and iLoader.

| Name                         | Description                                                  |
| ---------------------------- | ------------------------------------------------------------ |
| Execution Thread             | Specifies the maximum number of multi thread to be used when executing data migration. Default value is triple of the amount logical processors in the system running the Migration Center. The suggested range of this value is from 1 to triple of the amount logical processors in the system running the Migration Center. |
| Migration Target             | Specifies the targets for data migration: <br />\- Object & Data: Database objects and table data<br />\- Object: Database objects only |
| **Object Options**           |                                                              |
| Foreign Key Migration        | Specifies whether or not to include foreign key constraints in migration target. This option is set to 'No' by default. |
| PSM Migration                | Specifies whether or not to include  PSM objects such as procedures, functions, materialized views, views, typesets, and triggers. This option is set to 'Yes' by default. |
| Keep Partition Table         | Specifies whether or not to maintain partitioned tables.<br />When this option is set to 'Yes', partitioned tables in the the source database will be migrated as partitioned tables. In this case, additional work is required for the partitioned tables in the reconcile stage's '5. Partitioned Table Conversion'. When this option is set to 'No', target partitioned tables will be migrated as non-partitioned tables. This option is set to 'No' by default. |
| Use Double-quoted Identifier | Specifies whether or not to use double quotation marks for schema and object names. This option is set to 'No' by default. |
| **Data Files**               |                                                              |
| File Encoding                | Specifies the encoding character set to be used for scripts and data files. |

<br/>

