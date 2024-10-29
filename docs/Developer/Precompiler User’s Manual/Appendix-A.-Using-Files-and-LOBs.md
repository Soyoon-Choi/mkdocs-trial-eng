# Appendix A. Using Files and LOBs

This appendix explains how to use the file system with the BLOB and CLOB data types, so that data in files can be inserted into tables, or so that data from tables can be written to files.

### Output Host Variables and Files

When it is desired to select data from a BLOB or CLOB type column and store the data in a file, the following syntax is used to specify output host variables in the INTO clause of a SELECT statement:

#### Syntax

```
BLOB_FILE <:host_variable> OPTION <:file_type> INDICATOR <:indicator>
CLOB_FILE <:host_variable> OPTION <:file_type> INDICATOR <:indicator>
```

#### Arguments

<:host_varible>: This is a character-type variable containing the name of the file into which to store the data. 

<:file_type>: This is an integer-type variable that is used to specify the mode with which to access the file in order to write data to the file. The following modes are available:

- APRE_FILE_CREATE: In this mode, a new file is created, and the data are written to the new file. If a file having the specified name already exists, an error will be raised. 
- APRE_FILE_OVERWRITE: In this mode, an existing file is opened, and its contents are overwritten, starting from the beginning of the file. If no file having the specified name exists, a new file is created, and the data are written thereto. 
- APRE_FILE_APPEND: In this mode, an existing file is opened for appending, that is, the data are written at the end of the file, after any existing data. If no file having the specified name exists, an error will be raised.

\<:indicator\>: This is an indicator variable that is used to check for NULL returned values or to get the length of the data stored in the file.

#### Example

[Example] The following example shows the use of the CLOB_FILE keyword and a file open mode option. In this example, the T_LOB table is queried, an int type column value is stored in the sI1 output host variable, and a CLOB type column value is stored in a file whose name is specified in the string sI2FName, which is opened in APRE_FILE_CREATE mode.

\< Sample Program : clobSample.sc\>

```
EXEC SQL BEGIN DECLARE SECTION;
int          sI1;
char         sI2FName[33];
unsigned int sI2FOpt;
SQLLEN       sI2Ind;
EXEC SQL END DECLARE SECTION;

strcpy(sI2FName, aOutFileName);
sI2FOpt = APRE_FILE_CREATE;
 
EXEC SQL SELECT * 
INTO :sI1, CLOB_FILE :sI2FName OPTION :sI2FOpt INDICATOR :sI2Ind 
FROM T_LOB;
```

> \* An example of a blob is in \ $ ALTIBASE_HOME / sample / APRE / BLOB / blobSample.sc, and it is similar to CLOB.

### Input Host Variables

When it is desired to use an INSERT statement to insert all of the data from a file into a BLOB or CLOB type column, the following syntax for input host variables can be used.

#### Syntax

```
BLOB_FILE <:host_variable> OPTION <:file_type> INDICATOR <:indicator>
CLOB_FILE <:host_variable> OPTION <:file_type> INDICATOR <:indicator>
```

#### Arguments

<:host_varible> : This is a character-type variable containing the name of the file from which the data are to be read. 

<:file_type> : This is an integer variable to specify the file access mode when reading data from the file. 

Only the following mode is available:

- APRE_FILE_ READ: Open the file for reading. If no file having the specified name exists, an error will be raised.

\<:indicator\> : This is an indicator variable that is used to specify that NULL data are to be input.

#### Example

[Example] The following example shows how to insert a new record into the T_LOB table after reading binary data from a file in APRE_FILE_READ mode.

< Sample Program : blobSample.sc >

```
EXEC SQL BEGIN DECLARE SECTION;
int          sI1;
char         sI2FName[32];
unsigned int sI2FOpt;
SQLLEN       sI2Ind;
EXEC SQL END DECLARE SECTION;

sI1 = 1;
strcpy(sI2FName,aInputFileName);
sI2FOpt = APRE_FILE_READ;

EXEC SQL INSERT INTO T_LOB 
VALUES(:sI1, BLOB_FILE :sI2FName OPTION :sI2FOpt INDICATOR :sI2Ind); 
```

