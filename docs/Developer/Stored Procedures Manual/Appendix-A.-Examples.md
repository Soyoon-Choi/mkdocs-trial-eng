# Appendix A. Examples

### Stored Procedure Examples

#### Example 1

Create a stored procedure called *dumpReplScript*, which outputs a script for creating a replication object.

The tables to be replicated are the *employees* table and the *departments* table, the local server's IP address and port number are 192.168.1.12 and 35524, and the remote server's IP address and port number are 192.168.1.60 and 25524.

On the remote server:

```
iSQL> CREATE REPLICATION rep1 WITH '192.168.1.12',35524 FROM SYS.EMPLOYEES TO SYS.EMPLOYEES, FROM SYS.DEPARTMENTS TO SYS.DEPARTMENTS;
Create success.

iSQL> ALTER REPLICATION rep1 START;
Alter success.
```

On the local server:

```
iSQL> CREATE REPLICATION rep1 WITH '192.168.1.60',25524 FROM SYS.EMPLOYEES TO SYS.EMPLOYEES, FROM SYS.DEPARTMENTS TO SYS.DEPARTMENTS;
Create success.
iSQL> ALTER REPLICATION rep1 START;
Alter success.

iSQL> create or replace procedure dumpReplScript
(p1 varchar(40))
as
cursor c1 is
select  system_.sys_replications_.replication_name,
system_.sys_replications_.host_ip,
system_.sys_replications_.port_no,
system_.SYS_REPLICATIONS_.ITEM_COUNT
from system_.sys_replications_
where system_.sys_replications_.replication_name = UPPER(P1);
r_name varchar(40);
r_ip varchar(40);
r_port varchar(20);
r_item_cnt integer;
r_local_user_name varchar(40);
r_local_table_name varchar(40);
r_remote_user_name varchar(40);
r_remote_table_name varchar(40);
cursor c2 is
select system_.SYS_REPL_ITEMS_.LOCAL_USER_NAME,
system_.SYS_REPL_ITEMS_.LOCAL_TABLE_NAME,
system_.SYS_REPL_ITEMS_.REMOTE_USER_NAME,
system_.SYS_REPL_ITEMS_.REMOTE_TABLE_NAME
from system_.sys_repl_items_
where system_.SYS_REPL_ITEMS_.replication_name = r_name;
begin
open c1;
SYSTEM_.PRINTLN('---------------------------------------');
SYSTEM_.PRINTLN('');
loop
fetch C1 into r_name, r_ip, r_port, r_item_cnt;
exit when C1%NOTFOUND;
SYSTEM_.PRINT(' CREATE REPLICATION ');
SYSTEM_.PRINT(r_name);
SYSTEM_.PRINT(' WITH ''');
SYSTEM_.PRINT(r_ip);
SYSTEM_.PRINT(''',');
SYSTEM_.PRINT(r_port);
SYSTEM_.PRINTLN(' ');
open c2;
        for i in 1 .. r_item_cnt loop
fetch c2 into r_local_user_name,
r_local_table_name,
r_remote_user_name,
r_remote_table_name;
SYSTEM_.PRINT(' FROM ');
SYSTEM_.PRINT(r_local_user_name);
SYSTEM_.PRINT('.');
SYSTEM_.PRINT(r_local_table_name);
SYSTEM_.PRINT(' TO ');
SYSTEM_.PRINT(r_remote_user_name);
SYSTEM_.PRINT('.');
SYSTEM_.PRINT(r_remote_table_name);
if i <> r_item_cnt then
SYSTEM_.PRINTLN(',');
else
SYSTEM_.PRINTLN(';');
end if;
        end loop;
close c2;
end loop;
close c1;
SYSTEM_.PRINTLN('');
SYSTEM_.PRINTLN('---------------------------------------');
end;
/
```

The following is output by the *dumpReplScript* stored procedure on the local server.

```
iSQL> exec dumpReplScript('rep1');
----------------------------------------------------------
 CREATE REPLICATION REP1 WITH '192.168.1.60',25524
 FROM SYS.DEPARTMENTS TO SYS.DEPARTMENTS,
 FROM SYS.EMPLOYEES TO SYS.EMPLOYEES;
----------------------------------------------------------
Execute success.
```



#### Example 2

Create a stored procedure called *showReplications*, which outputs the name and other information about replication objects.

