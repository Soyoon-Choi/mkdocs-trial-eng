# Appendix C: Data Type Mapping

Migration Center’s policy for mapping data types between heterogeneous databases is to minimize the loss of data. However, the user may wish to customize the way that data is mapped, even if it incurs the loss or corruption of data. To satisfy this requirement, Migration Center also allows the user to edit the data type mapping table.

This chapter explains how to check and customize the default data type mapping table during the project progress of the Migration Center. It also explains the precautions that users need to know in the Migration Center's default data type mapping table.

### Manipulating Data Type Mapping

The user can change the data type mapping table in the reconcile stage as shown below.

**1.Reconcile**

Right-click in the project tree window and select the Reconcile menu. Or select Reconcile from the Migration menu. You must complete the Build step before you can perform this step.

<div align="left"><img src="media/MigrationCenter/datatypemapping-step-1.png"></div>

**2. Data Type Mapping**

*Reconcile* menu is clicked, the Reconcile window appears as shown below. In this window, users can view the default data type mapping table of Migration Center and change the data type of the target database in "1. Data Type Mapping". Select the data type you want to change and click the *Change* button at the bottom right.

<div align="left">
    <img src="media/MigrationCenter/datatypemapping-step-2.png">
</div>

**3. Change Mapping Type**

*Change* button is clicked, the following window appears. In the "Change Mapping Type" window, select the data type to change in the Destination DB Data Type. Depending on the data type, enter Precision and Scale if necessary, and click the *OK* button.

<div align="left">
    <img src="media/MigrationCenter/datatypemapping-step-3.png">
</div>

### Default Data Type Mapping Tables

These tables describe the basic data type mapping tables between heterogeneous databases and precautions that users should be aware of.

Since Migration Center 7.11, if a table's column length of a source database exceeds the maximum range of the data type mapped to the target database, the data type of the target database can be automatically converted to a data type with a larger range than the default mapping table. For instance, the following data types can be changed to CLOB if necessary in order to minimize data loss.

- <div align="left">
      <img src="media/MigrationCenter/datatypemapping-step-3.png">
  </div>    
- VARCHAR or VARCHAR2, LVARCHAR, TT_VARCHAR

#### Oracle to Altibase

|      | Source        | Destination     | Notice                                                       |
| :--: | :------------ | :-------------- | :----------------------------------------------------------- |
|  1   | CHAR          | CHAR            | CHAR type columns defined with character length in Oracle are automatically converted to CHAR type columns with byte length in Altibase, because in Altibase, CHAR type columns can be defined only with byte length. |
|  2   | NCHAR         | NCHAR           | The explicit sizes of the source and destination NCHAR columns are the same, e.g. NCHAR(10) -> NCHAR(10). <br/>However, in the Oracle JDBC driver, the size of a national character column is defined as the number of bytes used, whereas in the Altibase JDBC driver, the size of a national character column is defined as the number of characters that are stored. Please note that this means that the resultant column in Altibase will be two or three times as large as necessary. |
|  3   | VARCHAR2      | VARCHAR         | VARCHAR2 defined as character length in Oracle is converted into bytes in Altibase. Altibase's VARCHAR can be defined only in bytes. |
|  4   | NVARCHAR2     | NVARCHAR        | The column sizes differ, for the same reason as NCHAR.       |
|  5   | LONG          | CLOB            |                                                              |
|  6   | NUMBER        | NUMBER          | NUMBER type columns defined without precision and scale in Oracle are converted to the same NUMBER type columns without precision and scale for Altibase. *Both Oracle and Altibase internally handle NUMBER type columns defined without precision and scale as FLOAT type in the database. |
|  7   | FLOAT         | FLOAT           |                                                              |
|  8   | BINARY FLOAT  | FLOAT           |                                                              |
|  9   | BINARY DOUBLE | VARCHAR(310)    | There is no compatible data type in Altibase for the Oracle binary double type, so the data is stored in character form to prevent loss. |
|  10  | DATE          | DATE            |                                                              |
|  11  | TIMESTAMP     | DATE            | A small amount of data loss may occur due to the difference in scale. <br/>In Oracle, the scale of a timestamp value is nanoseconds (9 digits), whereas in Altibase, the scale of a timestamp value is microseconds (6 digits) |
|  12  | RAW           | BLOB            |                                                              |
|  13  | LONG RAW      | BLOB            |                                                              |
|  14  | BLOB          | BLOB            |                                                              |
|  15  | CLOB          | CLOB            |                                                              |
|  16  | NCLOB         | NVARCHAR(10666) | There is no compatible data type in Altibase for the Oracle NCLOB, so the data is stored in NVARCHAR with the maximum precision. This may cause data loss during data migration when the actual data precision exceeds the NVARCHAR maximum size. |
|  17  | ROWID         | VARCHAR(18)     | Oracle's ROWID converts to a character data type. Altibase does not support the data type ROWID. |

