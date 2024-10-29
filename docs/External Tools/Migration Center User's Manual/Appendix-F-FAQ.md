# Appendix F: FAQ

### Common

#### FAQ 1. OutOfMemoryError occurs during data transfer.

##### Cause

This error can occur due to a lack of memory while several threads are inserting data into the target database through batch operations.

##### Solution

According to the error message output from OutOfMemoryError, there are two cases as below.

###### \<Java heap space\>

Depending on the situation, you can choose between the two methods below.

- Change performance property values to reduce memory usage
1. Open the project. 
2. Click the menu "Migration" -> "Migration Options". 
3. Lower the Batch Size and Thread Count values
- Increase the maximum amount of memory the program can use
1. Open the executable file (migcenter.bat or migcenter.sh) with the editor. 
2. Set the value of the option '-Xmx' that sets the maximum heap size in the JVM to be higher than the existing value.

> Note: On Windows 32 bit machines, you can set Xmx values up to 1.5 GB due to OS dependency.

###### \<PermGen space\>

1. Open the executable file (migcenter.bat or migcenter.sh) with the editor. 
2. Set the '-XX: MaxPermSize' option to set the maximum size of the permanent generation space in the JVM above the existing value.

###### \<Metaspace\>

If the version of the JVM you are using is Java 8 or higher, the lack of space in Metaspace may be the cause. Metaspace, implemented since Java 8, is a replacement for PermGen (permanent generation space).

* Open the executable file (migcenter.bat or migcenter.sh) with an editor.

* Option to set the maximum size of permanent generation space in JVM
  Change '-XX: MaxPermSize' to an option that determines the maximum size of the metaspace, and modify it higher than the existing value.
- - Before change: -XX:MaxPermSize=128m
  - After change: -XX:MaxMetaspaceSize=256 m

References:

- <https://dzone.com/articles/java-8-permgen-metaspace>

- <https://www.infoq.com/articles/Java-PERMGEN-Removed>

#### The NOT NULL constraint on a table column whose data type is LOB is not migrated.

##### Cause

It is a phenomenon that the Migration Center arbitrarily removes the NOT NULL constraint on the LOB column. 

The Migration Center inserts data into the target database using a query that contains parameters (for example, “insert into tablename values (?,?)”). 

Unlike other columns, Altibase processes data into LOB columns by initializing data to null first, then receiving data through LOB Locator and inputting data. Therefore, if the column has a NOT NULL constraint, the insertion is failed because the data cannot be initialized to null. 

In order to overcome this difference, the Migration Center removes the NOT NULL constraint on the LOB column to perform data migration. For more detailed information, refer to the  *General Reference* - 1. Data Type - LOB Data Type - Restrictions. 

Because Oracle performs constraint checking only at commit time, you can insert data even under these conditions.

##### Solution

After the migration, execute an SQL statement that adds a NOT NULL constraint to the corresponding column in Altibase

#### Database Character Set Notes

Generally, it is recommended to use the same DB character set of the source database and target database.  
If you need to specify different character sets because of special circumstances, you should check whether each DB character set is compatible. If you migrate data between incompatible character sets, the data may be corrupted.

##### Example

1. From source database KSC5601 to Destination database UTF8:  
   Migratable  
   KSC5601 Korean data can be written in UTF8. Therefore, each character set is compatible with each other.  
   **Note**: Because the data length can be longer, you need to increase the size of the character type column when you relocate the table object.

2. From source database KSC5601 to Destination database GB231280:  
   Not migratable  
   KSC5601 Korean data cannot be written as GB231280. Therefore, each character set is incompatible with each other.  
   If data migration is required under these conditions, the table column data types CHAR and VARCHAR of the target database should all be changed to NCHAR and NVARCHAR, and data transfer must be performed.

##### JDBC & Migration Center Character Set Process

1. When fetching data from the source database, get the char data as the source database DB character set and store it in a byte array.
2. Convert the data stored in the byte array to UTF-16 format and store it in a String object of Java base type.
3. Pass the String object to the PreparedStatement object that inserts the data into the target database with the setString function. 
4. JDBC driver converts the String object into data into the DB character set of the target database and insert them into the target database.

#### The program terminates abnormally while selecting the JDBC driver file.

##### Cause