```
create or replace procedure showReplications
as
cursor c1 is select system_.sys_replications_.replication_name, system_.sys_replications_.host_ip, system_.sys_replications_.port_no, decode(system_.sys_replications_.is_started,1,'Running',0,'Not Running')
from system_.sys_replications_;
r_name varchar(40);
r_ip varchar(40);
r_port varchar(20);
r_status varchar(20);
r_local_user_name varchar(40);
r_local_table_name varchar(40);
r_remote_user_name varchar(40);
r_remote_table_name varchar(40);
cursor c2 is select system_.SYS_REPL_ITEMS_.LOCAL_USER_NAME, system_.SYS_REPL_ITEMS_.LOCAL_TABLE_NAME, system_.SYS_REPL_ITEMS_.REMOTE_USER_NAME
system_.SYS_REPL_ITEMS_.REMOTE_TABLE_NAME
from system_.sys_repl_items_
where system_.SYS_REPL_ITEMS_.replication_name
= r_name;
begin
open c1;
SYSTEM_.PRINTLN('-----------------------------------------');
SYSTEM_.PRINTLN('     Replications   Infos');
SYSTEM_.PRINTLN('-----------------------------------------');
SYSTEM_.PRINTLN(' Name          Ip            Port       Status');
SYSTEM_.PRINTLN('-----------------------------------------');
SYSTEM_.PRINTLN('');
loop
fetch C1 into r_name, r_ip, r_port, r_status;
exit when C1%NOTFOUND;
SYSTEM_.PRINT(' ');
SYSTEM_.PRINT(r_name);
SYSTEM_.PRINT('              ');
SYSTEM_.PRINT(r_ip);
SYSTEM_.PRINT('        ');
SYSTEM_.PRINT(r_port);
SYSTEM_.PRINT('      ');
SYSTEM_.PRINTLN(r_status);
SYSTEM_.PRINTLN('+++++++++++++++++++++++++++++++++++++');
SYSTEM_.PRINTLN(' Local Table Name   Remote Table Name');
SYSTEM_.PRINTLN('+++++++++++++++++++++++++++++++++++++');
open c2;
loop
fetch c2 into r_local_user_name, r_local_table_name, r_remote_user_name, r_remote_table_name;
exit when C2%NOTFOUND;
SYSTEM_.PRINT('         ');
SYSTEM_.PRINT(r_local_user_name);
SYSTEM_.PRINT('.');
SYSTEM_.PRINT(r_local_table_name);
SYSTEM_.PRINT('                          ');
SYSTEM_.PRINT(r_remote_user_name);
SYSTEM_.PRINT('.');
SYSTEM_.PRINTLN(r_remote_table_name);
end loop;
close c2;
end loop;
close c1;
SYSTEM_.PRINTLN('');
SYSTEM_.PRINTLN('-----------------------------------------');
end;
/
```

The following is output by the *showReplications* stored procedure.

```
iSQL> exec showReplications;
----------------------------------------------------------
 Replication Info
----------------------------------------------------------
 Name      IP              Port        Status
----------------------------------------------------------
 REP1      192.168.1.60    25524       Running
 ++++++++++++++++++++++++++++++++++++++++++++++++++++
 Local Table Name      Remote Table Name
 ++++++++++++++++++++++++++++++++++++++++++++++++++++
 SYS.DEPARTMENTS        SYS.DEPARTMENTS
 SYS.EMPLOYEES          SYS.EMPLOYEES
----------------------------------------------------------
EXECUTE success.
```



#### Example 3

Create a stored procedure called *showTables*, which outputs the names of all of a given user's tables.

```
create or replace procedure SHOWTABLES(p1 in varchar(40))
as
cursor c1 is select SYSTEM_.SYS_TABLES_.TABLE_NAME
from SYSTEM_.SYS_TABLES_
where SYSTEM_.SYS_TABLES_.USER_ID =
(select SYSTEM_.SYS_USERS_.USER_ID
from SYSTEM_.SYS_USERS_
where SYSTEM_.SYS_USERS_.USER_NAME =
upper(p1)
AND system_.SYS_TABLES_.TABLE_TYPE = 'T');
v1 CHAR(40);     
begin
open c1;
SYSTEM_.PRINTLN('-------------------');
SYSTEM_.PRINT(p1);
SYSTEM_.PRINTLN(' Table');
SYSTEM_.PRINTLN('-------------------');
loop
fetch C1 into v1;
exit when C1%NOTFOUND;
SYSTEM_.PRINT(' ');
SYSTEM_.PRINTLN(v1);
end loop;
SYSTEM_.PRINTLN('-------------------');
close c1;
end;
/
```