#### Microsoft SQL Server to Altibase

|      | Source           | Destination      | Notice                                                       |
| :--: | :--------------- | :--------------- | :----------------------------------------------------------- |
|  1   | BIGINT           | BIGINT           |                                                              |
|  2   | DECIMAL          | NUMERIC          |                                                              |
|  3   | INT              | INTEGER          |                                                              |
|  4   | NUMERIC          | NUMERIC          |                                                              |
|  5   | SMALLINT         | SMALLINT         |                                                              |
|  6   | MONEY            | FLOAT            |                                                              |
|  7   | TINYINT          | SMALLINT         |                                                              |
|  8   | SMALLINTMONEY    | FLOAT            |                                                              |
|  9   | BIT              | CHAR(1)          |                                                              |
|  10  | FLOAT            | VARCHAR(310)     | There is no compatible data type in Altibase for Microsoft SQL FLOAT type, so VARCHAR(310) is mapped to prevent data loss. |
|  11  | REAL             | FLOAT            |                                                              |
|  12  | DATE             | DATE             |                                                              |
|  13  | DATETIME2        | DATE             | A fraction of time can be loss due to difference in scale. Scale of DATETIME2 type of Microsoft SQL Server is hundreds of nanoseconds (7 digits), whereas the scale of DATE type of Altibase is only microseconds(6 digits). |
|  14  | DATETIME         | DATE             |                                                              |
|  15  | SMALLDATETIME    | DATE             |                                                              |
|  16  | CHAR             | CHAR             |                                                              |
|  17  | TEXT             | CLOB             |                                                              |
|  18  | VARCHAR          | VARCHAR          |                                                              |
|  19  | VARCHAR (MAX)    | CLOB             |                                                              |
|  20  | NVARCHAR         | NVARCHAR         |                                                              |
|  21  | NVARCHAR (MAX)   | NVARCHAR (10666) | There is no compatible data type in Altibase for SQL Server NTEXT type, NVARCHAR is used with maximum precision. It may cause data loss during data migration when actual data precision exceeds maximum NVARCHAR size. |
|  22  | BINARY           | BYTE             |                                                              |
|  23  | IMAGE            | BLOB             |                                                              |
|  24  | VARBINARY        | BLOB             |                                                              |
|  25  | ALLIDENTITY      | NUMERIC(38, 0)   |                                                              |
|  26  | UNIQUEIDENTIFIER | VARCHAR(40)      | There is no compatible data type in Altibase for SQL Server UNIQUEIDENTIFIER type, so VARCHAR type is used to prevent any data loss. |
|  27  | SYSNAME          | NVARCHAR (128)   |                                                              |

#### MySQL to Altibase