This is an error that can occur when running the Migration Center in a Windows environment. While selecting the JDBC driver file when registering the connection information, changing the directory may cause the problem that the program terminates abnormally. This is a Java JVM crash caused by a communication problem between the JVM and the Windows operating system. Users can check for the Java crashes that usually occurs in the old version of the JVM through the link below.

<http://www.java.com/en/download/help/error_hotspot.xml>

##### Solution

Install the latest version of the JRE, modify the JAVA_HOME path in the migcenter.bat file, and rerun the Migration Center.

#### When running the CLI Mode, UnsatisfiedLinkError: /usr/lib/jvm/java-8-oracle/jre/lib/amd64/libawt_xawt.so: libXrender.so.1: cannot open shared object file: No such file or directory has occurred.

This error occurs when the 64-bit libXrender.so file is requested by the JVM, but the package is not installed in the OS.

##### Cause

This usally occurs when you install a 32-bit JRE on a 64-bit device and try to run the Java program by using it.

##### Solution

After installing a new JRE that matches the bit value of the device, change JAVA_HOME to the appropriate location. Debian-like Linux installs a package by executing the following command. 

```
sudo apt-get install libXrender1
```

##### References:

- http://www.jmeter-archive.org/Bug-in-running-Jmeter-on-Ubuntu-12-04-td5722692.html
- https://www.spigotmc.org/threads/bungeecord-not-starting-up-on-java-8.24652/ 

#### When Running the Migration Center, the message "Could not create the java virutal machine" is displayed and the startup fails.

##### Cause

This is an error that can occur when the maximum memory allocation value (-Xmx) set in bat and sh is lager than the memory allocated by the system. In particular, this error is frequently reported in Window O/S 32 bit

##### Solution

Change the value of -Xms -Xmx in bat and sh according to the user environment, and then restart Migration Center.

### Oracle

#### FAQ 1. The error message 'ORA-01652 unable to extend temp segment by 128 in tablespace TEMP' is printed.

##### Cause

This error is caused by a lack of Oracle's temporary tablespace space during large-volume query processing.

##### Solution

The temporary table space for the connected user must be extended.

#### FAQ 2. During the Run phase, error message 'Fetch data from source database has been failed. Stream has already been closed' is printed and migration of some data is failed.

##### Cause

Tables containing LONG or LONG RAW and LOB columns can cause problems during data migration. Below is an excerpt from the Oracle JDBC Developer's Guide (link).

12. Java Streams in JDBC - Streaming LONG or LONG RAW Columns

Because the column data remains in the communications channel, the streaming
mode interferes with all other use of the connection. Any use of the connection,
other than reading the column data, will discard the column data from the
channel.

In Oracle, data transmission of a LONG or LONG RAW column is done through a stream. If stream of other data types also is transmitted through the same connection, the data transmission can be interrupted. This means that if a LONG or LONG RAW column and a LOB column are in the same table, it cannot guarantee that the data in the table will succeed. For this reason, Oracle does not recommend using this configuration.

##### Solution

The table cannot be migrated via the Migration Center.

#### When you start the Reconcile step, the error message 'Unable to find any volatile tablespace to store temporary tables in the destination database' is displayed.

##### Cause

If there is a global temporary table among the list of migration objects of the source database, Oracle, a volatile tablespace must exist in the target database, Altibase. Oracle's global temporary tables are migrated to Altibase's temporary tables, and Altibase's temporary tables can only be stored in volatile tablespaces. (Refer to the manual: *SQL Reference - 3.* Data Definition Language - CREATE TABLE - Description) 

When performing the Reconcile step, the Migration Center takes a list of Altibase tablespaces that the user can access and tries to map tablespaces and tables between databases. At this time, this error occurs when there is no accessible volatile tablespace in Altibase to be mapped for a global temporary table in Oracle.

##### Solution

Create a volatile tablespace in the target database, Altibase, grant access privileges, and then perform the reconcile step again.

#### SQLException: Protocol violation occurs during data migration.

##### Cause

An OOM error occurred during communication and it was lost, and a protocol violation error was returned.

##### Solution

Increase the maximum memory size that the program can use.

1. Open the executable file (migcenter.bat or migcenter.sh) with an editor.
2. Modify the value of option '-Xmx', which determines the maximum heap size in the JVM, than the existing value.

##### References:

- https://stackoverflow.com/questions/29372626/sqlexception-protocol-violation-in-oracle

- https://stackoverflow.com/questions/18227868/protocol-violation-oracle-jdbc-driver-issue?rq=1

#### Various SQLExceptions can occur after an OutOfMemoryError occurs during data migration.

During a large amount of data migration, there are several cases in which there are multiple SQLException related to fetch or bind in Oracle. In this case, if only one of the tables is successful in table mode, the error caused by OOM can be suspected.

Example) 

Caused by: java.sql.SQLException: Fail to convert to internal representation
at oracle.jdbc.driver.CharCommonAccessor.getBigDecimal(CharCommonAccessor.java:414)

Invalid column type: getCLOB not implemented for class oracle.jdbc.driver.T4CVarcharAccessor

##### Cause

Various malfunctions are possible after the OOM occurs inside the Oracle JDBC driver.

##### Solution

Refer to OutOfMemoryError in the Common section.

#### NullPointerException may occur during the build phase.

When the source database is Oracle 9i, 10, the following NullPointerException may occur during the build phase due to Oracle JDBC driver compatibility error:

Fail to retrieve Source DDL: java.lang.NullPointerException
at oracle.jdbc.driver.T4C8Oall.getNumRows(T4C8Oall.java:1046)
at oracle.jdbc.driver.T4CPreparedStatement.executeForRows(T4CPreparedStatement.java:1047)
at oracle.jdbc.driver.OracleStatement.executeMaybeDescribe(OracleStatement.java:1207)
at oracle.jdbc.driver.OracleStatement.doExecuteWithTimeout(OracleStatement.java:1296)
at oracle.jdbc.driver.OraclePreparedStatement.executeInternal(OraclePreparedStatement.java:3608)
at oracle.jdbc.driver.OraclePreparedStatement.executeQuery(OraclePreparedStatement.java:3652)
at oracle.jdbc.driver.OraclePreparedStatementWrapper.executeQuery(OraclePreparedStatementWrapper.java:1207)
at com.altibase.migLib.meta.SrcDbMeta_Oracle_9_0_0_0.getSrcDdlDbmsMetaData(SrcDbMeta_Oracle_9_0_0_0.java:2251)

##### Cause

Oracle JDBC Driver compatibility issues

##### Solution

Replace the JDBC driver file for Oracle in MigrationCenter with the JDBC driver file for your Oracle DBMS.

### MS-SQL

#### When "Test" button is pressed during connection information registration, connection is failed with error message

##### Cause

During registering MS-SQL connection information, when "Test" button is clicked, the following error message may be displayed:

- “Migration Center can support MS-SQL user who has a single schema only.”

- “User doesn't have appropriate schema in target database.”

Registration is not allowed because the schema relationship of the user of the connection information is incompatible with Altibase. 

MS-SQL has a different relationship between users and schemas from that of Altibase. Altibase has a structure that a single schema is assigned to each user and the database object is dependent on the schema. On the other hand, in MS-SQL, a single user is allowed to have from zero to multiple schemas, and each schema has its own database objects. the Migration Center allows connection information registration only if the MS-SQL user owns only one schema.

Therefore, if an MS-SQL user has no schema or multiple schemas, it is unable to register a connection information with that user due to schema structure difference.

##### Solution

Let the user of the connection information have only one schema.

#### When the "Test" button is pressed during connection information registration, the program stops.

##### Cause

It is presumably due to a conflict between the JVM installed on the appliance and the JVM embedded in the Migration Center. This problem may be shown up when the running operating system is Windows and a Java Runtime Environment (JRE) is installed before installing a Migration Center.

##### Solution

You should open the executable file migcenter.bat with an editor and change the value of the environment variable JAVA_HOME to the location of the JRE already installed on the machine. The JRE should be changed to Java SE 8.0 or later.

#### The error message 'Unable to insert (or update) NULL into NOT NULL column.' is printed and data migration is failed.

##### Cause

This is because the difference between databases on how to handle zero-length string in column with not null constraint. MS-SQL allows inserting a zero-length string into a table column that is subject to a NOT NULL constraint. In Altibase, a zero-length string means NULL, so it does not allow inserting a zero-length string into a table column that is subject to a NOT NULL.

##### Solution