The following is output by the *showTables* stored procedure.

```
iSQL> exec showTables('SYS');
-------------------
SYS Table
-------------------
 CUSTOMERS                                
 GOODS                                   
 DUMMY                                   
 ORDERS                                  
 EMPLOYEES                                
 DEPARTMENTS                              
-------------------
Execute success.
```



#### Example 4

Create a stored procedure called *showProcBody*, which outputs the contents of a desired stored procedure

```
create or replace procedure showProcBody(p1 in varchar(40))
as
cursor c1 is
    select system_.sys_proc_parse_.parse
    from system_.sys_proc_parse_
    where system_.sys_proc_parse_.proc_oid = (
    select SYSTEM_.sys_procedures_.proc_oid
    from system_.sys_procedures_
    where SYSTEM_.sys_procedures_.proc_name = upper(p1))
order by system_.sys_proc_parse_.seq_no;
v1 varchar(4000);
begin
open c1;
    SYSTEM_.PRINTLN('---------------------------------');
    system_.print(p1);
    SYSTEM_.PRINTLN(' Procedure');
    SYSTEM_.PRINTLN('---------------------------------');
    SYSTEM_.PRINTLN('');
    loop
      fetch C1 into v1;
    exit when C1%NOTFOUND;
    SYSTEM_.PRINTLN(v1);
    end loop;
  close c1;
  SYSTEM_.PRINTLN('');
  SYSTEM_.PRINTLN('---------------------------------');
end;
/
```

The following is the result of querying the SYS_PROC_PARSE_ meta table, which contains the actual text of stored procedure creation statements.

```
select system_.sys_proc_parse_.proc_oid, system_.sys_proc_parse_.parse
from system_.sys_proc_parse_
where system_.sys_proc_parse_.proc_oid = (
select SYSTEM_.sys_procedures_.proc_oid
from system_.sys_procedures_
where SYSTEM_.sys_procedures_.proc_name = upper('proc1'));
PROC_OID             
-----------------------
PARSE                                                                                                 
-----------------------------------------------------------
7695216              

create or replace procedure PROC1
(P1 in NUMBER, P2 in VARCHAR(10), P3 in DATE)
as
begin
    if P1 >  
7695216              
 0 then
        insert into T1 values (P1, P2, P3);
    end if;
end                                   
2 rows selected.
```

The following is output by the *showProcBody* stored procedure.

```
iSQL> exec showProcBody('proc1');
---------------------------------
proc1 Procedure
---------------------------------

create or replace procedure PROC1
(P1 in NUMBER, P2 in VARCHAR(10), P3 in DATE)
as
begin
    if P1 >
0 then
        insert into T1 values (P1, P2, P3);
    end if;
end

---------------------------------
Execute success.
```



#### Example 5

Create a stored procedure that uses a cursor variable. When this procedure is executed, a cursor variable is opened and used to read data via ODBC.

