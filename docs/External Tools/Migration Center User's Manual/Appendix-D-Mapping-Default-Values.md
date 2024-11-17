# Appendix D: Mapping Default Values

The default values for Altibase table columns are mostly compatible with the default values for the source database. 

However, the default value policy can differ among heterogeneous databases. In the event of such exceptional situations, Migration Center converts source database values according to the Altibase policy. 

This appendix provides a mapping table for default values according to which Migration Center converts source database default values to comply with Altibase.

### Mapping Table for Default Values

Prior to migrating data, Migration Center creates a table identical to the source database table in the migration destination database. Migration Center first creates a CREATE TABLE statement with the equivalent attributes as the source database table. To set the same default values for both source table columns and target table columns, Migration Center converts the default values in the following mapping tables and specifies them in the CREATE TABLE statement, when creating the statement. Default values excluded from the table are specified in the statement without being changed.

> Note: Some default values that are not changed can be incompatible with source and destination databases. If necessary, you must manually alter the default value for the CREATE TABLE statement in the DDL Editing window at the Reconcile Step. 

#### Default Value Mapping Policy

Default values of most of the original database are compatible with the target database without any modifications. However, Migration Center converts source database default values according to the policy of the target database system for the following exceptions.

- The CHARACTER data type with a zero length string as the default value: Altibase identifies zero length strings as NULL; therefore, the default value is not specified. 
- The DATE data type with a string expression as the default value: Since the default format for the DATE data type differs among source databases, Migration Center specifies a comment which includes the DEFAULT keyword in the CREATE TABLE statement, instead of the default value. If necessary, the user must manually set the default value later on, by referring to the comment. However, if the source database is the one among the MySQL, TimesTen or CUBRID, Migration Center automatically converts default values as shown below. 
- The default value specified with a function: A function which is listed in the following table is converted accordingly, only if the function is exclusively specified as the default value in the source database. Other functions or expressions of a complex form are converted without being changed. If necessary, the user must manually change them later on. 

#### Oracle Database to Altibase

<table>
    <tr>        
        <th>Expression Type</th> <th>Source(Oracle)</th><th>Destination(Altibase)</th><th>Remarks</th>
    </tr>
    <tr>
        <td>String for
CHARACTER Data
Type</td><td>"</td><td></td><td></td>
    </tr>
    <tr>
        <td>String for DATE Data
Type</td><td>'97/04/21'</td><td>/* DEFAULT '97/04/21' */</td><td></td>
    </tr>
    <tr>
        <td rowspan="4">Function</td><td>DBTIMEZONE</td><td>DB_TIMEZONE()</td><td>Supported for Altibase
6.3.1.0.0 or above.</td>
    </tr>
    <tr>
        <td >SYS_GUID()</td><td>SYS_GUID_STR()</td><td>Supported for Altibase
6.3.1.0.0 or above.</td>
    </tr>
     <tr>
        <td >UID</td><td>USER_ID()</td><td></td>
    </tr>
    <tr>
         <td >USER</td><td>USER_NAME()</td><td></td>
    </tr>
</table>

The following is an example of the conversion.

| Oracle CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                                                                                            | Altibase CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CREATE TABLE testtbl_4_defval<br /> ( c1 INT DEFAULT 123, <br />c2 VARCHAR(50) DEFAULT 'test', <br />c3 INT DEFAULT NULL,<br /> c4 CHAR(10) DEFAULT '', <br />c5 INT DEFAULT SQRT(144) + 72, <br />c6 DATE DEFAULT '97/04/21', <br />c7 DATE DEFAULT TO_DATE('1999-12-01', 'YYYY-MM-DD'), <br />c8 VARCHAR(100) DEFAULT DBTIMEZONE, <br />c9 VARCHAR(100) DEFAULT SYS_GUID(), <br />c10 VARCHAR(100) DEFAULT UID, <br />c11 VARCHAR(100) DEFAULT USER ); | CREATE TABLE TESTTBL_4_DEFVAL<br /> ( C1 NUMBER (38, 0) DEFAULT 123, <br />C2 VARCHAR (50) DEFAULT 'test', <br />C3 NUMBER (38, 0), <br />C4 CHAR (10), <br />C5 NUMBER (38, 0) DEFAULT SQRT(144) + 72, <br />C6 DATE /\* DEFAULT '97/04/21' \*/, <br />C7 DATE DEFAULT TO_DATE('1999-12-01', 'YYYY-MM-DD'), <br />C8 VARCHAR (100) DEFAULT DB_TIMEZONE(), <br />C9 VARCHAR (100) DEFAULT SYS_GUID_STR(), <br />C10 VARCHAR (100) DEFAULT USER_ID(), <br />C11 VARCHAR (100) DEFAULT USER_NAME() ); |

