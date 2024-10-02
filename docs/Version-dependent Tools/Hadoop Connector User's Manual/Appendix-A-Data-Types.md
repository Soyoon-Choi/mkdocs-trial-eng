# Appendix A: Data Types

This appendix describes data types supported by Altibase Hadoop Connector.

### Supported Data Type

The following is the table that shows the conversion of data type between Sqoop and Altibase when importing and exporting using the Altibase Hadoop Connector. Also it shows whether it supports for import or export depending on the data types.

| Altibase Data Type | Sqoop Data Type                | Import | Export |
| ------------------ | ------------------------------ | ------ | ------ |
| CHAR               | String                         | O      | O      |
| VARCHAR            | String                         | O      | O      |
| NCHAR              | String                         | O      | O      |
| NVARCHAR           | String                         | O      | O      |
| INTEGER            | Integer                        | O      | O      |
| BIGINT             | Long                           | O      | O      |
| SMALLINT           | Integer                        | O      | O      |
| NUMBER             | Double                         | O      | O      |
| NUMERIC            | java.math.BigDecimal           | O      | O      |
| DECIMAL            | java.math.BigDecimal           | O      | O      |
| FLOAT              | Double                         | O      | O      |
| DOUBLE             | Double                         | O      | O      |
| REAL               | Float                          | O      | O      |
| DATE               | java.sql.Timestamp             | O      | O      |
| BLOB               | com.cloudera.sqoop.lib.BlobRef | O      | X      |
| CLOB               | com.cloudera.sqoop.lib.ClobRef | O      | X      |

> Nope: BLOB and CLOB data types are supported only for import in Sqoop so Altibase Hadoop Connector also supports import as well. The export for BLOB and CLOB data types is to be supported soon.