```
CREATE OR REPLACE TYPESET MY_TYPE
AS
  TYPE MY_CUR IS REF CURSOR;
END;
/

CREATE OR REPLACE PROCEDURE OPENCURSOR2
( P1 OUT MY_TYPE.MY_CUR, P2 IN INTEGER )
AS
BEGIN
  OPEN P1 FOR 'SELECT C1 FROM T1 WHERE C1 <= ?' USING P2;
END;
/

iSQL> EXEC OPENCURSOR2(4);
C1
--------------
1
2
3
4
4 rows selected.

/* ODBC program */
    ...
    SQLINTEGER c1;
    SQLINTEGER param1;

    /* allocate Statement handle */
    if (SQL_ERROR == SQLAllocStmt(dbc, &stmt))
    {
        printf("SQLAllocStmt error!!\n");
        return SQL_ERROR;
    }

    sprintf(query,"EXEC OPENCURSOR2(?)");

    if (SQLPrepare(stmt, (SQLCHAR *) query, SQL_NTS)== SQL_ERROR)
    {
       printf("ERROR: prepare stmt\n");
       execute_err(dbc, stmt, query);
       return SQL_ERROR;
    }

    if (SQLBindParameter(stmt, 1, SQL_PARAM_INPUT, SQL_C_SLONG,
                 SQL_INTEGER, 0, 0, &param1, 0, NULL) == SQL_ERROR)
    {
       printf("ERROR: Bind Parameter 1\n");
       execute_err(dbc, stmt, query);
       return SQL_ERROR;
    }

    param1 = 4;
    if (SQLExecute( stmt ) != SQL_SUCCESS)
    {
        execute_err(dbc, stmt, query);
        SQLFreeStmt(stmt, SQL_DROP);
        return SQL_ERROR;
    }

   if (SQL_ERROR ==
SQLBindCol(stmt, 1, SQL_C_SLONG, &c1, 0, NULL))
   {
     printf("ERROR: Bind 1 Column\n");
   }

   while ( (rc = SQLFetch(stmt)) != SQL_NO_DATA)
    {
        if ( rc != SQL_SUCCESS )
        {
            execute_err(dbc, stmt, query);
            break;
        }
        printf(" Result Set : [ %d ] \n", c1 );
    }

    SQLFreeStmt(stmt, SQL_DROP);

    ....

$ refcursor
===========================================================
 Result Set : [ 1 ]
 Result Set : [ 2 ]
 Result Set : [ 3 ]
 Result Set : [ 4 ]
```



### File Control Example

#### Example 1

Create a user and grant appropriate privileges to the user.

```
CONNECT SYS/MANAGER;
CREATE USER JEJEONG IDENTIFIED BY JEJEONG;
GRANT CREATE ANY DIRECTORY TO JEJEONG;
GRANT DROP ANY DIRECTORY TO JEJEONG;
```

Create a table and a directory object.

```
CONNECT JEJEONG/JEJEONG;
CREATE TABLE T1( ID INTEGER, NAME VARCHAR(40) );
INSERT INTO T1 VALUES( 1, 'JEJEONG');
INSERT INTO T1 VALUES( 2, 'EJPARK' );
INSERT INTO T1 VALUES( 3, 'WSKIM'  );
INSERT INTO T1 VALUES( 4, 'KKSHIM' );
INSERT INTO T1 VALUES( 5, 'CSKIM'  );
INSERT INTO T1 VALUES( 6, 'KDHONG' );
CREATE DIRECTORY MYDIR AS '/home/JEJEONG';
```

Create a stored procedure that reads all of the records from the table and writes them to the t1.txt file.

```
CREATE OR REPLACE PROCEDURE WRITE_T1
AS
  V1 FILE_TYPE;
  ID INTEGER;
  NAME VARCHAR(40);
BEGIN
  DECLARE
    CURSOR T1_CUR IS
      SELECT * FROM T1;
  BEGIN
    OPEN T1_CUR;
    V1 := FOPEN( 'MYDIR', 't1.txt', 'w' );
    LOOP
      FETCH T1_CUR INTO ID, NAME;
      EXIT WHEN T1_CUR%NOTFOUND;
      PUT_LINE( V1, 'ID : '||ID||' NAME : '||NAME);
    END LOOP;
    CLOSE T1_CUR;
    FCLOSE(V1);
  END;
END;
/
```

Create a stored procedure that reads all of the records from the t1.txt file and outputs them to the screen.

```
CREATE OR REPLACE PROCEDURE READ_T1
AS
  BUFFER VARCHAR(200);
  V1 FILE_TYPE;
BEGIN
  V1 := FOPEN('MYDIR', 't1.txt', 'r' );
  LOOP
    GET_LINE( V1, BUFFER, 200 );
    PRINT( BUFFER );
  END LOOP;
  FCLOSE( V1 );
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    FCLOSE( V1 );
END;
/
```



When the stored procedures created as described above are executed, the output is as follows:

```
iSQL> exec write_t1;
Execute success.
iSQL> exec read_t1;
ID : 1    NAME : JEJEONG
ID : 2    NAME : EJPARK
ID : 3    NAME : WSKIM
ID : 4    NAME : KKSHIM
ID : 5    NAME : CSKIM
ID : 6    NAME : KDHONG
Execute success.
```

The contents of the actual directory in the file system are as shown below:

```
$ cd /home/JEJEONG
$ cat t1.txt
ID : 1    NAME : JEJEONG
ID : 2    NAME : EJPARK
ID : 3    NAME : WSKIM
ID : 4    NAME : KKSHIM
ID : 5    NAME : CSKIM
ID : 6    NAME : KDHONG
```