|      | Source                          | Destination    | Notice                                                       |
| :--: | :------------------------------ | :------------- | :----------------------------------------------------------- |
|  1   | TINYINT                         | SMALLINT       |                                                              |
|  2   | TINYINT UNSIGNED                | SMALLINT       |                                                              |
|  3   | SMALLINT                        | INTEGER        |                                                              |
|  4   | SMALLINT UNSIGNED               | INTEGER        |                                                              |
|  5   | MEDIUMINT                       | INTEGER        |                                                              |
|  6   | MEDIUMINT UNSIGNED              | INTEGER        |                                                              |
|  7   | INT (INTEGER)                   | INTEGER        | Please note that the minimum value of Altibase INT type (-2,147,483,647) is greater than the minimum value of MySQL INT type (- 2,147,483,648). |
|  8   | INT UNSIGNED                    | BIGINT         |                                                              |
|  9   | BIGINT                          | BIGINT         | Please note that the minimum value of Altibase BIGINTINT type (-9,223,372,036,854,775,807) is greater than the minimum value of MySQL BIGINT type (-9,223,372,036,854,775,808). |
|  10  | BIGINT UNSIGNED                 | NUMERIC(20,0)  | There is no compatible data type in Altibase for MySQL BIGINT UNSIGNED type, so NUMERIC type is used to prevent any data loss. |
|  11  | DECIMAL (NUMERIC)               | VARCHAR(70)    | There is no compatible data type in Altibase for MySQL DECIMAL type, so VARCHAR type is used to prevent any data loss. |
|  12  | FLOAT                           | FLOAT          |                                                              |
|  13  | DOUBLE                          | VARCHAR(310)   | There is no compatible data type in Altibase for MySQL DOUBLE type, so VARCHAR type is used to prevent any data loss. |
|  14  | BIT                             | VARBIT         |                                                              |
|  15  | DATETIME                        | DATE           | Time parts are set to ‘0'                                    |
|  16  | DATE                            | DATE           |                                                              |
|  17  | TIMESTAMP                       | DATE           | Except TIMEZONE                                              |
|  18  | CHAR                            | CHAR           |                                                              |
|  19  | VARCHAR                         | VARCHAR        | If the MySQL's VARCHAR legnth exceeds 32,000 bytes, the maximum size of Altibase's VARCHAR, it  is converted to Altibase's CLOB data type. This is to prevent data loss that may occur during migration due to the maximum size difference between MySQL and Altibase data types. MySQL's maximum value for VARCHAR is 65,536 bytes, larger than Altibase. |
|  20  | CHAR with National Character    |                | The data type of Altibase varies depending on the character set of MySQL and Altibase. |
|      |                                 | CHAR           | 1. If the character set of MySQL and Altibase is the same Unicode, it is converted to CHAR.<br />2. Even when MySQL's character set is not Unicode, it is converted to CHAR. |
|      |                                 | NCHAR          | If the character set of MySQL is Unicode and the character set of Altibase is not Unicode, it is converted to NCHAR. |
|  21  | VARCHAR with National Character |                | The data type of Altibase varies depending on the character set of MySQL and Altibase. |
|      |                                 | VARCHAR        | 1. If the character set of MySQL and Altibase is the same Unicode, it is converted to VARCHAR.<br />2. Even when MySQL's character set is not Unicode, it is converted to VARCHAR. |
|      |                                 | NVARCHAR       | If the character set of MySQL is Unicode and the character set of Altibase is not Unicode, it is converted to NVARCHAR. |
|  22  | BINARY                          | BYTE           |                                                              |
|  23  | VARBINARY                       | BLOB           |                                                              |
|  24  | TINYBLOB                        | BLOB           |                                                              |
|  25  | MEDIUMBLOB                      | BLOB           |                                                              |
|  26  | BLOB                            | BLOB           |                                                              |
|  27  | LONGBLOB                        | BLOB           |                                                              |
|  28  | TINYTEXT                        | VARCHAR(255)   |                                                              |
|  29  | TEXT                            | CLOB           |                                                              |
|  30  | MEDIUMTEXT                      | CLOB           |                                                              |
|  31  | LONGTEXT                        | CLOB           |                                                              |
|  32  | ENUM                            | VARCHAR(10666) | There is no compatible data type in Altibase for MySQL ENUM type, so VARCHAR is used to prevent data loss. |
|  33  | SET                             | VARCHAR(10666) | There is no compatible data type in Altibase for MySQL SET type, so VARCHAR is used to prevent data loss. |

