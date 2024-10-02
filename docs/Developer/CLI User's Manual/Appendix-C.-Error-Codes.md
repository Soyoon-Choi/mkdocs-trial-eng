# Appendix C. Error Codes

SQLError returns SQLSTATE values as defined in the X/Open and SQL Access Group SQL CAE specification (1992), and ODBC specifications. SQLSTATE is a five-byte alphanumeric string. This Appendix describes SQLSTATE values for Altibase CLI and ODBC.

### SQLSTATE

The following table displays SQLSTATE values that can be returned by SQLError of the Altibase CLI driver.

| SQLSTATE | Error                                                        | Can be returned from                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 01004    | Data truncation (if the size of the value to be returned is larger than the size of the buffer) | SQLDescribeCol SQLFetch SQLGetData                           |
| 07006    | Restricted data type attribute violation                     | SQLBindParameter SQLExecute SQLFetch                         |
| 07009    | Invalid descriptor index                                     | SQLBindCol SQLBindParameter SQLColAttribute SQLDescribeCol SQLDescribeParam SQLGetData |

> \* For SQLSTATE 08001, 08002, 08003, and 08S01, refer to the next table.
>

| SQLSTATE | Error                                                        | Can be returned from                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| HY000    | General error                                                | SQLAllocStmt SQLAllocConnect SQLBindCol SQLBindParameter SQLColAttribute SQLColumns SQLConnect SQLDescribeCol SQLDisconnect SQLDriverConnect SQLEndTran SQLExecDirect SQLExecute SQLFetch SQLFreeHandle SQLFreeStmt SQLGetData SQLNumParams SQLNumResultCols SQLPrepare SQLPrimaryKeys SQLProcedureColumns SQLProcedures SQLRowCount SQLSetAttribute SQLSetConnectAttr SQLSetEnvAttr SQLStatistics SQLTables |
| HY001    | Memory allocation error (Cannot allocate the requested memory for the ODBC driver to execute and complete the function.) | SQLAllocConnect SQLAllocStmt SQLBindCol SQLBindParameter SQLConnect SQLDriverConnect SQLExecDirect SQLGetTypeInfo SQLPrepare |
| HY003    | Invalid application buffer type (the cType argument value is not a valid C data type.) | SQLBindCol SQLBindParameter                                  |
| HY009    | Invalid argument (null pointer)                              | SQLAllocConnect SQLAllocStmt SQLBindParameter SQLExecDirect SQLForeignKeys SQLPrimaryKeys SQLProcedureColumns SQLProcedures SQLSpecialColumns SQLStatistics SQLTablePrivileges |
| HY010    | Function sequence error                                      | SQLAllocStmt SQLDescribeParam SQLGetData                     |
| HY090    | Invalid character string or buffer length                    | SQLBindParameter SQLDescribeCol SQLExecute SQLForeignKeys SQLGetData SQLGetStmtAttr SQLTablePrivileges |
| HY092    | Invalid attribute or option                                  | SQLGetStmtAttr                                               |
| HY105    | Invalid parameter type                                       | SQLBindParameter                                             |
| HYC00    | Unsupported attribute                                        | SQLGetConnectAttr SQLGetStmtAttr                             |

#### Database Connection-related Error Codes			

| SQLSTATE | Code    | Error                                                        | Can be returned from        |
| -------- | ------- | ------------------------------------------------------------ | --------------------------- |
| HY000    | 0x51001 | The character set does not exist.                            | SQLConnect SQLDriverConnect |
|          | 0x5003b | The communication buffer is insufficient. (It exceeded the length of the communication buffer.) | SQLExecute                  |
| HY001    | 0x5104A | Memory allocation error (Cannot allocate the memory requested for the driver to execute the function and complete execution) | SQLConnect SQLDriverConnect |
| 08001    | 0x50032 | The driver cannot set the connection to the database.        | SQLConnect SQLDriverConnect |

#### Network-related Error Codes

| SQLSTATE | Code    | Error                                                        | Can be returned from                                         |
| -------- | ------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 08002    | 0x51035 | The *dbc* is already connected to the database.              | SQLConnect SQLDriverConnect                                  |
| 08003    | 0x51036 | *stmt* is unconnected or disconnected.                       | SQLExecDirect SQLExecute SQLPrepare                          |
| 08S01    | 0x51043 | Communication channel error (Communication channel failure occurred before the execution of the function was complete between the ODBC driver and the database.) | SQLColumns SQLConnect SQLDriverConnect SQLExecDirect SQLExecute SQLFetch SQLForeignKeys SQLGetConnectAttr SQLPrepare SQLPrimaryKeys SQLProcedureColumns SQLProcedures SQLSetConnectAttr SQLSpecialColumns SQLStatistics SQLTablePrivileges SQLTables |

