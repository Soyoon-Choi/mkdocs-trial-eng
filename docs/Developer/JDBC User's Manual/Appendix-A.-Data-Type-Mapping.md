# Appendix A. Data Type Mapping

This appendix lists the compatibility between Altibase data types and standard JDBC data types/Java data types.

### Data Type Mapping

The following table shows the basic mapping relationship between JDBC data types, Altibase JDBC data types and Java language types.

| JDBC Type     | Altibase Type | Java Type  |
| ------------- | ------------- | ---------- |
| CHAR          | CHAR          | String     |
| VARCHAR       | VARCHAR       | String     |
| LONGVARCHAR   | VARCHAR       | String     |
| NUMERIC       | NUMERIC       | BigDecimal |
| DECIMAL       | NUMERIC       | BigDecimal |
| BIT           | VARBIT        | BitSet     |
| BOOLEAN       | \-            | \-         |
| TINYINT       | SMALLINT      | Short      |
| SMALLINT      | SMALLINT      | Short      |
| INTEGER       | INTEGER       | Integer    |
| BIGINT        | BIGINT        | Long       |
| REAL          | REAL          | Float      |
| FLOAT         | FLOAT         | BigDecimal |
| DOUBLE        | DOUBLE        | Double     |
| BINARY        | BYTE          | byte[]     |
| VARBINARY     | BLOB          | Blob       |
| LONGVARBINARY | BLOB          | Blob       |
| DATE          | DATE          | Timestamp  |
| TIME          | DATE          | Timestamp  |
| TIMESTAMP     | DATE          | Timestamp  |
| CLOB          | CLOB          | Clob       |
| BLOB          | BLOB          | Blob       |
| ARRAY         | \-            | \-         |
| DISTINCT      | \-            | \-         |
| STRUCT        | \-            | \-         |
| REF           | \-            | \-         |
| DATALINK      | \-            | \-         |
| JAVA_OBJECT   | \-            | \-         |
| NULL          | \-            | null       |
| \-            | GEOMETRY      | byte[]     |

### Converting Java Data Types to Database Data Types

The following table shows the database data types available for conversion for each object when setting an object for a parameter with the setObject method.

|                    | SMALLINT | INTEGER | BIGINT | REAL | FLOAT | DOUBLE | DECIMAL/NUMERIC | BIT | CHAR | VARCHAR/LONGVARCHAR | BINARY | VARBINARY/LONGVARBINARY | DATE | TIME | TIMESTAMP | BLOB | CLOB |
|--------------------|----------|---------|--------|------|-------|--------|-----------------|-----|------|---------------------|--------|-------------------------|------|------|-----------|------|------|
| Array              |          |         |        |      |       |        |                 |     |      |                     |        |                         |      |      |           |      |      |
| Blob               |          |         |        |      |       |        |                 |     |      |                     |        |                         |      |      |           | ○    |      |
| Boolean            | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○    | ○                   |        |                         |      |      |           |      |      |
| byte[]             |          |         |        |      |       |        |                 |     | ○    | ○                   | ○      | ○                       |      |      |           | ○    |      |
| char[]             | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○    | ○                   | ○      | ○                       | ○    | ○    | ○         |      | ○    |
| Clob               |          |         |        |      |       |        |                 |     |      |                     |        |                         |      |      |           |      | ○    |
| Double             | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○    | ○                   |        |                         |      |      |           |      |      |
| Float              | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○    | ○                   |        |                         |      |      |           |      |      |
| Integer            | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○    | ○                   |        |                         |      |      |           |      |      |
| Java class         |          |         |        |      |       |        |                 |     |      |                     |        |                         |      |      |           |      |      |
| BigDecimal         | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○    | ○                   |        |                         |      |      |           |      |      |
| java.net.URL       |          |         |        |      |       |        |                 |     |      |                     |        |                         |      |      |           |      |      |
| java.sql.Date      |          |         |        |      |       |        |                 |     | ○    | ○                   |        |                         | ○    | ○    | ○         |      |      |
| java.sql.Time      |          |         |        |      |       |        |                 |     | ○    | ○                   |        |                         | ○    | ○    | ○         |      |      |
| java.sql.Timestamp |          |         |        |      |       |        |                 |     | ○    | ○                   |        |                         | ○    | ○    | ○         |      |      |
| java.util.BitSet   |          |         |        |      |       |        |                 | ○   |      |                     |        |                         |      |      |           |      |      |
| Long               | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○    | ○                   |        |                         |      |      |           |      |      |
| Ref                |          |         |        |      |       |        |                 |     |      |                     |        |                         |      |      |           |      |      |
| Short              |          | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○    | ○                   |        |                         |      |      |           |      |      |
| String             | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○    | ○                   | ○      | ○                       | ○    | ○    | ○         |      |      |
| Struct             |          |         |        |      |       |        |                 |     |      |                     |        |                         |      |      |           |      |      |
| InputStream        |          |         |        |      |       |        |                 |     |      |                     |        |                         |      |      |           | ○    |      |
| Reader             |          |         |        |      |       |        |                 |     |      |                     |        |                         |      |      |           |      | ○    |