#### Informix 11.5 to Altibase

|      | Source        | Destination | Notice                                                       |
| :--: | :------------ | :---------- | :----------------------------------------------------------- |
|  1   | BIGINT        | BIGINT      |                                                              |
|  2   | INT8          | BIGINT      |                                                              |
|  3   | INT           | INTEGER     |                                                              |
|  4   | SMALLINT      | SMALLINT    |                                                              |
|  5   | BIGSERIAL     | BIGINT      |                                                              |
|  6   | SERIAL8       | BIGINT      |                                                              |
|  7   | SERIAL        | INTEGER     |                                                              |
|  8   | FLOAT         | DOUBLE      |                                                              |
|  9   | REAL          | REAL        |                                                              |
|  10  | SMALLFLOAT    | REAL        |                                                              |
|  11  | MONEY         | NUMERIC     |                                                              |
|  12  | DECIMAL_FLOAT | FLOAT       |                                                              |
|  13  | DATE          | DATE        |                                                              |
|  14  | DATETIME      | DATE        |                                                              |
|  15  | BOOLEAN       | CHAR(1)     |                                                              |
|  16  | CHAR          | CHAR        | If Informix's CHAR legnth exceeds 32,000 bytes, the maximum size of Altibase's CHAR, it is converted to Altibase's data type to CLOB. This is to prevent data loss that may occur during migration due to the maximum size difference between Informix and Altibase data types. Informix's maximum value for CHAR is 32,767 bytes, larger than Altibase. |
|  17  | NCHAR         | NCHAR       | The user should note that data loss can occur due to the maximum precision of NCHAR data type at Informix (32,767) being greater than that of Altibase (32,000). |
|  18  | VARCHAR       | VARCHAR     |                                                              |
|  19  | NVARCHAR      | NVARCHAR    |                                                              |
|  20  | LVARCHAR      | VARCHAR     | If Informix's LVARCHAR legnth exceeds 32,000 bytes, the maximum size of Altibase's VARCHAR, it is converted to Altibase's data type to CLOB. This is to prevent data loss that may occur during migration due to the maximum size difference between Informix and Altibase data types. Informix's maximum value for LVARCHAR is 32,767 bytes, larger than Altibase. |
|  21  | TEXT          | CLOB        |                                                              |
|  22  | CLOB          | CLOB        |                                                              |
|  23  | BYTE          | BLOB        |                                                              |
|  24  | BLOB          | BLOB        |                                                              |
|  25  | INTERVAL      | FLOAT       |                                                              |

#### TimesTen to Altibase