### Statement State Transition-related Errors

The following tables show how each state changes when an Altibase CLI function that uses an environment, connection or statement handle is called in a statement state. 

Altibase CLI statements have the following states.

| State | Description                                                                 |
|-------|-----------------------------------------------------------------------------|
| S0    | Unallocated statement. (The connection state must be connected connection.) |
| S1    | Allocated statement.                                                        |
| S2    | Prepared statement. (A (possibly empty) result set will be created.)        |
| S6    | Cursor positioned with SQLFetch.                                            |

The entry values in the transition table are as follows:

-   \-- : When the state remains the same after the function is executed

-   Sn : When the statement state is transitioned to a specified state

-   (IH) : Invalid Handle

-   (HY010) : Function sequence error

-   (24000) :

> Note*
>
> -   S : Success. In this case, the function returns one of the following values: SQL_SUCCESS_WITH_INFO or SQL_SUCCESS.
>   
>-   E : Error. In this case, the function returns SQL_ERROR.
> 
>-   R : Results. There is a result set when the statement is executed. (There is a possibility that the result set is an empty set.)
>   
> -   NR : No Results. There is no result set when the statement is executed.
>
> -   NF : No Data Found. The function returns SQL_NO_DATA.
>
> -   RD : Receive Done
>
> -   P : Prepared. The statement was prepared
>
> -   NP : Not Prepared. The statement was not prepared.
>

The following example shows how to view a statement state transition table for the SQLPrepare function.

<table>
   <tr>
      <th>S0 Unallocated</th>
      <th>S1Allocated</th>
      <th>S2 Prepared</th>
      <th>S6 Infetch</th>
   </tr>
   <tr>
      <td rowspan="2">(IH)</td>
       <td rowspan="2">S => S1</td>
       <td>S => S2 </td>
       <td rowspan="2">(24000)</td>
   </tr>  
   <tr> 
      <td>E => S1</td>
   </tr>
</table>
If the SQLPrepare function is called when the handle type is SQL_HANDLE_STMT and the statement state is S0, SQL_INVALID_Handle (IH) is returned. If SQLPrepare is called and successfully executed when the handle type is SQL_HANDLE_STMT and the state is S1, this state is retained. If SQLPrepare is called and successfully executed when the handle type is SQL_HANDLE_STMT and the state is S2, the statement state is transitioned to S2; if unsuccessful, the statement state remains S1 as before. If the function is called while the handle type is SQL_HANDLE_STMT and the state is S6, SQL_ERROR and SQLSTATE 24000 (Invalid Cursor State) are always returned.

**SQLAllocHandle**
<table>
   <tr>
      <th>S0 Unallocated</th>
      <th>S1Allocated</th>
      <th>S2 Prepared</th>
      <th>S6 Infetch</th>
   </tr>
   <tr>
      <td>--</td>
      <td rowspan="2">--</td>
       <td rowspan="2">--</td>  
       <td rowspan="2">--</td>
   </tr>  
   <tr> 
      <td>S1*</td>
   </tr>
</table>


>  \* When HandleType is SQL_HANDLE_STMT
>

**SQLBindCol**

| S0 Unallocated | S1 Allocated | S2 Prepared | S6 Infetch |
|----------------|--------------|-------------|------------|
| (IH)           | \--          | \--         | \--        |

**SQLBindParameter**

| S0 Unallocated | S1 Allocated | S2 Prepared | S6 Infetch |
|----------------|--------------|-------------|------------|
| (IH)           | \--          | \--         | \--        |

**SQLColumns, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns,
SQLProcedures, SQLStatistics, SQLTables**

<table>
   <tr>
      <th>S0 Unallocated</th>
      <th>S1Allocated</th>
      <th>S2 Prepared</th>
      <th>S6 Infetch</th>
   </tr>
   <tr>
      <td rowspan="2">(IH)</td>
      <td rowspan="2"> S => S6</td>
       <td >E => S1</td>  
       <td rowspan="2">(24000) </td>
   </tr>  
   <tr> 
      <td>S => S6 </td>
   </tr>
</table>


**SQLConnect**

