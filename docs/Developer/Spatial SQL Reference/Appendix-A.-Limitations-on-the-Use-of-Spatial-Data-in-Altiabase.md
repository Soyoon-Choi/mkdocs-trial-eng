# Appendix A. Limitations on the Use of Spatial Data in Altiabase

With the expansion of Altibase into the realm of spatial data, inevitably some of Altibase's extensive functionality lacks support for use with spatial data. The current limitations are explained in detail in this Appendix.

### Limitations on GEOMETRY Type Columns

#### Constraints

In Altibase, the use of various kinds of constraints is supported to ensure the validity of data. 

Currently, the only constraint that can be applied to GEOMETRY type columns is the NOT NULL constraint. None of the other constraints can be used with spatial data.

#### SRID (Spatial Reference Identifier)

SRID (Spatial Reference Identifier) is an identifier specified to identify a spatial object. SRID can be applied to GEOMETRY column, and SRID can be assigned to GEOMETRY object that inserts into table.

The SRID of the GEOMETRY object INSERTing into the table must match or equal zero to the SIRD of the corresponding column. If the SRID of column or object, the default value of 0.

The SRID of a column can be changed by using the ALTER TABLE MODIFY COLUMN statement. In this case, the SRID of all objects inserted into column must match or be 0.

##### Example

```
CREATE TABLE T1 (I1 GEOMETRY);
INSERT INTO T1 VALUES(GEOMETRY'POINT(1 1)');
INSERT INTO T1 VALUES(GEOMETRY'SRID=99;POINT(1 1)');
 
-- FAIL
iSQL> ALTER TABLE T1 MODIFY COLUMN I1 SRID 100;
[ERR-31461 : Invalid SRID datatype.]
 
-- SUCCESS
iSQL> ALTER TABLE T1 MODIFY COLUMN I1 SRID 99;
Alter success.
```

#### Indexes 

The R-Tree index is provided for use in indexing spatial data. The R-Tree index cannot be created with the UNIQUE attribute, and cannot be created on the basis of multiple columns. 

If a NOT NULL constraint does not exist on a column composing an R-Tree index, caution is required as the result can differ depending on whether or not the R-Tree index is used for query execution. The leaf node of an R-Tree index contains neither the NULL nor EMPTY value. Therefore, neither the NULL nor EMPTY value affects queries which use an R-Tree index as below.

```
CREATE TABLE T1 (I1 GEOMETRY) TABLESPACE SYS_TBS_DISK_DATA;
CREATE INDEX T1_I1_IDX ON T1(I1);
INSERT INTO T1 VALUES (NULL);

iSQL>  SELECT /*+ index(t1, T1_I1_IDX) */ count(*) from T1;
COUNT
-----------------------
0
1 row selected.
```

However, if the full scan hint is specified in a query as below, the query result is affected by the NULL or EMPTY value stored in the column of an R-Tree index.

```
iSQL>  SELECT /*+ full scan(T1) */ count(*) from T1;
COUNT
-----------------------
1
1 row selected.
```

#### Triggers

The same limitations that apply to the use of triggers with stored procedures also apply to the use of triggers with spatial data. Additionally, the value of a GEOMETRY type column, including the so-called “before-image” and “after-image” values, cannot be used in a trigger.

#### Stored Procedures

GEOMETRY type data cannot be used as stored procedure parameters or as local variables within stored procedures.

#### Stored Functions

GEOMETRY Type cannot be used for parameter, return type, and local variable. 