|      | Source        | Destination     | Notice                                                       |
| :--: | :------------ | :-------------- | :----------------------------------------------------------- |
|  1   | BINARY        | BLOB            |                                                              |
|  2   | BINARY_DOUBLE | VARCHAR(310)    |                                                              |
|  3   | BINARY_FLOAT  | FLOAT           |                                                              |
|  4   | BLOB          | BLOB            |                                                              |
|  5   | CHAR          | CHAR            | CHAR type columns defined with character length in TimesTen are automatically converted to CHAR type columns with byte length in Altibase, because in Altibase, CHAR type columns can be defined only with byte length. |
|  6   | CLOB          | CLOB            |                                                              |
|  7   | DATE          | DATE            |                                                              |
|  8   | NCHAR         | NCHAR           |                                                              |
|  9   | NCLOB         | NVARCHAR(10666) |                                                              |
|  10  | NUMBER        | NUMBER          |                                                              |
|  11  | NVARCHAR2     | NVARCHAR        | The maximum size of VARCHAR2 of TimesTen is 2,097,152 bytes, which is greater than the maximum VARCHAR size of Altibase, which is 32,000; thus, the potential data loss can be occurred. |
|  12  | ROWID         | VARCHAR(18)     |                                                              |
|  13  | TIME          | DATE            |                                                              |
|  14  | TIMESTAMP     | DATE            | The maximum TIMESTAMP scale of TimesTen takes nanoseconds (9digits), which is greater than the maximum DATE scale microseconds (6digits) of Altibase; thus, the potential data loss can be occurred. |
|  15  | TT_BIGINT     | BIGINT          | The minimum size of TT_BIGINT in TimesTen is -9,223,372,036,854,775,808 which is smaller than the minimum BIGINT size of Altibase, which is 9,223,372,036,854,775,807; thus, the potential data loss can be occurred. |
|  16  | TT_CHAR       | CHAR            |                                                              |
|  17  | TT_DATE       | DATE            |                                                              |
|  18  | TT_DECIMAL    | NUMBER          | The maximum TT_DECIMAL of TimesTen is precision(40) greater than the maximum precision size(38) of NUMBER in Altibase; thus, the potential data loss can be occurred. |
|  19  | TT_INTEGER    | INTEGER         | The minimum TT_INTEGER size of TimesTen is -2,147,483,648, which is smaller than the minimum INTEGER size of Altibase, which is -2,147,483,647; thus, the potential data loss can be occurred. |
|  20  | TT_NCHAR      | NCHAR           |                                                              |
|  21  | TT_NVARCHAR   | NVARCHAR        | The maximum TT_NVARCHAR size (2,097,152 bytes) of TimesTen is greater than that of the Altibase(32,000 bytes); thus, the potential data loss can be occurred. |
|  22  | TT_SMALLINT   | SMALLINT        | The potential data loss can be occurred since the minimum TT_SMALLINT size (-32,768) of TimesTen is smaller than the minimum SMALLINT size(-32,767) of Altibase. |
|  23  | TT_TIMESTAMP  | DATE            | The maximum scale of TT_TIMESTAMP in TimesTen is nanoseconds (7 digits), which is greater than the maximum scale of DATE in Altibase that is microseconds (6 digits); thus, the potential data loss can be occurred. |
|  24  | TT_TINYINT    | SMALLINT        |                                                              |
|  25  | TT_VARCHAR    | VARCHAR         | If TimesTen's TT_VARCHAR legnth exceeds 32,000 bytes, the maximum size of Altibase's VARCHAR, it is converted to Altibase's data type to CLOB. This is to prevent data loss that may occur during migration due to the maximum size difference between TimesTen and Altibase data types. TimesTen's maximum value for TT_VARCHAR is 4,194,304bytes, larger than Altibase. |
|  26  | VARBINARY     | BLOB            |                                                              |
|  27  | VARCHAR2      | VARCHAR         | 1. If TimesTen's VARCHAR2 legnth exceeds 32,000 bytes, the maximum size of Altibase's VARCHAR, it is converted to Altibase's data type to CLOB. This is to prevent data loss that may occur during migration due to the maximum size difference between TimesTen and Altibase data types. TimesTen's maximum value for VARCHAR2 is 4,194,304bytes, larger than Altibase.<br />2. VARCHAR2 defined as character length in TimesTen is converted into bytes in Altibase. Altibase's VARCHAR can be defined only in bytes. |

#### CUBRID to Altibase

