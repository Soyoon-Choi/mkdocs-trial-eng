# Appendix A. Properties and Data Dictionary

This appendix describes the various properties and data dictionaries used in connection with the database link.

### Data Dictionary related to Database Link

The following meta tables and performance views show the current status of database links. For more detailed information, please refer to *General Reference.*

#### Meta Table

-   SYS_DATABASE_LINKS\_

#### Performance Views

-   V\$DBLINK_ALTILINKER_STATUS

-   V\$DBLINK_DATABASE_LINK_INFO

-   V\$DBLINK_GLOBAL_TRANSACTION_INFO

-   V\$DBLINK_LINKER_CONTROL_SESSION_INFO

-   V\$DBLINK_LINKER_DATA_SESSION_INFO

-   V\$DBLINK_LINKER_SESSION_INFO

-   V\$DBLINK_NOTIFIER_TRANSACTION_INFO

-   V\$DBLINK_REMOTE_STATEMENT_INFO

-   V\$DBLINK_REMOTE_TRANSACTION_INFO

### Property Files

In order to use Altibase Database Link, altibase.properties, the properties file of the Altibase server, and dblink.conf, the properties file of AltiLinker, must be changed to its purpose.

#### altibase.properties

The following properties are related to database links. For further information on properties, please refer to *General Reference.* 

-   AUTO_REMOTE_EXEC

-   DBLINK_ENABLE

-   DBLINK_GLOBAL_TRANSACTION_LEVEL

-   DBLINK_RECOVERY_MAX_LOGFILE

-   DBLINK_REMOTE_STATEMENT_AUTOCOMMIT

-   DBLINK_REMOTE_TABLE_BUFFER_SIZE

-   DBLINK_DATA_BUFFER_BLOCK_SIZE

-   DBLINK_DATA_BUFFER_BLOCK_COUNT

-   DBLINK_DATA_BUFFER_ALLOC_RATIO

-   DBLINK_ALTILINKER_CONNECT_TIMEOUT

#### dblink.conf

The dblink.conf file is the properties file for AltiLinker. The following properties can be specified.

-   ALTILINKER_ENABLE

-   ALTILINKER_PORT_NO

-   ALTILINKER_RECEIVE_TIMEOUT

-   ALTILINKER_REMOTE_NODE_RECEIVE_TIMEOUT

-   ALTILINKER_QUERY_TIMEOUT

-   ALTILINKER_NON_QUERY_TIMEOUT

-   ALTILINKER_THREAD_COUNT

-   ALTILINKER_THREAD_SLEEP_TIME

-   ALTILINKER_REMOTE_NODE_SESSION_COUNT

-   ALTILINKER_TRACE_LOG_DIR

-   ALTILINKER_TRACE_LOG_FILE_SIZE

-   ALTILINKER_TRACE_LOGGING_LEVEL

-   ALTILINKER_JVM_BIT_DATA_MODEL_VALUE

-   ALTILINKER_JVM_MEMORY_POOL_INIT_SIZE

-   ALTILINKER_JVM_MEMORY_POOL_MAX_SIZE

-   TARGETS/NAME

-   TARGETS/JDBC_DRIVER

-   TARGETS/CONNECTION_URL

-   TARGETS/USER

-   TARGETS/PASSWORD

-   TARGETS/XADATASOURCE_CLASS_NAME

-   TARGETS/XADATASOURCE_URL_SETTER_NAME

The following is an example of the dblink.conf file.

```
ALTILINKER_ENABLE = 1
ALTILINKER_PORT_NO = 23238
  
ALTILINKER_RECEIVE_TIMEOUT = 100
ALTILINKER_REMOTE_NODE_RECEIVE_TIMEOUT = 100
  
ALTILINKER_QUERY_TIMEOUT = 10
ALTILINKER_NON_QUERY_TIMEOUT = 20
  
ALTILINKER_THREAD_COUNT = 10
ALTILINKER_THREAD_SLEEP_TIME  = 200
ALTILINKER_REMOTE_NODE_SESSION_COUNT = 100
  
ALTILINKER_TRACE_LOG_DIR = "?/trc"
ALTILINKER_TRACE_LOG_FILE_SIZE = 30
ALTILINKER_TRACE_LOG_FILE_COUNT = 9
ALTILINKER_TRACE_LOGGING_LEVEL =  3
  
ALTILINKER_JVM_MEMORY_POOL_INIT_SIZE = 128
ALTILINKER_JVM_MEMORY_POOL_MAX_SIZE = 512
ALTILINKER_JVM_BIT_DATA_MODEL_VALUE = 1

  
TARGETS = (
(
    NAME = "ora1"
    JDBC_DRIVER = "/home1/applys/work/natc/TC/Server/dk/RemoteServer/JdbcDrivers/ojdbc1
    CONNECTION_URL = "jdbc:oracle:thin:@dbdm.altibase.in:1521:ORCL"
    USER = "new_dblink"
    PASSWORD = "new_dblink"
),
(
    NAME = "alti2"
    JDBC_DRIVER =  "/home/user/altibase_home/lib/Altibase.jar"
    CONNECTION_URL = "jdbc:Altibase://127.0.0.1:20600/mydb" XADATASOURCE_CLASS_NAME= "oracle.jdbc.xa.OracleXADataSource XADATASOURCE_URL_SETTER_NAME = "setURL"
)
)
```

More explanations for each property are provided in the next section. 

### AltiLinker Properties

This section describes the AltiLinker properties set in the dblink.conf file.

#### ALTILINKER_ENABLE

##### Default Value

0

##### Range

[0, 1]

##### Description