### UTL_SMTP Example

##### Case

A mail  has to be sent in Korean text and the character set has to be converted to UTF-8 including the attached files while the character set of terminal and Altibase server are EUC-KR. Attached file is compressed version of utl_smtp.sql and it is sent in binary text.

##### Example

```
CREATE OR REPLACE PROCEDURE TEST2()
AS
    c CONNECT_TYPE;
    r VARCHAR(512);
BEGIN
    c := UTL_SMTP.OPEN_CONNECTION( '127.0.0.1', '25', NULL );
    r := UTL_SMTP.HELO( c, '127.0.0.1' );
    r := UTL_SMTP.MAIL( c, 'test@test.com' );
    r := UTL_SMTP.RCPT( c, '[test@test.com](mailto:test@test.com)');
    r := UTL_SMTP.OPEN_DATA( c );
    UTL_SMTP.WRITE_DATA( c, 'MIME-Version: 1.0' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, 'Content-Type: multipart/mixed; boundary="0A1B2C3D4E5F"' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, '--0A1B2C3D4E5F' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, 'Content-Type: text/plain; charset=utf-8' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, 'Content-Transfer-Encoding: 7bit' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, 'Content-Disposition: inline' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_RAW_DATA( c, to_raw(convert('Subject: 가나다','utf8')) );
    UTL_SMTP.WRITE_DATA( c, CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_RAW_DATA( c, to_raw(convert('가나다','utf8')) );
    UTL_SMTP.WRITE_DATA( c, CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, '--0A1B2C3D4E5F' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, 'Content-Type: application/octet-stream; name="test.zip"' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, 'Content-Transfer-Encoding: base64' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, 'Content-Disposition: attachment; filename="test.zip"' || CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, base64_encode_str( '504B03041403000008004E7AA24AD7949D9F9E010000500500000C00000075746C5F736D74702E73716CBD545D4FF23014BEEFAF3897607CF5054543B8AAA5207176A4762A57A46E8D3419EB6C8B1FFFDE4D5DC6F08390184F76757A9EAFD36D877BBF5308F68098FCC5EAFB85874EBFDFFFD7FDDF39DD079C7A7D279D2A4E6D6EACF4DA64602C68EFC0ADEE9C4EB4B45AB9839201A729BC3138B0CA29FBA892B2FF2B758810E1140B0A21074EA7012614A6985CE03185954FE76EE973C057088D2246C42464607295CD6393652A2E6DB760619C87F79A30B8C69C9C63DE3A396EEF23F8B68AD435A878041D53FE13C03FCFBD5E2AB3F26B0018D2118E02012C0A0268234E45C4199090314AC45CCCA674B0667DA152D382B8D26D0E7E124FCC52EA6C3354A552377B9D6E7B5DA540A5B5CA761DA7B244D98DF5757B279FF7974B2B975E59574F3657B0DD9C8D73BF8B39AB629D6B95F9BF30F7F66A25D2CBC2E1A6B36FD1531E123A8C388527ABBDAAF05BC3D555221AB7DCEB1D1517FD15B9954F3B0B344538BEA9041AD9E3D438B56BF81AFEB0D27E072065C301FAFAE38FCE820981AB190BD9ECB2FE078C8A19F7E20EAAC6008D396602E82D255149C11AC720C20FAAC2E52B504B01023F031403000008004E7AA24AD7949D9F9E010000500500000C0000000000000000002080B4810000000075746C5F736D74702E73716C504B050600000000010001003A000000C80100000000' ) );
    UTL_SMTP.WRITE_DATA( c, CHR(13) || CHR(10) );
    UTL_SMTP.WRITE_DATA( c, '--0A1B2C3D4E5F--' || CHR(13) || CHR(10) );
    r := UTL_SMTP.CLOSE_DATA( c );
    r := UTL_SMTP.QUIT( c );
END;
 /
```

### Checking SENDMAIL DAEMON Example

##### Case

Checking SENDMAIL DAEMON of E-mail server which sends E-mails before using UTL_SMTP package.

##### Example

Run the following commands in terminal.

1. telnet ip_address 25
2. helo ip_address
3. quit

The example below is run in terminal which is connected to the E-mail server.

```
$ telnet 127.0.0.1 25
...
2xx ...
helo 127.0.0.1
2xx ...
quit
2xx ...
```