|      | Source     | Destination   | Notice                                                       |
| :--: | :--------- | :------------ | :----------------------------------------------------------- |
|  1   | SHORT      | SMALLINT      | The minimum SHORT value of CURBID is - 32,768, which is smaller than the minimum value(-32,767) of SMALLINT in Altibase; thus the potential data loss can be occurred. |
|  2   | INTEGER    | INTEGER       | The minimum value of CUBRID( - 2,147,483,648) is smaller than that of the Altibase( -2,147,483,647); thus the potential data loss can be occurred. |
|  3   | BIGINT     | BIGINT        | The minimum value of CUBRID(- 9,223,372,036,854,775,808) is smaller than that of the Altibase ( -9,223,372,036,854,775,807);thus the potential data loss can be occurred. |
|  4   | NUMERIC    | NUMERIC       |                                                              |
|  5   | FLOAT      | REAL          |                                                              |
|  6   | DOUBLE     | DOUBLE        |                                                              |
|  7   | MONETARY   | DOUBLE        |                                                              |
|  8   | DATE       | DATE          |                                                              |
|  9   | TIME       | DATE          |                                                              |
|  10  | TIMESTAMP  | DATE          |                                                              |
|  11  | DATETIME   | DATE          |                                                              |
|  12  | CHAR       | CHAR          | If CUBRID's CHAR legnth exceeds 32,000 bytes, the maximum size of Altibase's CHAR, it is converted to Altibase's data type to CLOB. This is to prevent data loss that may occur during migration due to the maximum size difference between CUBRID and Altibase data types. CUBRID's maximum value for CHAR is 1,073,741,823 bytes, larger than Altibase. |
|  13  | VARCHAR    | VARCHAR       | If CUBRID's VARCHAR legnth exceeds 32,000 bytes, the maximum size of Altibase's VARCHAR, it is converted to Altibase's data type to CLOB. This is to prevent data loss that may occur during migration due to the maximum size difference between CUBRID and Altibase data types. CUBRID's maximum value for VARCHAR is 1,073,741,823 bytes, larger than Altibase. |
|  14  | NCHAR      | NCHAR         | Since the maximum size of NCHAR type in CUBRID is 1,073,741,823 bytes, which is greater than that of the Altibase( 16,000bytes), potential data loss can be occurred. |
|  15  | VARCHAR    | NVARCHAR      | The maximum size of VARCHAR type in CUBRID is 1,073,741,823 bytes and it is greater than the maximum NVARCHAR type of Altibase,which is 16,000 bytes; thus, potential data loss can be occured. |
|  16  | STRING     | VARCHAR       | The potential data loss can be occured since the maximum VARCHAR size of CUBRID is greater than that of Altiabse, which is 32,000 bytes with the identical data type. |
|  17  | BIT        | BLOB          |                                                              |
|  18  | VARBIT     | BLOB          |                                                              |
|  19  | BLOB       | BLOB          |                                                              |
|  20  | CLOB       | CLOB          |                                                              |
|  21  | ENUM       | VARCHAR(3200) | This is the data which is not supported by Altibase. The constants of enumeration character sting in CUBRID implement migration through arbitrarily converting them into VARCHAR type of Altibase. |
|  22  | COLLECTION | VARCHAR(3200) | This is the data type that Altibase does not support. The COLLECTION data type of CUBRID is converted as the VARCHAR data type of Altibase and the value of the former is also converted as string for the later during migrating. |

#### Altibase to Oracle

|      | Source   | Destination   | Notice                                                       |
| :--: | :------- | :------------ | :----------------------------------------------------------- |
|  1   | CHAR     | CHAR          | If Altibase's CHAR legnth exceeds 2,000 bytes, the maximum size of Oracle's CHAR, it is converted to Oracle's data type to CLOB. This is to prevent data loss that may occur during migration due to the maximum size difference between Altibase and Oracle. Altibase's CHAR maximum size is 32,000 bytes, which is larger than Oracle. |
|  2   | NCHAR    | NCHAR         | The potential data loss can be occurred since the maximum size of Altibase NCHAR is 32000 bytes and the maximum size of of Oracle NCHAR is 2000 bytes. |
|  3   | VARCHAR  | VARCHAR2      | If Altibase's VARCHAR legnth exceeds 4,000 bytes, the maximum size of Oracle's VARCHAR2, it is converted to Oracle's data type to CLOB. This is to prevent data loss that may occur during migration due to the maximum size difference between Altibase and Oracle. Altibase's VARCHAR maximum size is 32,000 bytes, which is larger than Oracle. |
|  4   | NVARCHAR | NVARCHAR2     | The potential data loss can be occurred since the maximum size of Altibase NVARCHAR is 32000 bytes and the maximum size of of Oracle NVARCHAR is 4000 bytes or characters. |
|  5   | SMALLINT | NUMBER        |                                                              |
|  6   | INTEGER  | NUMBER        |                                                              |
|  7   | BIGINT   | NUMBER        |                                                              |
|  8   | REAL     | BINARY_FLOAT  |                                                              |
|  9   | DOUBLE   | BINARY_DOUBLE |                                                              |
|  10  | FLOAT    | FLOAT         |                                                              |
|  11  | NUMERIC  | NUMBER        |                                                              |
|  12  | DATE     | TIMESTAMP     |                                                              |
|  13  | BIT      | CHAR          | The potential data loss can be occurred since the maximum size of CHAR is 2000 bytes, and the maximum size of BIT is 64000 bits, that is, 8000 bytes. |
|  14  | VARBIT   | VARCHAR2      | The potential data loss can be occurred since the maximum size of VARCHAR2 is 4000 bytes, and the maximum size of VARBIT is 64000 bits, that is, 8000 bytes. |
|  15  | BYTE     | RAW           | The potential data loss can be occurred since the maximum size of BYTE is 32000 bytes, and the maximum size of RAW is 2000 bytes. |
|  16  | VARBYTE  | RAW           | The potential data loss can be occurred since the maximum size of VARBYTE is 32000 bytes, and the maximum size of RAW is 2000 bytes. |
|  17  | NIBBLE   | RAW           |                                                              |
|  18  | BLOB     | BLOB          |                                                              |
|  19  | CLOB     | CLOB          |                                                              |