This property decides whether or not to activate the AltiLinker process. This value must be set to 1 for the use of database links. When the value is 0, the AltiLinker process does not start.

#### ALTILINKER_JVM_BIT_DATA_MODEL_VALUE

##### Default Value

1

##### Range

[0, 1]

##### Description

This is used to specify JVM bit for AltiLinker on JVM (Java Virtual Machine).

-   0: 32 bit

-   1: 64 bit

#### ALTILINKER_JVM_MEMORY_POOL_INIT_SIZE (Unit: bytes) 

##### Default Value

128 MBytes

##### Range

[128MB, 4096MB]

##### Description

This property specifies the initial size, in bytes, of the memory pool allocated for the AltiLinker on the JVM (Java Virtual Machine).

#### ALTILINKER_JVM_MEMORY_POOL_MAX_SIZE (Unit: bytes) 

##### Default Value

4096 MBytes

##### Range

[512MB, 32768MB]

##### Description

This property specifies the maximum size, in bytes, of the memory pool allocated for the AltiLinker on the JVM.

#### ALTILINKER_NON_QUERY_TIMEOUT (Unit: seconds)

##### Default Value

60

##### Range

[0, 2<sup>32</sup>-1]

##### Description

If the execution time of DML statements (excluding the SELECT statement) or DDL statements exceeds the number of seconds specified here, execution of the statements are canceled.

#### ALTILINKER_PORT_NO 

##### Default Value

0

##### Range

[1024, 65535]

##### Description

This property specifies the TCP port number at which AltiLinker listens.

#### ALTILINKER_QUERY_TIMEOUT (Unit: seconds)

##### Default Value

60

##### Range

[0, 2<sup>32</sup>-1]

##### Description

If the query execution time on the remote server exceeds the number of seconds specified here, execution of the query is canceled.

#### ALTILINKER_RECEIVE_TIMEOUT (Unit: seconds)

##### Default Value

5

##### Range

[0, 2<sup>32</sup>-1]

##### Description

This property specifies the maximum wait time, in seconds, when an Altibase server is exchanging data with AltiLinker.

#### ALTILINKER_REMOTE_NODE_RECEIVE_TIMEOUT (Unit: seconds)

##### Default Value

30

##### Range

[0, 2<sup>32</sup>-1]

##### Description

This property specifies the maximum wait time, in seconds, of executing PREPARE, DCL statements and setting auto-commit mode, other than executing SELECT, DML and DDL statements. 

#### ALTILINKER_REMOTE_NODE_SESSION_COUNT 

##### Default Value

64

##### Range

[1, 128]

##### Description

This property specifies the maximum number of sessions created when the AltiLinker process connects to the remote server. The maximum number of Linker Data Sessions is the value of this property -1(the number of Linker Control Sessions).

#### ALTILINKER_THREAD_COUNT 

##### Default Value

16

##### Range

[2, 2<sup>31</sup>-1]

##### Description

This property specifies the number of threads to execute the SQL statements on the remote server by AltiLinker.

#### ALTILINKER_THREAD_SLEEP_TIME (Unit: microseconds) 

##### Default Value

200

##### Range

[1, 2<sup>32</sup>-1]

##### Description

This property specifies the waiting time, in microseconds, for threads within the AltiLinker process, when the threads are free of operations.

#### ALTILINKER_TRACE_LOG_DIR

##### Default Value

$ALTIBASE_HOME/trc

##### Range

None

##### Description

This property specifies the location of the log file for the AltiLinker process to write trace logs.

#### ALTILINKER_TRACE_LOG_FILE_COUNT

##### Default Value

10

##### Range

[1, 100]

##### Description

This property specifies the maximum number of log files for the AltiLinker process to write trace logs.

#### ALTILINKER_TRACE_LOG_FILE_SIZE (Unit: bytes) 

##### Default Value

10 MBytes

##### Range

[1MB, 2<sup>32</sup>-1]

##### Description

This property specifies the size, in bytes, of the log file for the AltiLinker process to write trace logs.

#### ALTILINKER_TRACE_LOGGING_LEVEL 

##### Default Value

4

##### Range

[0, 6]

##### Description

This property specifies the level for the AltiLinker process to write trace logs. The following values can be specified. 

-   0: does not write logs

-   1: FATAL

-   2: ERROR

-   3: WARNING

-   4: INFO

-   5: DEBUG

-   6: TRACE

#### TARGETS/CONNECTION_URL

##### Default Value

None

##### Range

None

##### Description

This property specifies the connection URL for the remote database server.

#### TARGETS/JDBC_DRIVER

##### Default Value

None

##### Range

None

##### Description

This property specifies the JDBC Driver path for the remote database server. 

#### TARGETS/JDBC_DRIVER_CLASS_NAME

##### Default Value

None

##### Range

None

##### Description

CLASS NAME of the JDBC driver for the remote database server. If no value is specified, the class that implements the java.sql.Driver interface is loaded.

#### TARGETS/NAME

##### Default Value

None

##### Range

None

##### Description

This property specifies the name to be granted to the remote server.

#### TARGETS/PASSWORD

##### Default Value

None

##### Range

None

##### Description

This property specifies the user password to connect to the remote database server.

#### TARGETS/USER

##### Default Value

None

##### Range

None

##### Description

This property specifies the user name to connect to the remote database server.

#### TARGETS/ XADATASOURCE_CLASS_NAME

##### Default Value

None

##### Range

None

##### Description

This property specifies the XADataSource class name.

#### TARGETS/XADATASOURCE_URL_SETTER_NAME

##### Default Value

None

##### Range

None

##### Description

This function specifies the URL of XADataSource.