### Converting Database Data Types to Java Data Types

The following table shows whether or not conversion is possible for each database data type with the getXXX method.

|                    | SMALLINT | INTEGER | BIGINT | REAL | FLOAT | DOUBLE | DECIMAL/NUMERIC | BIT | CHAR/VARCHAR | LONGVARCHAR | BINARY | VARBINARY/LONGVARBINARY | DATE | TIME | TIMESTAMP | CLOB | BLOB |
|--------------------|----------|---------|--------|------|-------|--------|-----------------|-----|--------------|-------------|--------|-------------------------|------|------|-----------|------|------|
| getArray           |          |         |        |      |       |        |                 |     |              |             |        |                         |      |      |           |      |      |
| getAsciiStream     | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         | ○    | ○    | ○         |      |      |
| getBigDecimal      | ○        | ○       | ○      | ○    | ○     | ○      | ○               |     | ○            | ○           |        |                         |      |      |           |      |      |
| getBinaryStream    | ○        |         | ○      | ○    | ○     | ○      | ○               | ○   |              |             | ○      |                         | ○    | ○    | ○         |      |      |
| getBlob            |          |         |        |      |       |        |                 |     |              |             |        | ○                       |      |      |           | ○    |      |
| getBoolean         | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         |      |      |           |      |      |
| getByte            | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         |      |      |           |      |      |
| getBytes           | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   |              |             | ○      |                         | ○    | ○    | ○         |      |      |
| getCharacterStream | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         | ○    | ○    | ○         |      |      |
| getClob            |          |         |        |      |       |        |                 |     |              |             |        |                         |      |      |           | ○    |      |
| getDate            |          |         |        |      |       |        |                 |     | ○            | ○           |        |                         | ○    | ○    | ○         |      |      |
| getDouble          | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         |      |      |           |      |      |
| getFloat           | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         |      |      |           |      |      |
| getInt             | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         |      |      |           |      |      |
| getLong            | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         |      |      |           |      |      |
| getObject          | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      | ○                       | ○    | ○    | ○         | ○    | ○    |
| getRef             |          |         |        |      |       |        |                 |     |              |             |        |                         |      |      |           |      |      |
| getShort           | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         |      |      |           |      |      |
| getString          | ○        | ○       | ○      | ○    | ○     | ○      | ○               | ○   | ○            | ○           | ○      |                         | ○    | ○    | ○         |      |      |
| getTime            |          |         |        |      |       |        |                 |     | ○            | ○           |        |                         | ○    | ○    | ○         |      |      |
| getTimestamp       |          |         |        |      |       |        |                 |     | ○            | ○           |        |                         | ○    | ○    | ○         |      |      |