Reconcile phase - In DDL Editing, delete the NOT NULL constraint from the Destination DDL of the corresponding table and click the "Save" button to save it.

#### Migration of duplicate foreign key is failed.

##### Cause

MS-SQL allows duplicate foreign keys while Altibase does not. So, only one foreign key will be migrated.

##### Solution

In the “Missing” tab of the report created after the Run step, you can see the foreign keys that failed to escalate.

#### The error message 'The server selected protocol version TLS10 is not accepted by client preferences' is printed and connection fails.

##### Cause

The issue occurs because the Java Runtime Environment (JRE) used to run the Migration Center has been updated to default to TLS 1.2 or later, while the MS-SQL server does not support the newer TLS versions.

##### Solution 

To enable the use of older TLS versions, remove TLSv1 and TLSv1.1 from the jdk.tls.disabledAlgorithms in the java.security file.

The path to the java.security file depends on the Java version:

- Java 10 and earlier: `$JAVA_HOME/jre/lib/security`
- Java 11 and later: `$JAVA_HOME/conf/security`

~~~java
//jdk.tls.disabledAlgorithms=SSLv3, TLSv1, TLSv1.1, RC4, DES, MD5withRSA, 
jdk.tls.disabledAlgorithms=SSLv3, RC4, DES, MD5withRSA, 
~~~

If using TLS 1.2 or later is mandatory, refer to [KB3135244 - TLS 1.2 support for Microsoft SQL Server](https://support.microsoft.com/en-us/topic/kb3135244-tls-1-2-support-for-microsoft-sql-server-e4472ef8-90a9-13c1-e4d8-44aad198cdbe) to update Windows, MS-SQL server, and the MS-SQL JDBC driver file.

#### Using a JDBC driver that is not compatible with the Java version running the Migration center may result in the following error: "Unable to connect to DB. javax/xml/bind/DatatypeConverter."

Running the Migration Center on Java 11 or later while using a JDBC driver designed for JRE 10 or earlier may result in the following error: *"Unable to connect to DB. javax/xml/bind/DatatypeConverter."*

##### Cause

This error occurs because the JDBC driver designed for JRE 10 or earlier references the javax.xml.bind module, which has been removed in Java 11 and later versions.

##### Solution

Use a JDBC driver file that is compatible with the Java version running the Migration Center.

Example) mssql-jdbc-7.2.2.**jre11**.jar

### Altibase

#### When migrating Altibase version 5.1.5 or lower, the characters are broken.

##### Cause

Altibase version 5.1.5 or lower does not support globalization, so JDBC does not know how to handle the character set of the database.

##### Solution

You must include the character set value (for example, KSC5601) set in the destination database in the encoding option of the corresponding database connection information in the Migration Center. Here's how to check the Altibase character set.

- Version 4.3.9 \~ 5.1.5:  
  SELECT VALUE1 FROM V\$PROPERTY WHERE NAME = 'NLS_USE';

- Version 5.3.3 or later:  
  SELECT NLS_CHARACTERSET FROM V\$NLS_PARAMETERS;

#### In the Reconcile step, "Tablespace to Tablespace Mapping" does not show a specific tablespace.

##### Cause

The user in the connection information at the Migration Center does not have privilege to access to the tablespace.

##### Solution

Grant the Altibase user access to the tablespace.

#### When migrating Altibase version 4.5.1.0 or earlier, it fails to retrieve information of a column whose data type is BLOB, byte, or nibble.

##### Cause

The Altibase JDBC driver of the corresponding version cannot return the BLOB, byte, or nibble data type to UNKOWN to know the column data type.

##### Solution

Tables with BLOB, byte, and nibble data types are migrated using aexport and iloader.

#### Some data of bit, varbit, and nibble data types migrated from Altibase 6.1.1 and earlier versions do not match the original database.

##### Cause

When the Altibase 6.1.1 or lower JDBC driver inserts bit, varbit, or nibble type data in batch execution, it fails to migrate some data normally.

##### Solution

Open the project, click the menu Migration-Migration Option, select 'No' for Batch Execution, and perform data migration.

### Informix

#### Java.sql.SQLException: Encoding or code set not supported in Informix JDBC Driver during data migration."

The following SQLException occurred while fetching from Informix during data migration. This is an exception that occurs when retrieving this value when a byte of a multi-byte character is input to the Informix DB while being cut off.