#### MS SQL Server to Altibase

<table>
    <tr>        
        <th>Expression Type</th> <th>Source
(MS SQL Server)</th><th>Destination
(Altibase)</th><th>Remarks</th>
    </tr>
    <tr>
        <td>String for
CHARACTER Data
Type</td><td>"</td><td></td><td></td>
    </tr>
    <tr>
        <td>String for DATE Data
Type</td><td>'December 5, 1985'</td><td>/* DEFAULT 'December 5, 1985' */</td><td></td>
    </tr>
    <tr>
        <td rowspan="3">Function</td><td>GETDATE90</td><td>SYSDATE</td><td></td>
    </tr>
    <tr>
        <td >CURRENT_TIMESTAMP</td><td></td><td></td>
    </tr>
     <tr>
        <td >LEN( str_expression )</td><td>LENGTH( str_expression )</td><td></td>
    </tr>
</table>

The following is an example of the conversion.

| MS SQL CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                           | Altibase CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| CREATE TABLE testtbl_4_defval <br />( c1 BIT DEFAULT 0, <br />c2 INT DEFAULT 2 + 3, <br />c3 VARCHAR(50) DEFAULT 'test', <br />c4 INT DEFAULT NULL, <br />c5 NCHAR(10) DEFAULT '', <br />c6 FLOAT DEFAULT sqrt(12 \* 12), <br />c7 DATE DEFAULT 'December 5, 1985', <br />c8 DATE DEFAULT getdate(), <br />c9 DATETIME DEFAULT CURRENT_TIMESTAMP,<br /> c10 INT DEFAULT len('test'), ); | CREATE TABLE TESTTBL_4_DEFVAL<br /> ( C1 CHAR (1) DEFAULT (0), <br />C2 INTEGER DEFAULT (2)+(3), <br />C3 VARCHAR (50) DEFAULT 'test', <br />C4 INTEGER, <br />C5 NCHAR (10), <br />C6 VARCHAR (310) DEFAULT sqrt((12)\*(12)), <br />C7 DATE /\* DEFAULT 'December 5, 1985' \*/, <br />C8 DATE DEFAULT SYSDATE, <br />C9 DATE DEFAULT SYSDATE, <br />C10 INTEGER DEFAULT LENGTH('test') ); |

#### MySQL to Altibase

<table>
    <tr>        
        <th>Expression Type</th> <th>Source
(MySQL)</th><th>Destination
(Altibase)</th><th>Remarks</th>
    </tr>
    <tr>
        <td>String for
CHARACTER Data
Type</td><td>"</td><td></td><td></td>
    </tr>
    <tr>
        <td rowspan="3">String for DATE Data
Type</td><td>'1989-04-28'</td><td>TO_DATE('1989-04-28', 'YYYY-MM-DD')</td><td></td>
    </tr>
     <tr>
        <td> '1989-04-28 12:31:29'</td><td>TO_DATE('1989-04-28 12:31:29', 'YYYY-MM-DD HH:MI:SS')</td><td></td>
    </tr>
    <tr>
        <td>'0000-00-00 00:00:00'</td><td>/* DEFAULT '0000-00-00 00:00:00' */</td><td>If the default value for
the DATE data type is
omitted, MySQL
automatically specifies
it as '0000-00-00
00:00:00'. However,
since this value cannot
be input to the DATE
type for Altibase, a
comment is left.</td>
    </tr>
    <tr>
    <td rowspan="7">Function</td><td>CURRENT_TIMESTAMP</td><td rowspan="7">SYSDATE</td><td></td>
    </tr>
    <tr>
        <td >CURRENT_TIMESTAMP()</td><td></td>
    </tr>
     <tr>
        <td >NOW()</td><td></td>
    </tr>
    <tr>
        <td >LOCALTIME</td><td></td>
    </tr>
    <tr>
        <td >LOCALTIME()</td><td></td>
    </tr>
    <tr>
        <td >LOCALTIMESETAMP</td><td></td>
    </tr>
    <tr>
        <td >LOCALTIMESETAMP()</td><td></td>
    </tr>
</table>

> Note: If the first column of a table is of the TIMESTAMP data type, MySQL automatically specifies CURRENT_TIMESTAMP as the default value, even if the user omits it. In this case, the default value is converted to SYSDATE. Please refer to the following example.

