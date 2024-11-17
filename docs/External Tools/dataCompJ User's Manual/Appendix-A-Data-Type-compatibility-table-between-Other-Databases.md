# Appendix A: Data Type compatibility table between Other Databases

This section displays data type mapping table between other databases supported by dataCompJ. 

If comparison is executed on tables with unsupported data types by dataCompJ, it is processed in the following manners:

-   If both master table and slave table are in data types unsupported by dataCompJ: An error stating that the data type is unsupported is displayed in the build phase and it does not proceed to run phase.

-   If either the master table or the slave table is unsupported data type: In the build step, an error is reported to the report file (dataCompJ_report.txt) that the number of columns to be compared in the table is different, and does not proceed to run phase.

If the column data type of the table to be compared is a data type supported by dataCompJ, but the column data type of the target table for comparison is using different combination of data types to the one in data type compatibility table between other databases, it is processed in the following manners:

-   If the data comparison is impossible (e.g., comparison between integer type and character type data type): An error stating that the data type is incompatible is reported to the report file (dataCompJ_report.txt) in the build phase and it does not proceed to run phase.

-   For data types that is comparable but different to each other (e.g., integer and floating-point types): The run phase is executed since it cannot be processed in the build phase. However, in the execution result all records are treated as inconsistent records.

#### Altibase to Oracle

| Altibase | Oracle         | Description                                                  |
| -------- | -------------- | ------------------------------------------------------------ |
| SMALLINT | NUMBER         |                                                              |
| INTEGER  | NUMBER         |                                                              |
| BIGINT   | NUMBER         |                                                              |
| NUMERIC  | NUMBER         |                                                              |
| REAL     | NUMBER         |                                                              |
| DOUBLE   | NUMBER         |                                                              |
| FLOAT    | FLOAT          |                                                              |
| DATE     | DATE TIMESTAMP | - DATE - TIMESTAMP: Compared based on the smaller precision. For example, Altibase DATE supports up to microsecond, and Oracle TIMESTAMP supports up to nano second. In this case, the comparison of records is performed until nanosecond. <br>- DATE-DATE: Since Oracle DATE supports up to YYYY-MM-DD HH: MM: SS, only the according part is extracted from Altibase DATE and compared. |
| CHAR     | CHAR           |                                                              |
| VARCHAR  | VARCHAR2       |                                                              |
| NCHAR    | NCHAR          |                                                              |
| NVARCHAR | NVARCHAR2      |                                                              |

#### Altibase to MariaDB

| Altibase | MariaDB                                       | Description                                                  |
| -------- | --------------------------------------------- | ------------------------------------------------------------ |
| SMALLINT | SMALLINT                                      |                                                              |
| INTEGER  | INT                                           |                                                              |
| BIGINT   | BIGINT                                        |                                                              |
| NUMERIC  | DECIMAL                                       |                                                              |
| REAL     | FLOAT                                         |                                                              |
| DOUBLE   | DOUBLE                                        |                                                              |
| FLOAT    | N/A                                           | Data type corresponding to Altibase FLOAT does not exist in MariaDB. |
| DATE     | DATE, DATETIME, TIMESTAMP                     | - DATE-DATE: Since MariaDB DATE supports up to YYYY-MM-DD, only the according part is extracted from Altibase DATE and compared.<br>- DATE-DATETIME: Since MariaDB DATETIME supports up to YYYY-MM-DD HH: MM: SS, only the according part is extracted from Altibase DATE and compared.<br>- DATE-TIMESTAMP: Since MariaDB TIMESTAMP supports up to YYYY-MM-DD HH: MM: Microseconds 0-6 (default: 0), only the according part is extracted from Altibase DATE and compared. |
| CHAR     | CHAR                                          |                                                              |
| VARCHAR  | VARCHAR, TINYTEXT, TEXT, MEDIUMTEXT, LONGTEXT |                                                              |
| NCHAR    | CHAR with Character set                       |                                                              |
| NVARCHAR | VARCHAR with Character set                    |                                                              |