| S0 Unallocated | S1 Allocated | S2 Prepared | S6 Infetch |
|----------------|--------------|-------------|------------|
| (Error)        | (Error)      | (Error)     | (Error)    |

**SQLDisconnect**

| S0 Unallocated | S1 Allocated | S2 Prepared | S6 Infetch |
|----------------|--------------|-------------|------------|
| \--            | \* =\> S0    | \* =\> S0   | \* =\> S0  |

> \* *When the SQLDisconnect function is called, all statements related to the connection handle are terminated. This function moves a connection from a connection state to an allocated connection state; the connection state is C4 (connected connection) before the statement state becomes S0.

**SQLDriverConnect**

Please refer to SQLConnect.

**SQLExecDirect**
<table>
   <tr>
      <th>S0 Unallocated</th>
      <th>S1Allocated</th>
      <th>S2 Prepared</th>
      <th>S6 Infetch</th>
   </tr>
   <tr>
      <td rowspan="3">(IH)</td>
      <td> S, NR => --</td>
      <td >S, NR => S1</td>  
      <td rowspan="3">(24000) </td>
   </tr>  
   <tr> 
   	  <td  rowspan="2">S, R => S6 </td>
      <td> S, R => S6  </td>
   </tr>
   <tr> 
      <td> E => S1 </td>
   </tr>
</table>


**SQLExecute**

<table>
   <tr>
      <th>S0 Unallocated</th>
      <th>S1Allocated</th>
      <th>S2 Prepared</th>
      <th>S6 Infetch</th>
   </tr>
   <tr>
      <td rowspan="3">(IH)</td>
      <td rowspan="3"> (HY010) </td>
      <td >S, NR => --</td>  
      <td rowspan="3">(24000) </td>
   </tr>  
   <tr> 
      <td> S, R => S6 </td>
   </tr>
   <tr> 
      <td> E => --  </td>
   </tr>
</table>

**SQLFetch**

<table>
   <tr>
      <th>S0 Unallocated</th>
      <th>S1Allocated</th>
      <th>S2 Prepared</th>
      <th>S6 Infetch</th>
   </tr>
   <tr>
      <td>(IH)</td>
      <td> (HY010) </td>      
      <td>(HY010) </td>
      <td>S => --</td>  
   </tr>
</table>


**SQLFreeHandle**

| S0 Unallocated | S1 Allocated | S2 Prepared | S6 Infetch |
|----------------|--------------|-------------|------------|
| \--            | (HY010)      | (HY010)     | (HY010)    |
| (IH)           | S0           | S0          | S0         |

(1) When the handleType of the first row is SQL_HANDLE_ENV or SQL_HANDLE_DBC 

(2) When the handleType of the second row is SQL_HANDLE_STMT

**SQLFreeStmt**
<table>
   <tr>
      <th>S0 Unallocated</th>
      <th>S1Allocated</th>
      <th>S2 Prepared</th>
      <th>S6 Infetch</th>
   </tr>
   <tr>
      <td rowspan="2">(IH)</td>
      <td rowspan="2"> -- </td>      
      <td rowspan="2">--</td>
      <td >NP = > S1</td>  
   </tr>  
   <tr>
       <td> P => S2</td>
   </tr>
   <tr> 
      <td> (IH)</td>
      <td>S0</td>
      <td>S0</td>
      <td>S0</td>
   </tr>
</table>
(1) When *fOption* of the first row is SQL_CLOSE 

(2) When *fOption* of the second row is SQL_DROP

**SQLGetData**

| S0 Unallocated | S1 Allocated | S2 Prepared | S6 Infetch       |
|----------------|--------------|-------------|------------------|
| (IH)           | (HY010)      | (HY010)     | S \|\| NF =\> -- |


**SQLGetTypeInfo**

Please refer to SQLColumns.

**SQLNumResultCols**

| S0 Unallocated | S1 Allocated | S2 Prepared | S6 Infetch |
|----------------|--------------|-------------|------------|
| (IH)           | (HY010)      | S =\> --    | S =\> --   |

**SQLPrepare**
<table>
   <tr>
      <th>S0 Unallocated</th>
      <th>S1Allocated</th>
      <th>S2 Prepared</th>
      <th>S6 Infetch</th>
   </tr>
   <tr>
      <td rowspan="2">(IH)</td>
      <td rowspan="2">S => -- </td>      
      <td> S => --</td>
      <td rowspan="2">(24000) </td>  
   </tr>  
   <tr>
       <td> E => S1 </td>
   </tr>