#### Tibero to Altibase

|      | Source        | Destination     | Notice                                                       |
| :--: | :------------ | :-------------- | :----------------------------------------------------------- |
|  1   | CHAR          | CHAR            | Since Altibase's CHAR type can be defined only in byte length, in case of a column defined as character length in Tibero, it is automatically converted to byte length. |
|  2   | NCHAR         | NCHAR           |                                                              |
|  3   | VARCHAR       | VARCHAR         | Altibase's VARCHAR type can be defined only in byte length, so in case of a column defined as character length in Tibero, it is automatically converted to byte length. |
|  4   | NVARCHAR      | NVARCHAR        |                                                              |
|  5   | LONG          | CLOB            |                                                              |
|  6   | NUMBER        | NUMERIC         | NUMBER type columns defined without precision and scale in Tibero are converted to the same NUMBER type columns without precision and scale for Altibase. *Both Tibero and Altibase internally handle NUMBER type columns defined without precision and scale as FLOAT type in the database. |
|  7   | BINARY FLOAT  | FLOAT           |                                                              |
|  8   | BINARY DOUBLE | VARCHAR(310)    | There is no compatible data type in Altibase for the Tibero binary double type, so the data is stored in character form to prevent loss. |
|  9   | DATE          | DATE            |                                                              |
|  10  | TIME          | DATE            |                                                              |
|  11  | TIMESTAMP     | DATE            | A small amount of data loss may occur due to the difference in scale. In Tibero, the scale of a timestamp value is nanoseconds (9 digits), whereas in Altibase, the scale of a timestamp value is microseconds (6 digits). |
|  12  | RAW           | BLOB            |                                                              |
|  13  | LONG RAW      | BLOB            |                                                              |
|  14  | BLOB          | BLOB            |                                                              |
|  15  | CLOB          | CLOB            |                                                              |
|  16  | NCLOB         | NVARCHAR(10666) | There is no compatible data type in Altibase for the Tibero NCLOB, so the data is stored in NVARCHAR with the maximum precision. This may cause data loss during data migration when the actual data precision exceeds the NVARCHAR maximum size. |
|  17  | ROWID         | VARCHAR(18)     | Tibero's ROWID converts to a character data type. Altibase does not support the data type ROWID. |

### Automatic Correction of Character Column Length Considering Heterogeneous Character Set

When the character sets of the source and destination databases are different during migration, character data types (CHAR, VARCHAR) require length conversion.
For example, if the source database is set to the MS949 character set that requires a maximum of 2 bytes per character storage, and the target database is set to the UTF8 character set that requires 3 bytes per character, the character data of the target database is required to migrate without truncation. The size of the type should be 1.5 times the original.

Migration Center automatically performs this length conversion, and the length correction formula for character data types is as follows.