The following is an example of the conversion.

| MySQL CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                        | Altibase CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CREATE TABLE testtbl_4_defval <br />( c1 TIMESTAMP NOT NULL, <br />c2 INT DEFAULT 123, <br />c3 VARCHAR(50) DEFAULT 'test', <br />c4 INT DEFAULT NULL, <br />c5 CHAR(10) DEFAULT '', <br />c6 DATE DEFAULT '1989-04-28', <br />c7 DATETIME DEFAULT '1989-04-28 12:31:29', <br />c8 TIMESTAMP DEFAULT '1989-04-28 12:31:29' NOT NULL, <br />c9 TIMESTAMP NOT NULL ); | CREATE TABLE TESTTBL_4_DEFVAL <br />( C1 DATE DEFAULT SYSDATE NOT NULL, <br />C2 INTEGER DEFAULT 123, <br />C3 CLOB DEFAULT 'test', C4 INTEGER, <br />C5 CHAR (10), <br />C6 DATE DEFAULT TO_DATE('1989-04-28', 'YYYY-MM-DD'), <br />C7 DATE DEFAULT TO_DATE('1989-04-28 12:31:29', 'YYYY-MM-DD HH:MI:SS'), <br />C8 DATE DEFAULT TO_DATE('1989-04-28 12:31:29', 'YYYY-MM-DD HH:MI:SS') NOT NULL, <br />C9 DATE /\* DEFAULT '0000-00-00 00:00:00' \*/ NOT NULL ); |

#### Informix 11.5 to Altibase

<table>
    <tr>        
        <th>Expression Type</th> <th>Source
(Informix)</th><th>Destination
(Altibase) </th><th>Remarks</th>
    </tr>
    <tr>
        <td>String for
CHARACTER Data
Type</td><td>"</td><td></td><td></td>
    </tr>
    <tr>
        <td>String for DATE Data
Type</td><td>'2007-03-06'</td><td>/* DEFAULT '2007-03-06' */</td><td></td>
    </tr>
    <tr>
        <td rowspan="2">Function</td><td>CURRENT</td><td>SYSDATE</td><td></td>
    </tr>
    <tr>
        <td >TODAY</td><td>SYSDATE</td><td></td>
    </tr>     
</table>

The following is an example of the conversion.

| Informix CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                          | Altibase CREATE TABLE Statement                                                                                                                                                                                                                                                    |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CREATE TABLE testtbl_4_defval ( <br />c1 INTEGER DEFAULT 123, <br />c2 BOOLEAN DEFAULT 't',<br />c3 CHAR(100) DEFAULT 'test', <br />c4 INTEGER DEFAULT null, <br />c5 CHAR(10) DEFAULT '', <br />c6 DATETIME YEAR TO DAY DEFAULT DATETIME(07-3-6) YEAR TO DAY, <br />c7 DATETIME DAY TO HOUR DEFAULT CURRENT DAY TO HOUR, <br />c8 DATE DEFAULT TODAY ); | CREATE TABLE TESTTBL_4_DEFVAL ( <br />C1 INTEGER DEFAULT 123, <br />C2 CHAR (1) DEFAULT 't', <br />C3 CHAR (100) DEFAULT 'test', <br />C4 INTEGER, <br />C5 CHAR (10), <br />C6 DATE /\* DEFAULT '2007-03-06' \*/, <br />C7 DATE DEFAULT SYSDATE, <br />C8 DATE DEFAULT SYSDATE ); |

#### TimesTen to Altibase

<table>
    <tr>        
        <th>Expression Type</th> <th>Source(TimesTen)</th><th>Destination(Altibase)</th><th>Remarks</th>
    </tr>
    <tr>
        <td rowspan="3">String for DATE Data
Type</td><td>'1989-04-28'</td><td>TO_DATE('1989-04-28', 'YYYY-MM-DD')</td><td></td>
    </tr>
    <tr>
        <td>'1989-04-28 12:31:29'</td><td>TO_DATE('1989-04-28 12:31:29', 'YYYY-MM-DD HH:MI:SS')</td><td></td>        
    </tr>
    <tr>
        <td>'12:31:29'</td>    <td>TO_DATE('12:31:29', 'HH:MI:SS')</td>    <td></td>    
    </tr>
    <tr>
        <td rowspan="2">Function</td><td>UID</td><td>USER_ID</td><td></td>
    </tr>
    <tr>
        <td >USER</td><td>USER_NAME</td><td></td>
    </tr>     