</table>

**SQLPrimaryKeys**

Please refer to SQLColumns.

**SQLProcedureColumns**

Please refer to SQLColumns.

**SQLProcedures**

Please refer to SQLColumns.

**SQLSetConnectAttr**

| S0 Unallocated | S1 Allocated | S2 Prepared | S6 Infetch |
|----------------|--------------|-------------|------------|
| \--\*          | \--          | \--         | (24000)    |

> \* When the set Attribute is a connection attribute; please refer to SQLSetStmtAttr if the set Attribute is a statement attribute.

**SQLSetEnvAttr**

| S0 Unallocated | S1 Allocated | S2 Prepared | S6 Infetch |
|----------------|--------------|-------------|------------|
| (Error)        | (Error)      | (Error)     | (Error)    |

**SQLSetStmtAttr**
<table>
   <tr>
      <th>S0 Unallocated</th>
      <th>S1Allocated</th>
      <th>S2 Prepared</th>
      <th>S6 Infetch</th>
   </tr>
   <tr>
      <td rowspan="2">(IH)</td>
      <td rowspan="2">-- </td>      
      <td> (1) => -- </td>
      <td>(1) => --  </td>  
   </tr>  
   <tr>
       <td> (2) => (Error) </td>
       <td>(2) => (24000)  </td>
   </tr>
</table>

(1) When the Attribute argument is neither SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_SIMULATE_CURSOR, SQL_ATTR_USE_BOOKMARKS, SQL_ATTR_CURSOR_SCROLLABLE, nor SQL_ATTR_CURSOR_SENSITIVITY

(2) When the Attribute argument is either SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_SIMULATE_CURSOR, SQL_ATTR_USE_BOOKMARKS, SQL_ATTR_CURSOR_SCROLLABLE, or SQL_ATTR_CURSOR_SENSITIVITY

**SQLStatistics**

Please refer to SQLColumns.

**SQLTables**

Please refer to SQLColumns.

### State Transition Table

The following table lists the major functions that affect state transition:

<table>
  <tr>
    <th><img src="media/CLI/diagonal.png"/></th>
    <th>S0
UNALLOCATED
</th>
    <th>S1
ALLOCATED
</th>
    <th>S2
PREPARED
</th>
    <th>S6
INFETCH
</th>
  </tr>
  <tr>
        <td rowspan="2">Prepare</td>
        <td rowspan="2">(IH)</td>
        <td rowspan="2">S => S1</td>
        <td>S => S2</td>
        <td rowspan="2">(24000)</td>
  </tr>
  <tr>
   		<td>E => S1</td>
   </tr>
   <tr>
        <td rowspan="3">ExecDirect</td>
        <td rowspan="3">(IH)</td>
        <td>S,NR => S1</td>
        <td>S => S2</td>
        <td rowspan="3">(24000)</td>
    </tr>
    <tr>
        <td rowspan="2">S,R => S6</td>
        <td>S,R => S6</td>
    </tr>
    <tr>
    	<td>E => S1</td>
    </tr>
    <tr>
        <td rowspan="3">Execute</td>
        <td rowspan="3">(IH)</td>
        <td rowspan="3">(HY010)</td>
        <td>S,NR => S2</td>
        <td rowspan="3">(24000)</td>
    </tr>
    <tr>
        <td>S,R => S6</td>
    </tr>
    <tr>
    	<td>E => S2</td>
    </tr>
     <tr>
        <td>Fetch</td>
        <td>(IH)</td>
        <td>(HY010)</td>
        <td>(HY010)</td>
        <td>S => S6</td>        
    </tr>
    <tr>
        <td rowspan="2">FreeStmt
(CLOSE)
</td>
        <td rowspan="2">(IH)</td>
        <td rowspan="2">S1</td>
        <td rowspan="2">S2</td>
        <td>NP => S1</td>        
    </tr>
    <tr>
        <td>P => S2</td>
    </tr>
    <tr>
    	<td>FreeStmt
(DROP)
</td>
    	<td>(IH)</td>
        <td>S0</td>
        <td>S0</td>
        <td>S0</td>
    </tr>
</table>

Cf )

\- (IH) : Invalid Handle (HY010) : Function Sequence Error

\- (24000) : Invalid Cursor State

\- S : Success E : Error except Network Error

\- R : Results NR : No Results NF : No data Found RD: Receive Done

\- P : Prepared NP : Not Prepared