```
Dest. Size = Ceil(Correction Factor * Src. Size)
Correction Factor = Dest. MaxBytes / Src. MaxBytes
* MaxBytes = The maximum number of bytes required to store one character
```

However, if MaxBytes of the original is 1 or the correction factor is less than 1, the length conversion is not performed.

The MaxBytes and correction factors of the source and target databases can be found on the summary page of the build report.

#### Precautions

For large tables, the length of data storage in the target database can be much larger than the original due to length correction. If there is a guarantee that the data will not be truncated without converting the length, the length can be manually specified in the reconcile step.

#### Support Character Set for Each Database

For character sets not listed in the table below, Migration Center does not perform length correction.

##### Altibase

| Character Set | Max. Bytes Per Character |
| ------------- | ------------------------ |
| KO16KSC5601   | 2                        |
| MS949         | 2                        |
| BIG5          | 2                        |
| GB231280      | 2                        |
| MS936         | 2                        |
| UTF8          | 3                        |
| SHIFTJIS      | 2                        |
| MS932         | 2                        |
| EUCJP         | 3                        |

##### Cubrid

| Character Set | Max. Bytes Per Character |
| ------------- | ------------------------ |
| utf8          | 3                        |
| euckr         | 2                        |

##### Informix

| Character Set      | Max. Bytes Per Character |
| ------------------ | ------------------------ |
| zh_cn.GB18030_2000 | 4                        |
| zh_tw.big5         | 2                        |
| zh_tw.euctw        | 4                        |
| zh_cn.gb           | 2                        |
| zh_tw.sbig5        | 2                        |
| zh_tw.ccdc         | 2                        |
| ja_jp.sjis-s       | 2                        |
| ja_jp.ujis         | 3                        |
| ja_up.sjis         | 2                        |
| ko_kr.cp949        | 2                        |
| ko_kr.ksc          | 2                        |

##### MySQL

See the result set of the query below.

```
SELECT CHARACTER_SET_NAME,MAXLEN FROM INFORMATION_SCHEMA.CHARACTER_SETS;
```

##### SQL Server

| Code Page | Max. Bytes Per Character |
| --------- | ------------------------ |
| 932       | 2                        |
| 936       | 2                        |
| 949       | 2                        |
| 950       | 2                        |

##### Oracle

| Character Set | Max. Bytes Per Character |
| ------------- | ------------------------ |
| AL32UTF8      | 4                        |
| JA16EUC       | 3                        |
| JA16EUCTILDE  | 3                        |
| JA16SJIS      | 2                        |
| JA16SJISTILDE | 2                        |
| KO16MSWIN949  | 2                        |
| UTF8          | 3                        |
| ZHS16GBK      | 2                        |
| ZHT16HKSCS    | 2                        |
| ZHT16MSWIN950 | 2                        |
| ZHT32EUC      | 4                        |

##### Tibero

| Character Set | Max. Bytes Per Character |
| ------------- | ------------------------ |
| UTF8          | 3                        |
| EUCKR         | 2                        |
| MSWIN949      | 2                        |
| SJIS          | 2                        |
| JA16SJIS      | 2                        |
| JA16SJISTILDE | 2                        |
| JA16EUC       | 3                        |
| JA16EUCTILDE  | 3                        |
| GBK           | 2                        |
| ZHT16HKSCS    | 2                        |

##### TimesTen

| Character Set  | Max. Bytes Per Character |
| -------------- | ------------------------ |
| AL16UTF16      | 4                        |
| AL32UTF8       | 4                        |
| JA16EUC        | 3                        |
| JA16EUCTILDE   | 3                        |
| JA16SJIS       | 2                        |
| JA16SJISTILDE  | 2                        |
| KO16KSC5601    | 2                        |
| KO16MSWIN949   | 2                        |
| ZHS16CGB231280 | 2                        |
| ZHS16GBK       | 2                        |
| ZHS32GB18030   | 4                        |
| ZHT16BIG5      | 2                        |
| ZHT16HKSCS     | 2                        |
| ZHT16MSWIN950  | 2                        |
| ZHT32EUC       | 4                        |

<br/>