</table>

The following is an example of the conversion.

| TimesTen CREATE TABLE Statement                                                                                                                                                                                                                                                                                             | Altibase CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                                  |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| CREATE TABLE testtbl_4_defval ( <br />c1 INT DEFAULT 123, <br />c2 VARCHAR2(50) DEFAULT 'test', <br />c3 INT DEFAULT NULL, <br />c4 DATE DEFAULT '1999-12-01', <br />c5 TIMESTAMP DEFAULT '1999-12-01 11:30:21', <br />c6 TIME DEFAULT '11:30:21', <br />c7 VARCHAR(100) DEFAULT UID, <br />c8 VARCHAR(100) DEFAULT USER ); | CREATE TABLE TESTTBL_4_DEFVAL ( <br />c1 INT DEFAULT 123, <br />c2 VARCHAR2(50) DEFAULT 'test', <br />c3 INT DEFAULT NULL, <br />c4 DATE DEFAULT TO_DATE('1999-12-01', 'YYYY-MM-DD'), <br />c5 TIMESTAMP DEFAULT TO_DATE('1999-12-01 11:30:21', 'YYYY-MM-DD HH:MI:SS), <br />c6 TIME DEFAULT TO_DATE('11:30:21', 'HH:MI:SS'), c7 VARCHAR(100) DEFAULT UID, <br />c8 VARCHAR(100) DEFAULT USER ); |

#### CUBRID to Altibase

<table>
    <tr>        
        <th>Expression Type</th> <th>Source(CUBRID)</th><th>Destination(Altibase)</th><th>Remarks</th>
    </tr>   
    <tr>
        <td rowspan="2">Function</td><td>USER</td><td>USER_ID()</td><td></td>
    </tr>
    <tr>
        <td >CURRENT_USER</td><td>USER_NAME()</td><td></td>
    </tr>     
</table>

The following is an example of the conversion.

| CUBRID CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                                                                                                                | Altibase CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CREATE TABLE testtbl_4_defval ( <br />c1 INTEGER DEFAULT 123, <br />c2 CHARACTER VARYING (50) DEFAULT 'test', <br />c3 INTEGER, <br />c4 CHARACTER VARYING (100) DEFAULT 'USER', <br />c5 CHARACTER VARYING (100) DEFAULT 'CURRENT_USER', <br />c6 CHARACTER VARYING(100) DEFAULT ' ', <br />c7 DATE DEFAULT DATE'2008-10-31', <br />c8 TIME DEFAULT TIME'1:15', <br />c9 TIMESTAMP DEFAULT TIMESTAMP'10/31', <br />c10 DATETIME DEFAULT DATETIME'01:15:45 PM 2008-10-31' ); | CREATE TABLE TESTTBL_4_DEFVAL ( <br />C1 INTEGER DEFAULT 123, <br />C2 VARCHAR (50) DEFAULT 'test', <br />C3 INTEGER, <br />C4 VARCHAR (100) DEFAULT USER_ID(), <br />C5 VARCHAR (100) DEFAULT USER_ID(), <br />C6 VARCHAR (100) DEFAULT ' ', <br />C7 DATE /\* DEFAULT '10/31/2008' \*/, <br />C8 DATE /\* DEFAULT '01:15:00 AM' \*/, <br />C9 DATE /\* DEFAULT '12:00:00 AM 10/31/2016' \*/, <br />C10 DATE /\* DEFAULT '01:15:45.000 PM 10/31/2008' \*/ ); |

#### Altibase to Oracle

<table>
    <tr>        
        <th>Expression Type</th> <th>Source(Altibase)</th><th>Destination(Oracle)</th><th>Remarks</th>
    </tr>
    <tr>
        <td>String for Character
type</td><td>"</td><td></td><td></td>
    </tr>
    <tr>
    <td rowspan="4">Function</td><td>DB_TIMEZONE()</td><td>DBTIMEZONE</td><td>Supported in Altibase
6.3.1.0.0 or above</td>
    </tr>
    <tr>
        <td >SYS_GUID_STR()</td><td>SYS_GUID()</td><td>Supported in Altibase
6.3.1.0.0 or above</td>
    </tr> 
    <tr>
        <td>USER_ID()</td><td>UID</td><td></td>
    </tr>
    <tr>
        <td>USER_NAME()</td><td>USER</td><td></td>
    </tr>
</table>

The following is an example of the conversion.

| Altibase Table Creation SQL statements                                                                                                                                                                                                                                                                                                                                                                                                          | Oracle Table Creation SQL statements                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CREATE TABLE testtbl_4_defval <br />( c1 INT DEFAULT 123, <br />c2 VARCHAR(50) DEFAULT 'test', <br />c3 INT DEFAULT NULL, <br />c4 CHAR(10) DEFAULT '', <br />c5 INT DEFAULT SQRT(144) + 72, <br />c6 DATE DEFAULT TO_DATE('1999-12-01 PM', 'YYYY-MM-DD AM'), <br />c7 VARCHAR(100) DEFAULT DB_TIMEZONE(), <br />c8 VARCHAR(100) DEFAULT SYS_GUID_STR(), <br />c9 VARCHAR(100) DEFAULT USER_ID(), <br />c10 VARCHAR(100) DEFAULT USER_NAME() ); | CREATE TABLE TESTTBL_4_DEFVAL <br />( C1 NUMBER (10) DEFAULT 123 ,<br />C2 VARCHAR2 (50) DEFAULT 'test' ,<br />C3 NUMBER (10) ,<br />C4 CHAR (10) ,<br />C5 NUMBER (10) DEFAULT SQRT(144) + 72 ,<br />C6 TIMESTAMP  DEFAULT TO_DATE('1999-12-01 PM', 'YYYY-MM-DD AM') ,<br />C7 VARCHAR2 (100) DEFAULT DBTIMEZONE ,<br />C8 VARCHAR2 (100) DEFAULT SYS_GUID() ,<br />C9 VARCHAR2 (100) DEFAULT UID ,<br />C10 VARCHAR2 (100) DEFAULT USER ); |

#### Tibero to Altibase

<table>
    <tr>        
        <th>Expression Type</th> <th>Source(Tibero)</th><th>Destination(Altibase)</th><th>Remarks</th>
    </tr>
    <tr>
        <td>String for
CHARACTER Data
Type</td><td>"</td><td></td><td></td>
    </tr>
     <tr>
        <td>String for DATE Data
Type</td><td>'97/04/21'</td><td>/* DEFAULT '97/04/21' */    </td><td></td>
    </tr>
    <tr>
    <td rowspan="4">Function</td><td>DBTIMEZONE</td><td>DB_TIMEZONE()</td><td>Supported in Altibase
6.3.1.0.0 or above.</td>
    </tr>
    <tr>
        <td >SYS_GUID_GUID()</td><td>SYS_GUID_STR()</td><td>Supported in Altibase
6.3.1.0.0 or above.</td>
    </tr> 
    <tr>
        <td>UID</td><td>USER_ID()</td><td></td>
    </tr>
    <tr>
        <td>USER</td><td>USER_NAME()</td><td></td>
    </tr>
</table>

The following is an example of the conversion.

| Tibero CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                                                                                          | Altibase CREATE TABLE Statement                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CREATE TABLE testtbl_4_defval( <br />c1 INT DEFAULT 123, <br />c2 VARCHAR(50) DEFAULT 'test', <br />c3 INT DEFAULT NULL, <br />c4 CHAR(10) DEFAULT '', <br />c5 INT DEFAULT QRT(144) + 72, <br />c6 DATE DEFAULT '97/04/21', <br />c7 DATE DEFAULT TO_DATE('1999-12-01', 'YYYY-MM-DD'), <br />c8 VARCHAR(100) DEFAULT DBTIMEZONE, <br />c9 VARCHAR(100) DEFAULT SYS_GUID(), <br />c10 VARCHAR(100) DEFAULT UID, <br />c11 VARCHAR(100) DEFAULT USER ); | CREATE TABLE TESTTBL_4_DEFVAL(  <br />C1  NUMBER (38, 0)  DEFAULT 123,    <br />C2  VARCHAR (50)    DEFAULT 'test',    <br />C3  NUMBER (38, 0),    <br />C4  CHAR (10),    <br />C5  NUMBER (38, 0)  DEFAULT SQRT(144) + 72,   <br />C6  DATE /\* DEFAULT '97/04/21' \*/,    <br />C7  DATE DEFAULT TO_DATE('1999-12-01', 'YYYY-MM-DD'),    <br />C8  VARCHAR (100)   DEFAULT DB_TIMEZONE(),    <br />C9  VARCHAR (100)   DEFAULT SYS_GUID_STR(),<br />C10 VARCHAR (100)   DEFAULT USER_ID(), <br />C11 VARCHAR (100)   DEFAULT USER_NAME() ); |

<br/>