Example) 

java.sql.SQLException: Encoding or code set not supported.
at com.informix.util.IfxErrMsg.getSQLException(IfxErrMsg.java:412)
at com.informix.jdbc.IfxChar.fromIfx(IfxChar.java:235)
at com.informix.jdbc.IfxRowColumn.a(IfxRowColumn.java:380)
at com.informix.jdbc.IfxRowColumn.a(IfxRowColumn.java:282)
at com.informix.jdbc.IfxSqli.a(IfxSqli.java:4657)
at com.informix.jdbc.IfxResultSet.a(IfxResultSet.java:666)
at com.informix.jdbc.IfxResultSet.b(IfxResultSet.java:638)
at com.informix.jdbc.IfxResultSet.getString(IfxResultSet.java:724)
at com.altibase.migLib.run.databinder.DataBinder.getValuesFromSrc(DataBinder.java:445)

##### Cause

If an Informix DB is entered with bytes of multi-byte characters truncated, an exception occurs when retrieving this value.

##### Solution

Add IFX_USE_STRENC = true to Informix connection properties.

##### References:

https://m.blog.naver.com/PostView.nhn?blogId=jangkeunna&logNo=70146227929&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F

### MySQL

#### When the data type of a table column is VARCHAR or CHAR with a size of 1 or 2, data of size 1 is migrated to null.

##### Cause

If you find that the MySQL JDBC driver returns a null value when importing data with a size of 1, it is probably a MySQL JDBC driver issue.

##### Solution

You need to replace the driver with version 5.0.8 of MySQL Connector / J (link). If the same happens in the retry, cancel Batch Execution by following the procedure below.

1. Open the project.

2. Click the menu "Migration" -> "Migration Options".

3. Change the value of "Batch Execution" to "No".

#### Data types CHAR and VARCHAR are changed to NCHAR and NVARCHAR.

##### Cause

MySQL does not support NCHAR or NVARCHAR data types. Instead, it allows for user to specify national character set as an attribute of the CHAR or VARCHAR data type column. The Migration Center converts national character set specified CHAR or VARCHAR column to NCHAR and NVARCHAR column in order to migrate data successfully.

##### Solution

This phenomenon is normal.

###### Note:

In the default DataType Map of Migration Center, MySQL's NVARCHAR is mapped to NVARCHAR (10666) in Altibase. If the number of bytes per character in the national character set between MySQL and Altibase is different, performing the escalation without consideration of this may cause the schema not to be generated beyond the limit byte count. To avoid this situation, the Migration Center basically fixed the size of the NVARCHAR.

If the data size of the corresponding table column is not large, you can transfer the size of the source database as it is by following the procedure below.

1. Reconcile step: Click on "NVARCHAR" row in "Data Type Mapping" 
2. Click the "Change" button 
3. Select 'NVARCHAR' as Destination DB Data Type, leave Precision blank, and save

### TimesTen

#### When registering connection information, when clicking "Test" button, error message 'Problems with loading native library / missing methods: no ttJdbcCS in java.library.path' is output.

##### Cause

This message shows up when the TimesTen Type 2 JDBC is failed to load the native library.

##### Solution

Install the TimesTen client package on the machine where you want to run the Migration Center, then rerun the Migration Center. If the target operating system is Linux, users have to add the lib directory path of the client package to LD_LIBRARY_PATH environment variable after installing the client package.

#### When I click "Test" button while registering connection information, I get error message 'Problems with loading native library / missing method: ~\bin\ttJdbcCS1122.dll: Cannot load AMD 64-bit.dll on a IA 32-bit platrom'.

##### Cause

The error message shows up when incompatible bit-version, 32-bit or 64-bit, between TimesTen client package and the JRE the Migration Center is using.

##### Solution

Open the executable file (migcenter.bat or migcenter.sh) with an editor and change the value of the environment variable JAVA_HOME. Specify the JAVA_HOME path to the JRE that matches the number of bits in the installed TimesTen client package, then rerun the Migration Center.

#### When registering connection information, when clicking "Test" button, error message 'Data source name not found and no default driver specified' is displayed.

##### Cause

This message is output when the DSN value entered in the connection information is invalid.

##### Solution

Enter the DSN registered in the server into the DSN field.
