# Appendix E: PSM Converter Rule List

Migration Center provides DDL SQL texts for creating PSM type database objects with the PSM converter for Oracle or TimesTen 11.2 to Altibase Migration. 

The PSM converter uses rules to convert DDL SQL texts, and these rules fall into the following three categories:

- CONVERTED: Convertible

- REMOVED:  Inconvertible, but may be removable

- TODO: Neither convertible nor removable

If a TODO rule is applied to a PSM object, then it will show up in the To-do list pane. If not, then it will show up in the Done list pane. 

If the version scope is given for a rule, it means that the rule applies to only the specified Altibase version. On omission, the rule applies to all Altibase versions. 

When converting several SQL statements, end each SQL statement with a slash (‘/’) to separate them.

### View Conversion Rules

#### RULE-11001

- Type: REMOVED

- Description: 'WITH CHECK OPTION'이 제거되었다.

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS SELECT \* FROM t1
  
  **WITH CHECK OPTION**;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS SELECT \* FROM t1
  
  **/\* WITH CHECK OPTION \*/ /\* [REMOVED] RULE-11001 : 'WITH CHECK OPTION' is
  removed \*/**;

#### RULE-11002

- Type: REMOVED

- Description: The alias constraints have been removed

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  (a1 **UNIQUE**)
  
  AS SELECT c1 FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  (a1 **/\* UNIQUE \*/ /\* [REMOVED] RULE-11002 : Inline constraints are removed
  \*/**)
  
  AS SELECT c1 FROM t1;

#### RULE-11003

- Type: TODO

- Description: View level constraints must be converted manually.

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  (c1, CONSTRAINT v1_uk UNIQUE(c1)
  
  )
  
  AS SELECT c1 FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  (c1, CONSTRAINT v1_uk UNIQUE(c1) **/\* [TODO] RULE-11003 : Out of line
  constraint must be converted manually \*/)**
  
  AS SELECT c1 FROM t1;

#### RULE-11004

- Type: REMOVED

- Description: BEQUEATH clause has been removed.

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  BEQUEATH CURRENT_USER
  
  AS SELECT \* FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  **/\* BEQUEATH CURRENT_USER \*/ /\* [REMOVED] RULE-11004 : BEQUEATH clause is
  removed \*/**
  
  AS SELECT \* FROM t1;

#### RULE-11005

- Type: TODO

- Description: XMLType view clause should be converted manually.

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  **OF XMLTYPE WITH OBJECT ID DEFAULT**
  
  AS SELECT \* FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  **OF XMLTYPE WITH OBJECT ID DEFAULT /\* [TODO] RULE-11005 : XMLType view should
  be manually converted \*/**
  
  AS SELECT \* FROM t1;

#### RULE-11006

- Type: TODO

- Description: The clause of object type view should be manually converted.

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1_1
  
  **OF type1 UNDER v1**
  
  AS SELECT \* FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1_1
  
  **OF type1 UNDER v1 /\* [TODO] RULE-11006 : An object view must be converted
  manually \*/**
  
  AS SELECT \* FROM t1;

#### RULE-11007

- Type: REMOVED

- Description: VISIBLE or INVISIBLE has been removed.

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  (c1, c2 **INVISIBLE**)
  
  AS SELECT \* FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  (c1, c2 **/\* INVISIBLE \*/ /\* [REMOVED] RULE-11007 : VISIBLE or INVISIBLE is
  removed \*/**)
  
  AS SELECT \* FROM t1;

#### RULE-11008

- Type: REMOVED

- Description: FORCE has been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE **FORCE** VIEW v1
  
  (c1, c2)
  
  AS SELECT \* FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE /**\* FORCE \*/ /\* [REMOVED] RULE-11008 : FORCE has been
  removed \*/** VIEW v1
  
  (c1, c2)
  
  AS SELECT \* FROM t1;

### Trigger Conversion Rules

#### RULE-12002

- Version Scope: Less than the Altibase version 6.3.1.0.0

- Type: TODO

- Description: ‘INSTEAD OF’ should be manually converted 

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER log_attendance
  
  **INSTEAD OF** INSERT ON attendance_view FOR EACH ROW
  
  BEGIN
  
  IF :NEW.cnt \< 2 THEN
  
  INSERT INTO daily_log VALUES(:NEW.id, CURRENT_TIMESTAMP);
  
  END IF;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER log_attendance
  
  **INSTEAD OF /\* [TODO] RULE-12002 : 'INSTEAD OF' must be converted manually
  \*/** INSERT ON attendance_view FOR EACH ROW
  
  BEGIN
  
  IF :NEW.cnt \< 2 THEN
  
  INSERT INTO daily_log VALUES(:NEW.id, CURRENT_TIMESTAMP);
  
  END IF;
  
  END;

#### RULE-12003

- Type: TODO

- Description: Triggers supporting multiple events must be converted manually

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE **INSERT OR DELETE ON** t1
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE **INSERT OR DELETE ON t1 /\* [TODO] RULE-12003 : Triggers supporting
  multiple events must be converted manually \*/**
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12004

- Version Scope: Less than the Altibase version 6.3.1.0.0

- Type: TODO

- Description: AS or IS should be used regardless of that DECLARE exists or not in the PSM block. 

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE INSERT ON t1
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE INSERT ON t1
  
  BEGIN **/\* [TODO] RULE-12004 : 'AS' or 'IS' should be used regardless of that
  DECLARE exists or not in the PSM block. \*/**
  
  NULL;
  
  END;

- Version Scope: Altibase 6.3.1.0.0 ~ 6.5.1.3.7 or below

- Type: TODO

- Description: The DECLARE preceding the PSM body should be replaced with AS or IS.

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE INSERT ON t1
  
  **DECLARE**
  
  v1 NUMBER := 1;
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE INSERT ON t1
  
  **DECLARE /\* [TODO] RULE-12004 : 'AS' or 'IS' must replace 'DECLARE' that
  starts the declarative part of the block \*/**
  
  v1 NUMBER := 1;
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12005

- Type: TODO

- Description: Non-DML triggers must be converted manually

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  **BEFORE CREATE ON DATABASE**
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  **BEFORE CREATE ON DATABASE /\* [TODO] RULE-12005 : Non DML trigger must be
  converted manually \*/**
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12007

- Type: TODO

- Description: Nested tables must be converted manually

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  INSTEAD OF DELETE ON **NESTED TABLE t1 OF v1**
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  INSTEAD OF DELETE ON **NESTED TABLE t1 OF v1 /\* [TODO] RULE-12007 : Nested
  table must be converted manually \*/**
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12008

- Type: TODO

- Description: The CALL routine clause must be converted manually

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER DELETE ON t1
  
  **CALL testproc1(a1, a2);**

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER DELETE ON t1
  
  **CALL testproc1(a1, a2) /\* [TODO] RULE-12008 : CALL routine clause must be
  converted manually \*/;**

#### RULE-12009

- Type: TODO

- Description: The parent row of a nested table cannot be specified.

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  INSTEAD OF DELETE ON NESTED TABLE t1 OF v1
  
  REFERENCING **PARENT AS parent** FOR EACH ROW
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  INSTEAD OF DELETE ON NESTED TABLE t1 OF v1
  
  REFERENCING **PARENT AS parent /\* [TODO] RULE-12009 : Parent value of the
  current row cannot be specified \*/** FOR EACH ROW
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12010

- Type: TODO

- Description: The trigger ordering clause should be converted manually

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER DELETE ON t1
  
  **FOLLOWS trig2**
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER DELETE ON t1
  
  **FOLLOWS trig2 /\* [TODO] RULE-12010 : Trigger ordering clause must be
  converted manually \*/**
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12011

- Type: CONVERTED

- Description: The ommitted correlation name has been added in the REFERENCING clause.

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER INSERT ON t1 FOR EACH ROW
  
  BEGIN
  
  **:new**.c1 := SYSDATE;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER INSERT ON t1
  
  **REFERENCING NEW AS new** FOR EACH ROW
  
  DECLARE
  
  BEGIN
  
  **:new**.c1 := SYSDATE;
  
  END;

#### RULE-12012

- Type: CONVERTED

- Description: A suffix has been added to the local identifier corresponding to the reserved words of Altibase.

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER UPDATE ON t1
  
  REFERENCING NEW AS **new** OLD AS **old** FOR EACH ROW
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL TExt:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER UPDATE ON t1
  
  REFERENCING NEW AS **new_POC** OLD AS **old_POC** FOR EACH ROW
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12013

- Type: REMOVED

- Description: The trigger eddition clause has been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER DELETE ON t1
  
  **CROSSEDITION**
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER DELETE ON t1
  
  **/\* CROSSEDITION \*/ /\* [REMOVED] RULE-12013 : Trigger edition clause is
  removed \*/**
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12014

- Type: REMOVED

- Description: The ENABLE has been removed

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER INSERT ON t1
  
  **ENABLE**
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER INSERT ON t1
  
  **/\* ENABLE \*/ /\* [REMOVED] RULE-12014 : ENABLE is removed \*/**
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12015

- Type: TODO

- Description: The DISABLE should be converted manually. : 

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER DELETE ON t1
  
  **DISABLE**
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER DELETE ON t1
  
  **DISABLE /\* [TODO] RULE-12015 : DISABLE must be converted manually \*/**
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-12016

- Type: CONVERTED

- Description: The colon preceding the alias referring to the rows defined in the REFERENCING clause has been eliminated. 

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE INSERT ON t1 FOR EACH ROW
  
  BEGIN
  
  DBMS_OUTPUT.PUT_LINE(:new.c1);
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE INSERT ON t1 FOR EACH ROW
  
  BEGIN
  
  DBMS_OUTPUT.PUT_LINE(new.c1);
  
  END;

#### RULE-12017

- Type: REMOVED

- Description: The trigger label name at the end of PL/SQL block has been removed in the CREATE TRIGGER statement.

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER INSERT ON t1
  
  BEGIN
  
  NULL;
  
  END trig1;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  AFTER INSERT ON t1
  
  BEGIN
  
  NULL;
  
  END /\* trig1 \*/ /\* [REMOVED] RULE-12017 : The trigger label name at the end
  of body has been removed \*/;

### Function Conversion Rules

#### RULE-13001

- Version Scope: Less than the Altibase version 6.3.1.0.0 

- Type: TODO 

- Description: The AS LANGUAGE clause must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1
  
  RETURN VARCHAR2
  
  **AS LANGUAGE JAVA**
  
  **NAME 'test.quote() return java.lang.String';**

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1
  
  RETURN VARCHAR2
  
  AS LANGUAGE JAVA
  
  NAME 'test.quote() return java.lang.String' **/\* [TODO] RULE-13001 : AS
  LANGUAGE clause must be converted manually \*/**;

#### RULE-13002

- Type: REMOVED 

- Description: The AUTHID clause is removed 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  **AUTHID CURRENT_USER**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  **/\* AUTHID CURRENT_USER \*/ /\* [REMOVED] RULE-13002 : The invoker rights
  clause is removed \*/**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

#### RULE-13003

- Type: REMOVED 

- Description: The PARALLEL_ENABLE clause is removed 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  **PARALLEL_ENABLE**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  **/\* PARALLEL_ENABLE \*/ /\* [REMOVED] RULE-13003 : PARALLEL_ENABLE clause is
  removed \*/**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

#### RULE-13004

- • Type: REMOVED 

- Description: The RESULT_CACHE clause is removed 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  **RESULT_CACHE RELIES_ON(t1, t2)**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  **/\* RESULT_CACHE RELIES_ON(t1, t2) \*/ /\* [REMOVED] RULE-13004 : RESULT_CACHE
  clause is removed \*/**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

#### RULE-13005

- Type: REMOVED 

- Description: DETERMINISTIC is removed 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION tfunc1(a1 NUMBER)
  
  RETURN NUMBER
  
  **DETERMINISTIC**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  **/\* DETERMINISTIC \*/ /\* [REMOVED] RULE-13005 : 'DETERMINISTIC' is removed
  \*/**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

#### RULE-13006

- Type: TODO 

- Description: The PIPELINED keyword must be converted manually 

- Original SQL Text:
  
  CREATE FUNCTION getCityList RETURN tripLog_pkg.nt_city PIPELINED AS
  
  BEGIN
  
  FOR i IN 1..tripLog_pkg.v_cityList.LAST LOOP
  
  PIPE ROW(tripLog_pkg.v_cityList(i));
  
  END LOOP;
  
  RETURN;
  
  END;

- Processed SQL Text:
  
  CREATE FUNCTION getCityList RETURN tripLog_pkg.nt_city **PIPELINED /\* [TODO]
  RULE-13006 : The keyword PIPELINED must be converted manually \*/** AS
  
  BEGIN
  
  FOR i IN 1 .. tripLog_pkg.v_cityList.LAST LOOP
  
  PIPE ROW(tripLog_pkg.v_cityList(i)) /\* [TODO] RULE-32012 : The PIPE ROW
  statement must be converted manually \*/;
  
  END LOOP;
  
  RETURN;
  
  END;

#### RULE-13007

- Type: TODO 

- Description: The PIPELINED USING/AGGREGATE USING clause must be converted manually .

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1
  
  RETURN NUMBER
  
  **AGGREGATE USING implementation_type;**

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1
  
  RETURN NUMBER
  
  AGGREGATE USING implementation_type **/\* [TODO] RULE-13007 : PIPELINED USING or
  AGGRAGATE USING clause must be converted manually \*/**;

#### RULE-13008

- Version Scope: Altibase 6.3.1.0.0 or above 

- Type: TODO 

- Description: The WITH CONTEXT clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1
  
  RETURN NUMBER IS
  
  LANGUAGE C LIBRARY lib1 **WITH CONTEXT** PARAMETERS(CONTEXT);

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1
  
  RETURN NUMBER IS
  
  LANGUAGE C LIBRARY lib1 **WITH CONTEXT /\* [TODO] RULE-13008 : WITH CONTEXT
  clause must be converted manually \*/** PARAMETERS(CONTEXT);

#### RULE-13009

- Version Scope: Altibase 6.3.1.0.0 or above 

- Type: TODO 

- Description: The AGENT IN claue should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1
  
  RETURN NUMBER IS
  
  LANGUAGE C LIBRARY lib1 **AGENT IN(EXTPROC)**;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1
  
  RETURN NUMBER IS
  
  LANGUAGE C LIBRARY lib1 **AGENT IN(EXTPROC) /\* [TODO] RULE-13009 : AGENT IN
  clause must be converted manually \*/**;

#### RULE-13010

- Type: REMOVED 

- Description: The ACCESSIBLE BY clause has been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  **ACCESSIBLE BY (TRIGGER trig1)**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  **/\* ACCESSIBLE BY (TRIGGER trig1) \*/ /\* [REMOVED] RULE-13010 : The
  ACCESSIBLE BY clause is removed \*/**
  
  IS
  
  BEGIN
  
  RETURN a1;
  
  END;

#### RULE-13011

- Version Scope: Altibase 6.3.1.0.0 or later

- Type: TODO 

- Description: JAVA call specification should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 VARCHAR2)
  
  RETURN VARCHAR2 IS
  
  **LANGUAGE JAVA NAME**
  
  **'com.altibase.ex.empMgr.addEmp(java.lang.String)'**;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 VARCHAR2)
  
  RETURN VARCHAR2 IS
  
  **LANGUAGE JAVA NAME**
  
  **'com.altibase.ex.empMgr.addEmp(java.lang.String)' /\* [TODO] RULE-13011 : Java
  call specification must be converted manually \*/;**

#### RULE-13012

- Version Scope: Altibase 6.3.1.0.0 or above 

- Type: TO DO 

- Description: The external parameter CONTEXT and SELF should manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 VARCHAR2)
  
  RETURN NUMBER AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1, a1 LENGTH, **SELF**);

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 VARCHAR2)
  
  RETURN NUMBER AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1, a1 LENGTH, **SELF /\* [TODO] RULE-13012 : The external parameter
  CONTEXT and SELF should be manually converted \*/**);

#### RULE-13013

- Version Scope: Altibase 6.3.1.0.0 or later

- Type: TODO 

- Description: The properties should be manually converted except INDICATOR, LENGTH, and MAXLEN. 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 VARCHAR2)
  
  RETURN NUMBER AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1, a1 **CHARSETID**, a1 **CHARSETFORM**);

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 VARCHAR2)
  
  RETURN NUMBER AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1, a1 **CHARSETID /\* [TODO] RULE-13013 : The property except for
  INDICATOR, LENGTH, MAXLEN must be converted manually \*/**, a1 **CHARSETFORM /\*
  [TODO] RULE-13013 : The properties should be manually converted except
  INDICATOR, LENGTH, and MAXLEN \*/**);

#### RULE-13014

- Version Scope: Altibase 6.3.1.0.0 or later

- Type: TODO 

- Description: The BY REFERENCE clause should manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1 **BY REFERENCE**);

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1 **BY REFERENCE /\* [TODO] RULE-13014 : BY REFERENCE clause must be
  converted manually \*/**);

#### RULE-13015

- Version Scope: Altibase 6.3.1.0.0 or later

- Type: TODO 

- Description: External data type of the parameters should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1 OCINUMBER)**;**

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER AS
  
  LANGUAGE C LIBRARY lib
  
  PARAMETERS(a1 **OCINUMBER /\* [TODO] RULE-13015 : External data type of the
  parameters should be manually converted \*/**);

### Procedure Conversion Rules

#### RULE-14001

- Version Scope: Less than the Altibase version tag 6.3.1.0.0 

- Type: TODO 

- Description: The AS LANGUAGE clause must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER)
  
  AS LANGUAGE JAVA
  
  NAME 'test.quote() return java.lang.String';

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 INUMBER)
  
  AS LANGUAGE JAVA
  
  NAME 'test.quote() return java.lang.String' **/\* [TODO] RULE-14001 : AS
  LANGUAGE clause must be converted manually \*/**;

#### RULE-14002

- Type: REMOVED 

- Description: The AUTHID clause is removed 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER)
  
  **AUTHID DEFINER**
  
  IS
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER)
  
  **/\* AUTHID DEFINER \*/ /\* [REMOVED] RULE-14002 : AUTHID clause is removed
  \*/**
  
  IS
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-14003

- Version Scope : Altibase 6.3.1.0.0 or later

- Type: TODO 

- Description: The WITH CONTEXT clause should manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  LANGUAGE C LIBRARY lib1 **WITH CONTEXT;**

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  LANGUAGE C LIBRARY lib1 **WITH CONTEXT /\* [TODO] RULE-14003 : WITH CONTEXT
  clause must be converted manually \*/;**

#### RULE-14004

- Version Scope : Altibase 6.3.1.0.0 or later

- Type: TODO 

- Description: The AGENT IN clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  LANGUAGE C LIBRARY lib1 **AGENT IN(EXTPROC)**;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  LANGUAGE C LIBRARY lib1 **AGENT IN(EXTPROC) /\* [TODO] RULE-14004 : AGENT IN
  clause must be converted manually \*/**;

#### RULE-14005

- Type: REMOVED

- Description: The ACCESSIBLE BY clause has been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1
  
  **ACCESSIBLE BY (TRIGGER trig1)**
  
  IS
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1
  
  **/\* ACCESSIBLE BY (TRIGGER trig1) \*/ /\* [REMOVED] RULE-14005 : The
  ACCESSIBLE BY clause is removed \*/**
  
  IS
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-14006

- Version Scope : Altibase 6.3.1.0.0 or later

- Type: TODO 

- Description: JAVA call specification should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 VARCHAR2) AS
  
  **LANGUAGE JAVA NAME**
  
  **'com.altibase.ex.empMgr.addEmp(java.lang.String)'**;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 VARCHAR2) AS
  
  **LANGUAGE JAVA NAME**
  
  **'com.altibase.ex.empMgr.addEmp(java.lang.String)';**
  
  **/\* [TODO] RULE-14006 : Java call specification should be converted manually
  \*/**

#### RULE-14007

- Version Scope : Altibase 6.3.1.0.0 or later 

- Type: TODO

- Description: The parameters CONTEXT and SELF should be mannuallly converted. 

- Original SQL Text
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER) AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1, a1 LENGTH, **SELF**);

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER) AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1, a1 LENGTH, **SELF /\* [TODO] RULE-14007 : The parameters CONTEXT
  and SELF should be manually converted \*/**);

#### RULE-14008

- Version Scope : Altibase 6.3.1.0.0 or later 

- Type: TODO 

- Description: The properties should be manually converted except INDICATOR, LENGTH, and MAXLEN. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER) AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1, a1 **CHARSETID**, a1 **CHARSETFORM**);

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER) AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1, a1 **CHARSETID /\* [TODO] RULE-14008 : The property except for
  INDICATOR, LENGTH, MAXLEN must be converted manually \*/**, a1 **CHARSETFORM /\*
  [TODO] RULE-14008 : The property except for INDICATOR, LENGTH, MAXLEN must be
  converted manually \*/**);

#### RULE-14009

- Version Scope : Altibase 6.3.1.0.0 or later 

- Type: TODO 

- Description: The BY REFERENCE clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER) AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1 **BY REFERENCE**);

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER) AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1 **BY REFERENCE /\* [TODO] RULE-14009 : BY REFERENCE clause must be
  converted manually \*/**);

#### RULE-14010

- Version Scope : Altibase 6.3.1.0.0 or later 

- Type: TODO 

- Description: External data type of the parameters should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER) AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1 OCINUMBER);

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER) AS
  
  LANGUAGE C LIBRARY lib1
  
  PARAMETERS(a1 OCINUMBER /\* [TODO] RULE-14010 : External data type of the
  parameters should be manually converted \*/);

### Materialized View Conversion Rules

#### RULE-15004

- Type: REMOVED 

- Description: All clauses between the column alias clause and subquery are removed 

- Original SQL Text:
  
  CREATE MATERIALIZED VIEW mview1
  
  **ORGANIZATION HEAP PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255**
  
  **NOCOMPRESS LOGGING**
  
  **STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645**
  
  **PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT)**
  
  **TABLESPACE test**
  
  **BUILD IMMEDIATE**
  
  **USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255**
  
  **STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645**
  
  **PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT)**
  
  **TABLESPACE tbs1**
  
  **REFRESH FAST ON DEMAND**
  
  **WITH PRIMARY KEY USING DEFAULT LOCAL ROLLBACK SEGMENT**
  
  **USING ENFORCED CONSTRAINTS FOR UPDATE DISABLE QUERY REWRITE**
  
  AS SELECT \* FROM t1;

- Processed SQL Text:
  
  CREATE MATERIALIZED VIEW mview1
  
  **//\* ORGANIZATION HEAP PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255**
  
  **NOCOMPRESS LOGGING**
  
  **STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645**
  
  **PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT)**
  
  **TABLESPACE test**
  
  **BUILD IMMEDIATE**
  
  **USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255**
  
  **STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645**
  
  **PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT)**
  
  **TABLESPACE tbs1**
  
  **REFRESH FAST ON DEMAND**
  
  **WITH PRIMARY KEY USING DEFAULT LOCAL ROLLBACK SEGMENT**
  
  **USING ENFORCED CONSTRAINTS FOR UPDATE DISABLE QUERY REWRITE \*/ /\* [REMOVED]
  RULE-15004 : All clauses between column alias clause and subquery are removed
  \*/**
  
  AS SELECT \* FROM t1;

### Package Conversion Rules

#### RULE-16001

- Type: REMOVED 

- Description: The AUTHID clause is removed

- Original SQL Text:
  
  CREATE OR REPLACE PACKAGE empMgr_pkg AUTHID CURRENT_USER AS PROCEDURE
  delete(p_id INTEGER);
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PACKAGE empMgr_pkg **/\* AUTHID CURRENT_USER \*/ /\* [REMOVED]
  RULE-16001 : The invoker rights clause is removed \*/** AS PROCEDURE delete(p_id
  INTEGER);
  
  END;

#### RULE-16002

- Type: REMOVED 

- Description: The ACCESSIBLE BY clause has been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE PACKAGE pkg1
  
  **ACCESSIBLE BY (TRIGGER trig1)**
  
  AS
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PACKAGE pkg1
  
  **/\* ACCESSIBLE BY (TRIGGER trig1) \*/ /\* [REMOVED] RULE-16002 : The
  ACCESSIBLE BY clause is removed \*/**
  
  AS
  
  END;

### Library Conversion Rules

#### RULE-17001

- Type: REMOVED 

- Description: The AGENT clause is removed 

- Original SQL Text:
  
  CREATE OR REPLACE LIBRARY lib1 AS
  
  '\${ORACLE_HOME}/lib/test_lib.so' **AGENT 'test.rule.no_17001.com'**;

- Processed SQL Text:
  
  CREATE OR REPLACE LIBRARY lib1 AS
  
  '\${ORACLE_HOME}/lib/test_lib.so' **/\* AGENT 'test.rule.no_17001.com' \*/ /\*
  [REMOVED] RULE-17001 : Agent clause is removed \*/**;

#### RULE-17002

- Type: REMOVED 

- Description: The UNTRUSTED keyword is removed 

- Original SQL Text:
  
  CREATE OR REPLACE LIBRARY lib1 **UNTRUSTED**
  
  AS '\${ORACLE_HOME}/lib/test_lib.so';

- Processed SQL Text:
  
  CREATE OR REPLACE LIBRARY lib1 **/\* UNTRUSTED \*/ /\* [REMOVED] RULE-17002 :
  The keyword UNTRUSTED is removed \*/**
  
  AS '\${ORACLE_HOME}/lib/test_lib.so';

### DML Conversion Rules

#### RULE-20001

- Type: TODO 

- Description: The Flashback Query clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \* FROM t1 CROSS JOIN t2 **VERSIONS BETWEEN TIMESTAMP MINVALUE AND
  MAXVALUE**;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM t1 CROSS JOIN t2 **VERSIONS BETWEEN TIMESTAMP MINVALUE AND
  MAXVALUE /\* [TODO] RULE-20001 : Flashback query clause must converted manually
  \*/**;

#### RULE-20006

- Type: TODO 

- Description: DBlink must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM t1@remote;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM t1@**remote /\* [TODO] RULE-20006 : DBlink must be converted
  manually \*/**;

#### RULE-20007

- Version Scope: Less than the Altibase version tag 6.5.1.0.0 

- Type: TODO 

- Description: The GROUPING SETS clause must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1, c2, c3, c4, SUM( c5 )
  
  FROM t1
  
  GROUP BY **GROUPING SETS((c1, c2, c3, c4), (c1, c2, c3), (c3, c4));**

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW r1
  
  AS
  
  SELECT c1, c2, c3, c4, SUM(c5)
  
  FROM t1
  
  GROUP BY **GROUPING SETS( (c1, c2, c3, c4), (c1, c2, c3), (c3, c4) ) /\* [TODO]
  RULE-20007 : GROUPING SETS clause must be converted manually \*/**;

#### RULE-20009

- Version Scope: Less than the Altibase version tag 6.3.1.0.0 

- Type: TODO 

- Description: The START WITH clause following the CONNECT BY clause must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1, c2, c3, c4 FROM t1 CONNECT BY c1 = c2 **START WITH c1 = c4;**

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1, c2, c3, c4 FROM t1 CONNECT BY c1 = c2 **START WITH c1 = c4 /\* [TODO]
  RULE-20009 : START WITH clause after CONNECT BY clause must be converted
  manually \*/;**

#### RULE-20010

- Version Scope: Less than the Altibase version tag 6.3.1.0.0 

- Type: TODO 

- Description: The IGNORE LOOP should be placed after the following condition to convert NOCYCLE. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1, c2, c3, c4
  
  FROM t1 CONNECT BY NOCYCLE c1 = c2 START WITH c1 = c4;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1, c2, c3, c4
  
  FROM t1 CONNECT BY NOCYCLE **/\* [TODO] RULE-20010 : To convert 'NOCYCLE',
  'IGNORE LOOP' should come after the following condition \*/** c1 = c2 START WITH
  c1 = c4;

#### RULE-20011

- Type: REMOVED 

- Description: All hints are removed 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT **/\*+ORDERED \*/** \* FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM t1;

#### RULE-20012

- Version Scope: Less than the Altibase version tag 6.1.1.0.0 

- Type: TODO 

- Description: The PIVOT clause must be reviewed 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW xmlView
  
  AS
  
  SELECT \*
  
  FROM (SELECT d.dname, e.sex FROM departments d, employees e WHERE d.dno = e.dno)
  
  **PIVOT XML (COUNT(\*) FOR sex IN (ANY))**
  
  ORDER BY dname;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW xmlView
  
  AS
  
  SELECT \*
  
  FROM (SELECT d.dname, e.sex FROM departments d, employees e WHERE d.dno = e.dno)
  
  **PIVOT XML (COUNT(\*) FOR sex IN (ANY)) /\* [TODO] RULE-20012 : PIVOT clause
  must be reviewed \*/**
  
  ORDER BY dname;

#### RULE-20013

- Version Scope: Less than the Altibase version tag 6.5.1.0.0 

- Type: TODO 

- Description: The UNPIVOT clause must be reviewed 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM t1
  
  UNPIVOT (c5 FOR c2 IN (c3 AS 'no', c4 AS 'name'))
  
  ORDER BY c1, c2;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM t1
  
  UNPIVOT (c5 FOR c2 IN (c3 AS 'no', c4 AS 'name')) **/\* [TODO] RULE-20013 :
  UNPIVOT clause must be reviewed \*/**
  
  ORDER BY c1, c2;

#### RULE-20014

- Type: CONVERTED 

- Description: Schema names are removed 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE **testuser1**.proc1(a1 NUMBER)
  
  AS
  
  BEGIN
  
  INSERT INTO **testuser1**.t1 VALUES(1, 2, 3);
  
  UPDATE testuser2.t1 SET c1 =3, c2 = c2 + 4, c3 = 9 WHERE c4 = 12;
  
  DELETE FROM **TESTUSER1**.t1 WHERE c4 = 12;
  
  SELECT \* INTO :cur1, :cur2 FROM "**TEST_USER1**".t1;
  
  SELECT \* INTO :cur1, :cur2 FROM "Test_User1".t1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER)
  
  AS
  
  BEGIN
  
  INSERT INTO t1 VALUES(1, 2, 3);
  
  UPDATE testuser2.t1 SET c1 =3, c2 = c2 + 4, c3 = 9 WHERE c4 = 12;
  
  DELETE FROM t1 WHERE C4 = 12;
  
  SELECT \* INTO :cur1, :cur2 FROM t1;
  
  SELECT \* INTO :cur1, :cur2 FROM "Test_User1".t1;
  
  END;

#### RULE-20015

- Version Scope: Less than the Altibase version tag 6.3.1.0.0 

- Type: TODO 

- Description: The RETURNING clause must be converted manually 

- Original SQL Text:
  
  CREATE FUNCTION deleteMenu(p_menuName IN VARCHAR2) RETURN INTEGER
  
  AS
  
  v_totalCnt INTEGER;
  
  BEGIN
  
  SELECT COUNT(\*) INTO v_totalCnt FROM menus;
  
  DELETE FROM menus WHERE name = p_menuName **RETURNING v_totalCnt - COUNT(\*)
  INTO v_totalCnt;**
  
  RETURN v_totalCnt;
  
  END;

- Processed SQL Text:
  
  CREATE FUNCTION deleteMenu(p_menuName IN VARCHAR(32000))
  
  RETURN INTEGER
  
  AS
  
  v_totalCnt INTEGER;
  
  BEGIN
  
  SELECT COUNT(\*) INTO v_totalCnt FROM menus;
  
  DELETE FROM menus WHERE name = p_menuName **RETURNING v_totalCnt - COUNT(\*)
  INTO v_totalCnt; /\* [TODO] RULE-20015 : The RETURNING clause must be converted
  manually \*/;**
  
  RETURN v_totalCnt;
  
  END;

#### RULE-20016

- Type: TODO 

- Description: The CONNECT_BY_ISCYCLE pseudo-column should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1,
  
  CONNECT_BY_ISCYCLE "IsCycle",
  
  LEVEL,
  
  SYS_CONNECT_BY_PATH(c1, '/') "Path"
  
  FROM t1
  
  WHERE LEVEL \<= 3
  
  START WITH c2 = 100
  
  CONNECT BY PRIOR c2 = c3 AND LEVEL \<= 4;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1,
  
  CONNECT_BY_ISCYCLE "IsCycle" **/\* [TODO] RULE-20016 : The CONNECT_BY_ISCYCLE
  pseudocolumn must be converted manually \*/**,
  
  LEVEL,
  
  SYS_CONNECT_BY_PATH(c1, '/') "Path"
  
  FROM t1
  
  WHERE LEVEL \<= 3
  
  START WITH c2 = 100
  
  CONNECT BY PRIOR c2 = c3 AND LEVEL \<= 4;

#### RULE-20017

- Version Scope: Altibase 6.3.1.1.7 or earlier

- Type: REMOVED 

- Description: NULLS FIRST and NULLS LAST are removed 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1,
  
  RANK() OVER (ORDER BY c1 **NULLS LAST**)
  
  FROM t1
  
  ORDER BY c1 **NULLS FIRST**;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1,
  
  RANK() OVER (ORDER BY c1 **/\* NULLS LAST \*/ /\* [REMOVED] RULE-20017 : 'NULLS
  FIRST' and 'NULLS LAST' are removed \*/**)
  
  FROM t1
  
  ORDER BY c1 **/\* NULLS LAST \*/ /\* [REMOVED] RULE-20017 : 'NULLS FIRST' and
  'NULLS LAST' are removed \*/**;

#### RULE-20019

- Type: REMOVED 

- Description: The Subquery restriction clasue has been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM (SELECT \* FROM t2 **WITH READ ONLY**) t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM (SELECT \* FROM t2 **/\* WITH READ ONLY \*/ /\* [REMOVED]
  RULE-20019 : Restriction clause is removed \*/**) t1;

#### RULE-20020

- Type: TODO 

- Description: An inner join clause that is a CROSS or NATURAL INNER join must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM (SELECT \* FROM t1) CROSS JOIN t2;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT \* FROM (SELECT \* FROM t1) CROSS JOIN t2 **/\* [TODO] RULE-20020 : A
  CROSS or NATURAL INNER join must be converted manually \*/**;

#### RULE-20021

- Type: TODO 

- Description: The USING clause in a join should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1, c2 FROM t1 JOIN t2 USING(c1, c2);

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT c1, c2 FROM t1 JOIN t2 USING(c1, c2**) /\* [TODO] RULE-20021 : USING
  clause in a join must be converted manually \*/**;

#### RULE-20022

- Type: TODO 

- Description: A NATURAL type outer join clause must be converted manually 

- Original SQL Text:
  
  CREATE VIEW sales_view
  
  AS
  
  SELECT \* FROM log_guest NATURAL **FULL OUTER JOIN log_sales**
  
  ORDER BY datetime;

- Processed SQL Text:
  
  CREATE VIEW sales_view AS
  
  SELECT \* FROM log_guest **NATURAL FULL OUTER JOIN log_sales /\* [TODO]
  RULE-20022 : NATURAL type outer join clause must be converted manually \*/**
  
  ORDER BY datetime;

#### RULE-20023

- Type: CONVERTED 

- Description: The UNIQUE is converted 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT **UNIQUE** c1 FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1
  
  AS
  
  SELECT **DISTINCT** c1 FROM t1;

#### RULE-20028

- Type: CONVERTED

- Description: Double quotations are removed. However, in the reconcile "Unacceptable Name" step, if the "Use Double-quoted Identifier" option is selected for an object that requires double quotes in the name, the quotes for the name are not removed.

- Original SQL Text:
  
  CREATE OR REPLACE VIEW "USER1"."V1" ("A1")
  
  AS
  
  SELECT "CODE" "A1" FROM "T1"
  
  UNION ALL
  
  SELECT code A1 FROM T2
  
  UNION ALL
  
  SELECT "no" "A1" FROM "T3" WHERE "C6" = '2';

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW USER1.V1(A1)
  
  AS
  
  SELECT CODE A1 FROM T1
  
  UNION ALL
  
  SELECT code A1 FROM T2
  
  UNION ALL
  
  SELECT no A1 FROM T3 WHERE C6 = '2';

#### RULE-20029

- Type: CONVERTED 

- Description: The global identifier that is an Altibase keyword is converted by appending a postfix 

- Original SQL Text:
  
    CREATE PROCEDURE open(p_objName VARCHAR2, p_objType VARCHAR2)
  
    AS
  
    v_ddl VARCHAR2(200) := 'CREATE ' \|\| p_objType \|\| ' ' \|\| p_objName;
  
    BEGIN
  
    CASE p_objType
  
    WHEN 'TABLE' THEN v_ddl := v_ddl \|\| ' (c1 INTEGER)';
  
    WHEN 'VIEW' THEN v_ddl := v_ddl \|\| ' AS SELECT \* FROM dual';
  
    END CASE;
  
    DBMS_OUTPUT.PUT_LINE(v_ddl);
  
    EXECUTE IMMEDIATE v_ddl;
  
    END;

- Processed SQL Text:
  
  CREATE PROCEDURE open_POC(p_objName VARCHAR2, p_objType VARCHAR2)
  
  AS
  
  v_ddl VARCHAR2(200) := 'CREATE' \|\| p_objType \|\| ' ' \|\| p_objName;
  
  BEGIN
  
  CASE p_objType
  
  WHEN 'TABLE' THEN v_ddl := v_ddl \|\| ' (c1 INTEGER)';
  
  WHEN 'VIEW' THEN v_ddl := v_ddl \|\| ' AS SELECT \* FROM dual';
  
  END CASE;
  
  DBMS_OUTPUT.PUT_LINE(v_ddl);
  
  EXECUTE IMMEDIATE v_ddl;
  
  END;

#### RULE-20030

- Version Scope: Greater than or equal to the Altibase version tag 6.5.1.0.0 

- Type: TODO 

- Description: Window functions with the GROUPING SETS clause must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS SELECT c1, c2, SUM(c3), **RANK() OVER(ORDER BY
  c1)**
  
  FROM t1 **GROUP BY GROUPING SETS(c1, c2)**;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS SELECT c1, c2, SUM(c3**), RANK() OVER(ORDER BY c1)
  /\* [TODO] RULE-20030 : Window functions with the GROUPING SETS clause must be
  convert manually. \*/** FROM t1 GROUP BY GROUPING SETS(c1, c2);

#### RULE-20031

- Version Scope: Greater than or equal to the Altibase version tag 6.5.1.0.0 

- Type: TODO 

- Description: Multiple GROUPING SETS clauses must be converted manually 

- Original SQL Text:
  
  CREATE VIEW mgr_view
  
  AS
  
  SELECT mgr, job, comm, deptno, SUM(sal) FROM emp GROUP BY
  
  **GROUPING SETS(job), GROUPING SETS(mgr, deptno), GROUPING SETS(comm)**;

- Processed SQL Text:
  
  CREATE VIEW mgr_view
  
  AS
  
  SELECT mgr, job, comm, deptno, SUM(sal) FROM emp GROUP BY
  
  GROUPING **SETS(job), GROUPING SETS(mgr, deptno), GROUPING SETS(comm) /\* [TODO]
  RULE-20031 : Multiple GROUPING SETS clauses must be converted manually. \*/;**

#### RULE-20043

- Type: REMOVED

- Description: The EDITIONING, EDITIONABLE, and NONEDITIONABLE properties have been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE **EDITIONABLE** PROCEDURE proc1 AS
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE **/\* EDITIONABLE \*/ /\* [REMOVED] RULE-20043 : The
  EDITIONING, EDITIONABLE, and NONEDITIONABLE properties have been removed \*/**
  PROCEDURE proc1 AS
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-20044

- Type: TODO 

- Description: The partition extention clause defining values of partition key should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 PARTITION **FOR ('QA', 'RND')**;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 PARTITION **FOR ('QA', 'RND') /\* [TODO] RULE-20052 : Query partition
  clause must be converted manually \*/ /\* [TODO] RULE-20044 : The partition
  extension clause specifying key value must be converted manually \*/**;

#### RULE-20045

- Version Scope: Altibase 6.3.1.0.0 or later

- Type: TODO 

- Description: The alias of a subquery column in the WITH clause should be converted manually.

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  WITH t1**(c1, c2)** AS (SELECT \* FROM TABLE(func1))
  
  SELECT \* FROM t1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  WITH t1**(c1, c2) /\* [TODO] RULE-20045 : The column alias for subquery in the
  with clause must be converted manually \*/** AS (SELECT \* FROM TABLE(func1))
  
  SELECT \* FROM t1;

#### RULE-20046

- Version Scope: Greater than or equal to the Altibase version tag 6.1.1.0.0 

- Type: TODO 

- Description: The XML of PIVOT clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 PIVOT **XML** (SUM(c1) FOR c2 IN (ANY));

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 PIVOT **XML /\* [TODO] RULE-20046 : The XML keyword of the pivot clause
  must be converted manually \*/** (SUM(c1) FOR c2 IN (ANY));

#### RULE-20047

- Version Scope: Greater than or equal to the Altibase version tag 6.1.1.0.0 

- Type: TODO 

- Description: ANY or a subquery declared in the pivot_in_clause should be manually converted.

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 PIVOT XML (SUM(c1) FOR c2 IN (**ANY**));

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 PIVOT XML (SUM(c1) FOR c2 IN (**ANY**) **/\* [TODO] RULE-20047 : The ANY
  keyword or a subquery in the pivot_in_clause must be converted manually \*/**);

#### RULE-20048

- Type: TODO 

- Description: The SMAPLE clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 **SAMPLE(50);**

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 **SAMPLE(50) /\* [TODO] RULE-20048 : The sample clause must be converted
  manually \*/;**

#### RULE-20049

- Type: TODO 

- Description: The ROW LIMITING should be converted into the LIMIT clause. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 **OFFSET 1 ROW**;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 **OFFSET 1 ROW /\* [TODO] RULE-20049 : The row limiting clause must be
  converted to the limit clause \*/**;

#### RULE-20050

- Type: TODO 

- Description: The SKIP LOCKED in the FOR UPDATE clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  v1 NUMBER := 1;
  
  CURSOR cur1 IS SELECT c1 FROM t1 FOR UPDATE **SKIP LOCKED**;
  
  BEGIN
  
  OPEN cur1;
  
  LOOP
  
  FETCH cur1 INTO v1;
  
  EXIT WHEN cur1%NOTFOUND;
  
  DBMS_OUTPUT.PUT_LINE('v1: ' \|\| v1);
  
  END LOOP;
  
  CLOSE cur1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  v1 NUMBER := 1;
  
  CURSOR cur1 IS SELECT c1 FROM t1 FOR UPDATE **SKIP LOCKED /\* [TODO] RULE-20050
  : SKIP LOCKED in the FOR UPDATE clause must be converted manually \*/;**
  
  BEGIN
  
  OPEN cur1;
  
  LOOP
  
  FETCH cur1 INTO v1;
  
  EXIT WHEN cur1%NOTFOUND;
  
  DBMS_OUTPUT.PUT_LINE('v1: ' \|\| v1);
  
  END LOOP;
  
  CLOSE cur1;
  
  END;

#### RULE-20051

- Type: TODO 

- Description: OF...column in the FOR UPDATE clause should be manually converted.

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  v1 NUMBER := 1;
  
  CURSOR cur1 IS SELECT c1 FROM t1 FOR UPDATE **OF c1**;
  
  BEGIN
  
  OPEN cur1;
  
  LOOP
  
  FETCH cur1 INTO v1;
  
  EXIT WHEN cur1%NOTFOUND;
  
  DBMS_OUTPUT.PUT_LINE('v1: ' \|\| v1);
  
  END LOOP;
  
  CLOSE cur1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  v1 NUMBER := 1;
  
  CURSOR cur1 IS SELECT c1 FROM t1 FOR UPDATE **OF c1 /\* [TODO] RULE-20051 : OF
  ... column clause in the FOR UPDATE clause must be converted manually \*/**;
  
  BEGIN
  
  OPEN cur1;
  
  LOOP
  
  FETCH cur1 INTO v1;
  
  EXIT WHEN cur1%NOTFOUND;
  
  DBMS_OUTPUT.PUT_LINE('v1: ' \|\| v1);
  
  END LOOP;
  
  CLOSE cur1;
  
  END;

#### RULE-20052

- Type: TODO 

- Description: The query partition clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 LEFT OUTER JOIN t2 **PARTITION BY (10)** ON t1.c2 = t2.c2;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 LEFT OUTER JOIN t2 **PARTITION BY (10) /\* [TODO] RULE-20052 : Query
  partition clause must be converted manually \*/** ON t1.c2 = t2.c2;

#### RULE-20053

- Version Scope: Greater than or equal to the Altibase version tag 6.3.1.0.0 

- Type: TODO 

- Description: The WHERE clause in the MERGE statement should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  MERGE INTO t1 USING t2 ON (t1.c1 = t2.c1)
  
  WHEN MATCHED THEN UPDATE SET t1.c2 = t2.c2 **WHERE t1.c1 = 10**;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  BEGIN
  
  MERGE INTO t1 USING t2 ON (t1.c1 = t2.c1)
  
  WHEN MATCHED THEN UPDATE SET t1.c2 = t2.c2 **WHERE t1.c1 = 10 /\* [TODO]
  RULE-20053 : Where clause of MERGE statement must be converted manually \*/**;
  
  END;

#### RULE-20054

- Type: REMOVED 

- Description: The error logging clause has been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  INSERT INTO t1 VALUES('6.12') **LOG ERRORS**;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  INSERT INTO t1 VALUES('6.12') **/\* LOG ERRORS \*/ /\* [REMOVED] RULE-20054 :
  The error logging clause is removed \*/**;
  
  END;

#### RULE-20055

- Version Scope: Greater than or equal to the Altibase version tag 6.3.1.0.0 

- Type: TODO 

- Description: The DELETE WHERE clause in the MERGE statement.should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  MERGE INTO t1 USING t2 ON (t1.c1 = t2.c1)
  
  WHEN MATCHED THEN UPDATE SET t1.c2 = t2.c2 **DELETE t1.c1 = 11**;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  MERGE INTO t1 USING t2 ON (t1.c1 = t2.c1)
  
  WHEN MATCHED THEN UPDATE SET t1.c2 = t2.c2 **DELETE WHERE t1.c1 = 11 /\* [TODO]
  RULE-20055 : The DELETE WHERE clause in MERGE statement must be converted
  manually \*/**
  
  END;

#### RULE-20056

- Type: TODO 

- Description: Inserting of record type variables should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 t1%ROWTYPE) AS
  
  BEGIN
  
  INSERT INTO t1 VALUES **a1**;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 t1%ROWTYPE) AS
  
  BEGIN
  
  INSERT INTO t1 VALUES **a1 /\* [TODO] RULE-20056 : Record variable insert must
  be converted manually \*/**;
  
  END;

#### RULE-20057

- Type: TODO

- Description: Conditional insert clause must be converted manually. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  INSERT
  
  **WHEN team = 'UX' THEN INTO emp_ux**
  
  **ELSE INTO emp_etc SELECT \* FROM employees;**
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  INSERT
  
  **WHEN team = 'UX' THEN INTO emp_ux**
  
  **ELSE INTO emp_etc SELECT \* FROM employees; /\* [TODO] RULE-20057 :
  Conditional insert clause must be converted manually \*/**
  
  END;

#### RULE-20058

- Type: TODO 

- Description: The CURRENT OF clause in the WHERE clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  c1 NUMBER;
  
  CURSOR cur1 IS SELECT c1 FROM t1 FOR UPDATE;
  
  BEGIN
  
  OPEN cur1;
  
  LOOP
  
  FETCH cur1 INTO c1;
  
  IF c1 \> 10 THEN
  
  DELETE FROM t1 WHERE **CURRENT OF cur1**;
  
  END IF;
  
  EXIT WHEN cur1%NOTFOUND;
  
  END LOOP;
  
  CLOSE cur1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  c1 NUMBER;
  
  CURSOR cur1 IS SELECT c1 FROM t1 FOR UPDATE;
  
  BEGIN
  
  OPEN cur1;
  
  LOOP
  
  FETCH cur1 INTO c1;
  
  IF c1 \> 10 THEN
  
  DELETE FROM t1 WHERE **CURRENT OF cur1 /\* [TODO] RULE-20058 : 'CURRENT OF'
  clause in the WHERE clause must be converted manually \*/**;
  
  END IF;
  
  EXIT WHEN cur1%NOTFOUND;
  
  END LOOP;
  
  CLOSE cur1;
  
  END;

#### RULE-20059

- Version Scope: Altibase version tag 6.5.1.0.0 or earlier

- Type: TODO 

- Description: The TABLE function should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM **TABLE(func1('ALTIBASE'));**

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM **TABLE(func1('ALTIBASE')) /\* [TODO] RULE-20059 : Table function must be
  converted manually \*/**;

- Version Scope: Altibase version tag 6.5.1.0.0 or later 

- Type: TODO 

- Description: DML(INSERT, DELET, and UPDATE) used in the TABLE funtions should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  DELETE FROM **TABLE(SELECT c2 FROM t1)** t WHERE t.c1 = 1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  DELETE FROM TABLE(SELECT c2 FROM t1) t **/\* [TODO] RULE-20059 : The TABLE
  function with DML(insert, delete, update) must be converted manually \*/** WHERE
  t.c1 = 1;
  
  END;

#### RULE-20060

- Version Scope: Altibase version tag 6.5.1.0.0 or later 

- Type: TODO 

- Description: The (+) operator should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT t1.c1, t1_c2.c2
  
  FROM t1, TABLE(t1.c2) **(+)** t1_c2;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT t1.c1, t1_c2.c2
  
  FROM t1, TABLE(t1.c2) **(+) /\* [TODO] RULE-20060 : The (+) operator must be
  converted manually \*/** t1_c2;

#### RULE-20061

- Version Scope: Altibase version tag 6.5.1.0.0 or later

- Type: TODO 

- Description: The collection expression arguments in the TABLE function should be the user-defined function. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM TABLE(**SELECT c2 FROM t1**);

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM TABLE(**SELECT c2 FROM t1**) **/\* [TODO] RULE-20061 : The collection
  expression arguments in the TABLE function should be the user-defined function
  \*/;**

#### RULE-20062

- Type: TODO 

- Description: The ONLY clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \* FROM **ONLY(v2);**

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \* FROM **ONLY(v2) /\* [TODO] RULE-20062 : ONLY Clause must be converted
  manually \*/**;

#### RULE-20063

- Type: TODO 

- Description: The record type variables in the SET clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 t1%ROWTYPE) AS
  
  BEGIN
  
  UPDATE t1 SET **ROW = a1** WHERE c1 = a1.c1;
  
  END**;**

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  UPDATE t1 SET **ROW = a1 /\* [TODO] RULE-20063 : Record variable in SET clause
  must be converted manually \*/** WHERE c1 = a1.c1;
  
  END;

#### RULE-20065

- Type: TODO 

- Description: Subpartitions should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 **SUBPARTITION FOR ('HDB', 'HDB DA')**;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 **SUBPARTITION for ('HDB', 'HDB DA') /\* [TODO] RULE-20065 :
  SUBPARTITION must be converted manually \*/ /\* [TODO] RULE-20044 : The
  partition extension clause specifying key value must be converted manually
  \*/**;

#### RULE-20066

- Version Scope: Altibase version tag 6.1.1.0.0 or earlier

- Type: TODO 

- Description: The CROSS APPLY or OUTER APPLY join should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 **CROSS APPLY** (SELECT \* FROM t2 WHERE t1.c1 = c1);

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT \*
  
  FROM t1 **CROSS APPLY** (SELECT \* FROM t2 WHERE t1.c1 = c1) **/\* [TODO]
  RULE-20066 : CROSS APPLY or OUTER APPLY join must be converted manually \*/**;

### PSM Conversion Rules

#### RULE-30001

- Type: CONVERTED 

- Description: Unsupported data types are converted 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 **VARCHAR2**)
  
  RETURN **VARCHAR2**
  
  IS
  
  m_binary_double **BINARY_DOUBLE**;
  
  m_number **NUMBER(10)** := 1234;
  
  TYPE rt_n IS RECORD (c1 **NATURAL**);
  
  TYPE rt_nn IS RECORD (c1 **NATURAL**);
  
  TYPE tt_1 IS TABLE OF **TIMESTAMP(3)** INDEX BY VARCHAR2(10);
  
  TYPE tt_2 IS TABLE OF **TIMESTAMP(3) WITH TIME ZONE** INDEX BY VARCHAR2(10);
  
  BEGIN
  
  RETURN a1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 **VARCHAR(65534)**)
  
  RETURN **VARCHAR(65534)**
  
  IS
  
  m_binary_double **DOUBLE**;
  
  m_number **NUMBER** := 1234;
  
  TYPE rt_n IS RECORD (c1 **INTEGER**);
  
  TYPE rt_nn IS RECORD (c1 **INTEGER**);
  
  TYPE tt_1 IS TABLE OF **DATE** INDEX BY VARCHAR2(10);
  
  TYPE tt_2 IS TABLE OF **DATE** INDEX BY VARCHAR2(10);
  
  BEGIN
  
  RETURN a1;
  
  END;

#### RULE-30002

- Type: TODO 

- Description: Unsupported data types must be converted manually.

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1
  
  IS
  
  v_rowid **ROWID**;
  
  v_urowid **UROWID**;
  
  BEGIN
  
  NULL;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1
  
  IS
  
  v_rowid ROWID **/\* [TODO] RULE-30002 : Unsupported data type must be converted
  manually \*/**;
  
  v_urowid UROWID **/\* [TODO] RULE-30002 : Unsupported data type must be
  converted manually \*/**;
  
  BEGIN
  
  NULL;
  
  END;

#### RULE-30003

- Type: TODO 

- Description: If the data type of variable referencing the %TYPE were to be a userdefined or VARRAY type, it should be manually converted. 

- Original SQL Text:
  
    CREATE FUNCTION getSeason_thailand(p_date DATE) RETURN VARCHAR2
  
    AS
  
    TYPE vt_season IS VARRAY(5) OF INTEGER;
  
    rainy vt_season := vt_season(6, 7, 8, 9, 10);
  
    dry **rainy%TYPE** := vt_season(11, 12, 1, 2);
  
    v_currSeason VARCHAR2(20) := 'Unknown';
  
    v_currMonth NUMBER(2);
  
    BEGIN
  
    SELECT TO_NUMBER(TO_CHAR(p_date, 'MM')) INTO v_currMonth FROM dual;
  
    FOR i IN 1..rainy.LAST LOOP
  
    IF rainy(i) = v_currMonth THEN
  
    v_currSeason := 'Rainy season';
  
    END IF;
  
    END LOOP;
  
    FOR i IN 1..dry.LAST LOOP
  
    IF dry(i) = v_currMonth THEN
  
    v_currSeason := 'Dry season';
  
    END IF;
  
    END LOOP;
  
    RETURN v_currSeason;
  
    END;

- Processed SQL Text:
  
  CREATE FUNCTION getSeason_thailand(p_date DATE) RETURN VARCHAR2
  
  AS
  
  TYPE vt_season IS VARRAY(5) OF INTEGER;
  
  rainy vt_season := vt_season(6, 7, 8, 9, 10);
  
  **dry rainy%TYPE /\* [TODO] RULE-30003 : If the data type of variable
  referencing the %TYPE were to be a user-defined or VARRAY type, it should be
  manually converted \*/** := vt_season(11, 12, 1, 2);
  
  v_currSeason VARCHAR2(20) := 'Unknown';
  
  v_currMonth NUMBER(2);
  
  BEGIN
  
  SELECT TO_NUMBER(TO_CHAR(p_date, 'MM')) INTO v_currMonth FROM dual;
  
  FOR i IN 1..rainy.LAST LOOP
  
  IF rainy(i) = v_currMonth THEN
  
  v_currSeason := 'Rainy season';
  
  END IF;
  
  END LOOP;
  
  FOR i IN 1..dry.LAST LOOP
  
  IF dry(i) = v_currMonth THEN
  
  v_currSeason := 'Dry season';
  
  END IF;
  
  END LOOP;
  
  RETURN v_currSeason;
  
  END;

#### RULE-30004

- Type: TODO 

- Description: If the data type of variables is VARRY or the user-defined type, it should be manually converted. 

- Original SQL Text:
  
    CREATE FUNCTION getSeason_thailand(p_date DATE) RETURN VARCHAR2
  
    AS
  
    TYPE vt_season IS VARRAY(5) OF INTEGER;
  
    rainy **vt_season** := vt_season(6, 7, 8, 9, 10);
  
    dry rainy%TYPE := vt_season(11, 12, 1, 2);
  
    v_currSeason VARCHAR2(20) := 'Unknown';
  
    v_currMonth NUMBER(2);
  
    BEGIN
  
    SELECT TO_NUMBER(TO_CHAR(p_date, 'MM')) INTO v_currMonth FROM dual;
  
    FOR i IN 1..rainy.LAST LOOP
  
    IF rainy(i) = v_currMonth THEN
  
    v_currSeason := 'Rainy season';
  
    END IF;
  
    END LOOP;
  
    FOR i IN 1..dry.LAST LOOP
  
    IF dry(i) = v_currMonth THEN
  
    v_currSeason := 'Dry season';
  
    END IF;
  
    END LOOP;
  
    RETURN v_currSeason;
  
    END;

- Processed SQL Text:
  
  CREATE FUNCTION getSeason_thailand(p_date DATE) RETURN VARCHAR2
  
  AS
  
  TYPE vt_season IS VARRAY(5) OF INTEGER;
  
  rainy **vt_season /\* [TODO] RULE-30004 : If the datatype of variable is an
  VARRAY or user-defined type, the user must convert it manually \*/** :=
  vt_season(6, 7, 8, 9, 10);
  
  dry rainy%TYPE:= vt_season(11, 12, 1, 2);
  
  v_currSeason VARCHAR3(20) := 'Unknown';
  
  v_currMonth NUMBER(2);
  
  BEGIN
  
  SELECT TO_NUMBER(TO_CHAR(p_date, 'MM')) INTO v_currMonth FROM dual;
  
  FOR i IN 1..rainy.LAST LOOP
  
  IF rainy(i) = v_currMonth THEN
  
  v_currSeason := 'Rainy season';
  
  END IF;
  
  END LOOP;
  
  FOR i IN 1..dry.LAST LOOP
  
  IF dry(i) = v_currMonth THEN
  
  v_currSeason := 'Dry season';
  
  END IF;
  
  END LOOP;
  
  RETURN v_currSeason;
  
  END;

#### RULE-30005

- Type: REMOVED 

- Description: The NOT NULL constraint is removed 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  IS
  
  v1 PLS_INTEGER NOT NULL;
  
  BEGIN
  
  RETURN a1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  IS
  
  v1 PLS_INTEGER **/\***NOT NULL **\*/ /\* [REMOVED] RULE-30005 : The NOT NULL
  constraint is removed \*/**;
  
  BEGIN
  
  RETURN a1;
  
  END;

#### RULE-30006

- Version Scope: Less than the Altibase version tag 6.5.1.0.0 

- Type: REMOVED 

- Description: NOCOPY is removed 

- Original SQL Text:
  
  CREATE PROCEDURE appendSysdate
  
  (
  
  p1 IN OUT **NOCOPY** VARCHAR2
  
  )
  
  IS
  
  v_date VARCHAR2(50);
  
  BEGIN
  
  SELECT SYSDATE INTO v_date FROM dual;
  
  p1 := p1 \|\| v_date;
  
  END;

- Processed SQL Text:
  
  CREATE PROCEDURE appendSysdate
  
  (
  
  p1 IN OUT **/\* NOCOPY \*/ /\* [REMOVED] RULE-30006 : NOCOPY is removed \*/
  VARCHAR2**
  
  )
  
  IS
  
  v_date VARCHAR2(50);
  
  BEGIN
  
  SELECT SYSDATE INTO v_date FROM dual;
  
  p1 := p1 \|\| v_date;
  
  END;

#### RULE-30008

- Type: CONVERTED 

- Description: The local identifier that is an Altibase reserved word is converted by appending a postfix.

- Original SQL Text:
  
    CREATE PROCEDURE printDdlReplEnable
  
    AS
  
    **true** INTEGER := 1;
  
    BEGIN
  
    DECLARE
  
    isEnable INTEGER := printDdlReplEnable.**true**;
  
    BEGIN
  
    SELECT value1 INTO isEnable
  
    FROM v\$property WHERE name='REPLICATION_DDL_ENABLE';
  
    DBMS_OUTPUT.PUT('[Property]REPLICATION_DDL_ENABLE: ');
  
    IF isEnable = printDdlReplEnable.**true** THEN
  
    DBMS_OUTPUT.PUT_LINE('true');
  
    ELSE
  
    DBMS_OUTPUT.PUT_LINE('false');
  
    END IF;
  
    END;
  
    END;

- Processed SQL Text: 
  
  CREATE PROCEDURE printDdlReplEnable
  
  AS
  
  **true_POC** INTEGER := 1;
  
  BEGIN
  
  DECLARE
  
  isEnable INTEGER := printDdlReplEnable.**true_POC**;
  
  BEGIN
  
  SELECT value1 INTO isEnable
  
  FROM v\$property WHERE name='REPLICATION_DDL_ENABLE';
  
  DBMS_OUTPUT.PUT('[Property]REPLICATION_DDL_ENABLE:');
  
  IF isEnable = printDdlReplEnable.**true_POC** THEN
  
  DBMS_OUTPUT.PUT_LINE('true');
  
  ELSE
  
  DBMS_OUTPUT.PUT_LINE('false');
  
  END IF;
  
  END;
  
  END;

#### RULE-31001

- Type: CONVERTED 

- Description: All implicit cursors are converted to explicit cursors 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER)
  
  IS
  
  BEGIN
  
  FOR item1 IN (**SELECT c1 FROM t1**)
  
  LOOP
  
  NULL;
  
  END LOOP;
  
  FOR item2 IN (**SELECT c1 FROM t2**)
  
  LOOP
  
  NULL;
  
  END LOOP;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER)
  
  IS
  
  **CURSOR O2A_generated_cur_00 IS SELECT c1 FROM t1;**
  
  **CURSOR O2A_generated_cur_01 IS SELECT c1 FROM t2;**
  
  BEGIN
  
  FOR item1 IN **O2A_generated_cur_00**
  
  LOOP
  
  NULL;
  
  END LOOP;
  
  FOR item2 IN **O2A_generated_cur_01**
  
  LOOP
  
  NULL;
  
  END LOOP;
  
  END;

#### RULE-31002

- Type: TODO 

- Description: SUBTYPE type variables must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  IS
  
  TYPE typ1 IS RECORD ( m1 NUMBER(4) NOT NULL := 99 );
  
  TYPE typ2 IS REF CURSOR RETURN record_name%TYPE;
  
  TYPE typ3 IS TABLE OF a1%TYPE NOT NULL;
  
  TYPE typ4 iS VARYING ARRAY(10) OF INTEGER;
  
  **SUBTYPE subtyp1 IS CHAR(10);**
  
  BEGIN
  
  RETURN a1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  IS
  
  TYPE typ1 IS RECORD ( m1 NUMBER(4) NOT NULL := 99 );
  
  TYPE typ2 IS REF CURSOR RETURN record_name%TYPE;
  
  TYPE typ3 IS TABLE OF a1%TYPE NOT NULL;
  
  TYPE typ4 iS VARYING ARRAY(10) OF INTEGER;
  
  **SUBTYPE subtyp1 IS CHAR(10) /\* [TODO] RULE-31002 : SUBTYPE type variable must
  be converted manually \*/;**
  
  BEGIN
  
  RETURN a1;
  
  END;

#### RULE-31003

- Type: TODO 

- Description: VARRAY type variables must be converted manually 

- Original SQL Text:
  
  CREATE FUNCTION getSeason_korea(p_date DATE) RETURN VARCHAR2 IS
  
  TYPE **vt_season IS VARRAY(4) OF VARCHAR2(20);**
  
  v_seasonList vt_season := vt_season('Winter', 'Spring', 'Summer', 'Fall');
  
  v_currSeason VARCHAR2(20);
  
  v_currMonth NUMBER(2);
  
  BEGIN
  
  SELECT TO_NUMBER(TO_CHAR(p_date, 'MM')) INTO v_currMonth FROM dual;
  
  v_currSeason := v_seasonList(FLOOR(MOD(v_currMonth, 12) / 3 + 1));
  
  RETURN v_currSeason;
  
  END;

- Processed SQL Text:
  
  CREATE FUNCTION getSeason_korea(p_date DATE) RETURN VARCHAR2 IS
  
  TYPE **vt_season IS VARRAY(4) OF VARCHAR2(20) /\* [TODO] RULE-31003 : VARRAY
  type variable must be converted manually \*/;**
  
  v_seasonList vt_season:= vt_season('Winter', 'Spring', 'Summer', 'Fall');
  
  v_currseason VARCHAR2(20);
  
  v_currMonth NUMBER(2);
  
  BEGIN
  
  SELECT TO_NUMBER(TO_CHAR(p_date, 'MM')) INTO v_currMonth FROM dual;
  
  v_currseason := v_seasonList(FLOOR(MOD(v_currMonth, 12) / 3 + 1));
  
  RETURN a1v_currseason;
  
  END;

#### RULE-31004

- Type: TODO 

- Description: %ROWTYPE type parameters for CURSOR must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN t2%ROWTYPE
  
  IS
  
  CURSOR cur1
  
  (
  
  m1 collection_name%TYPE,
  
  m2 t1.c3%ROWTYPE
  
  ) RETURN t2%ROWTYPE
  
  IS SELECT c2, c3 FROM t1 WHERE c1 \> 10;
  
  BEGIN
  
  RETURN cur1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN t2%ROWTYPE
  
  IS
  
  CURSOR cur1
  
  (
  
  m1 collection_name%TYPE,
  
  m2 t1.c3%ROWTYPE **/\* [TODO] RULE-31004 : %ROWTYPE type parameter for CURSOR
  must be converted manually \*/**
  
  ) RETURN t2%ROWTYPE
  
  IS SELECT c2, c3 FROM t1 WHERE c1 \> 10;
  
  BEGIN
  
  RETURN cur1;
  
  END;

#### RULE-31005

- Type: TODO 

- Description: The RETURN clause of CURSOR must be converted manually 

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  IS
  
  TYPE typ1 IS REF CURSOR RETURN record_name%TYPE;
  
  CURSOR cur1 ( m1 NUMBER )
  
  RETURN NUMBER
  
  IS SELECT c2, c3 FROM t1 WHERE c1 \> 10;
  
  BEGIN
  
  RETURN a1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 NUMBER)
  
  RETURN NUMBER
  
  IS
  
  TYPE typ1 IS REF CURSOR RETURN record_name%TYPE **/\* [TODO] RULE-31005 : RETURN
  clause of CURSOR must be converted manually \*/**;
  
  CURSOR cur1 ( m1 NUMBER ) RETURN NUMBER **/\* [TODO] RULE-31005 : RETURN clause
  of CURSOR must be converted manually \*/**
  
  IS SELECT c2, c3 FROM t1 WHERE c1 \> 10;
  
  BEGIN
  
  RETURN a1;
  
  END;

#### RULE-31006

- Type: REMOVED 

- Description: Cannot define or declare procedure or function in the declare section 

- Original SQL Text:
  
  CREATE PROCEDURE util_tblMgr(p_cmd VARCHAR2, p_tblName VARCHAR2) IS
  
  **FUNCTION isTblExist(p_tblName VARCHAR2) RETURN BOOLEAN;**
  
  **FUNCTION isTblExist(p_tblName VARCHAR2) RETURN BOOLEAN AS**
  
  **v_cnt INTEGER;**
  
  **BEGIN**
  
  **SELECT COUNT(\*) INTO v_cnt FROM user_tables WHERE table_name = p_tblName;**
  
  **IF v_cnt \> 0 THEN**
  
  **RETURN true;**
  
  **ELSE**
  
  **RETURN false;**
  
  **END IF;**
  
  **END;**
  
  BEGIN
  
  CASE p_cmd
  
  WHEN 'EXIST' THEN
  
  IF isTblExist(p_tblName) THEN
  
  DBMS_OUTPUT.PUT_LINE(p_tblName \|\| ' exists.');
  
  ELSE
  
  DBMS_OUTPUT.PUT_LINE(p_tblName \|\| ' does not exist.');
  
  END IF;
  
  ELSE DBMS_OUTPUT.PUT_LINE('Unknown command: ' \|\| p_cmd);
  
  END CASE;
  
  END;

- Processed SQL Text:
  
  CREATE PROCEDURE util_tblMgr(p_cmd VARCHAR2, p_tblName VARCHAR2) IS
  
  **/\* FUNCTION isTblExist(p_tblName VARCHAR2) RETURN BOOLEAN; \*/ /\* [REMOVED]
  RULE-31006 : Cannot define or declare a procedure or function in the declare
  section \*/**
  
  **/\* FUNCTION isTblExist(p_tblName VARCHAR2) RETURN BOOLEAN AS**
  
  **v_cnt INTEGER;**
  
  **BEGIN**
  
  **SELECT COUNT(\*) INTO v_cnt FROM user_tables WHERE table_name = p_tblName;**
  
  **IF v_cnt \> 0 THEN**
  
  **RETURN true;**
  
  **ELSE**
  
  **RETURN false;**
  
  **END IF;**
  
  **END; \*/ /\* [REMOVED] RULE-31006 : Cannot define or declare a procedure or
  function in the declare section \*/**
  
  BEGIN
  
  CASE p_cmd
  
  WHEN 'EXIST' THEN
  
  IF isTblExist(p_tblName) THEN
  
  DBMS_OUTPUT.PUT_LINE(p_tblName \|\| ' exists.');
  
  ELSE
  
  DBMS_OUTPUT.PUT_LINE(p_tblName \|\| ' does not exist.');
  
  END IF;
  
  ELSE DBMS_OUTPUT.PUT_LINE('Unknown command: ' \|\| p_cmd);
  
  END CASE;
  
  END;

#### RULE-31008

- Type: REMOVED 

- Description: PRAGMA is removed. PRAGMA is removed. However, when it is removed, AUTONOMOUS_TRANSACTION is excluded in Altibase 6.3.1.0.10 or higher, and EXCEPTION_INIT is excluded in Altibase 6.5.1.0.0 or higher. 

- Original SQL Text:
  
    CREATE PROCEDURE addShot(p_cnt INTEGER)
  
    AS
  
    **PRAGMA AUTONOMOUS_TRANSACTION;**
  
    tmp_opt_empty EXCEPTION;
  
    **PRAGMA EXCEPTION_INIT(tmp_opt_empty, 100);**
  
    v_currcnt INTEGER;
  
    BEGIN
  
    SELECT shot_cnt INTO v_currcnt FROM tmp_opt;
  
    v_currcnt := v_currcnt + p_cnt;
  
    UPDATE tmp_opt SET shot_cnt = v_currcnt WHERE id = 1;
  
    COMMIT;
  
    EXCEPTION
  
    WHEN tmp_opt_empty THEN
  
    INSERT INTO tmp_opt(id, shot_cnt) VALUES (1, p_cnt + 1);
  
    COMMIT;
  
    END;

- Processed SQL Text:
  
  CREATE PROCEDURE addShot(p_cnt INTEGER)
  
  AS
  
  **/\* PRAGMA AUTONOMOUS_TRANSACTION; \*/ /\* [REMOVED] RULE-31008 : PRAGMA is
  removed \*/**
  
  tmp_opt_empty EXCEPTION;
  
  **/\* PRAGMA EXCEPTION_INIT(tmp_opt_empty, 100); \*/ /\* [REMOVED] RULE-31008 :
  PRAGMA is removed \*/**
  
  v_currcnt INTEGER;
  
  BEGIN
  
  SELECT shot_cnt INTO v_currcnt FROM tmp_opt;
  
  v_currcnt := v_currcnt + p_cnt;
  
  UPDATE tmp_opt SET shot_cnt = v_currcnt WHERE id = 1;
  
  COMMIT;
  
  EXCEPTION
  
  WHEN tmp_opt_empty THEN
  
  INSERT INTO tmp_opt(id, shot_cnt) VALUES (1, p_cnt + 1);
  
  COMMIT;
  
  END;

- Original SQL Text: Altibase 6.3.1.0.10 or above
  
  CREATE PROCEDURE addShot(p_cnt INTEGER)
  
  AS
  
  **PRAGMA AUTONOMOUS_TRANSACTION;**
  
  tmp_opt_empty EXCEPTION;
  
  **PRAGMA EXCEPTION_INIT(tmp_opt_empty, 100);**
  
  v_currcnt INTEGER;
  
  BEGIN
  
  SELECT shot_cnt INTO v_currcnt FROM tmp_opt;
  
  v_currcnt := v_currcnt + p_cnt;
  
  UPDATE tmp_opt SET shot_cnt = v_currcnt WHERE id = 1;
  
  COMMIT;
  
  EXCEPTION
  
  WHEN tmp_opt_empty THEN
  
  INSERT INTO tmp_opt(id, shot_cnt) VALUES (1, p_cnt + 1);
  
  COMMIT;
  
  END;

- Processed SQL Text:
  
  CREATE PROCEDURE addShot(p_cnt INTEGER)
  
  AS
  
  **PRAGMA AUTONOMOUS_TRANSACTION**
  
  tmp_opt_empty EXCEPTION;
  
  **/\* PRAGMA EXCEPTION_INIT(tmp_opt_empty, 100); \*/ /\* [REMOVED] RULE-31008 :
  PRAGMA is removed \*/**
  
  v_currcnt INTEGER;
  
  BEGIN
  
  SELECT shot_cnt INTO v_currcnt FROM tmp_opt;
  
  v_currcnt := v_currcnt + p_cnt;
  
  UPDATE tmp_opt SET shot_cnt = v_currcnt WHERE id = 1;
  
  COMMIT;
  
  EXCEPTION
  
  WHEN tmp_opt_empty THEN
  
  INSERT INTO tmp_opt(id, shot_cnt) VALUES (1, p_cnt + 1);
  
  COMMIT;
  
  END;

- Original SQL Text: Altibase 6.3.1.0.10 or above
  
  CREATE OR REPLACE PROCEDURE addShot(p_cnt INTEGER)
  
  AS
  
  **PRAGMA AUTONOMOUS_TRANSACTION;**
  
  tmp_opt_empty EXCEPTION;
  
  **PRAGMA EXCEPTION_INIT(tmp_opt_empty, 100);**
  
  v_currcnt INTEGER;
  
  BEGIN
  
  SELECT shot_cnt INTO v_currcnt FROM tmp_opt;
  
  v_currcnt := v_currcnt + p_cnt;
  
  UPDATE tmp_opt SET shot_cnt = v_currcnt WHERE id = 1;
  
  COMMIT;
  
  EXCEPTION
  
  WHEN tmp_opt_empty THEN
  
  INSERT INTO tmp_opt(id, shot_cnt) VALUES (1, p_cnt + 1);
  
  COMMIT;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE addShot(p_cnt INTEGER) AS
  
  **PRAGMA AUTONOMOUS_TRANSACTION;**
  
  tmp_opt_empty EXCEPTION;
  
  **PRAGMA EXCEPTION_INIT(tmp_opt_empty, 100);**
  
  v_currcnt INTEGER;
  
  BEGIN
  
  SELECT shot_cnt INTO v_currcnt FROM tmp_opt;
  
  v_currcnt := v_currcnt + p_cnt;
  
  UPDATE tmp_opt SET shot_cnt = v_currcnt WHERE id = 1;
  
  COMMIT;
  
  EXCEPTION
  
  WHEN tmp_opt_empty THEN
  
  INSERT INTO tmp_opt(id, shot_cnt) VALUES (1, p_cnt + 1);
  
  COMMIT;
  
  END;

#### RULE-31010

- Type: TODO 

- Description: The collection constructor must be converted manually 

- Original SQL Text:
  
  CREATE FUNCTION getSeason_korea(p_date DATE) RETURN VARCHAR2
  
  AS
  
  TYPE vt_season IS VARRAY(4) OF v_currSeason%TYPE;
  
  v_seasonList vt_season := **vt_season('Winter', 'Spring', 'Summer', 'Fall')**;
  
  v_currMonth NUMBER(2);
  
  v_currSeason VARCHAR2(20);
  
  BEGIN
  
  SELECT TO_NUMBER(TO_CHAR(p_date, 'MM')) INTO v_currMonth FROM dual;
  
  v_currSeason := v_seasonList(FLOOR(MOD(v_currMonth, 12) / 3 + 1));
  
  RETURN v_currSeason;
  
  END;

- Processed SQL Text:
  
  CREATE FUNCTION getSeason_korea(p_date DATE) RETURN VARCHAR(32000)
  
  AS
  
  TYPE vt_season IS VARRAY(4) OF v_currSeason%TYPE;
  
  v_seasonList **vt_season**:= **vt_season('Winter', 'Spring', 'Summer', 'Fall')
  /\* [TODO] RULE-31010 : The collection constructor must be converted manually
  \*/;**
  
  v_currMonth NUMBER(38, 0);
  
  v_currSeason VARCHAR(32000);
  
  BEGIN
  
  SELECT TO_NUMBER(TO_CHAR(p_date, 'MM')) INTO v_currMonth FROM dual;
  
  v_currSeason := v_seasonList(FLOOR(MOD(v_currMonth, 12) / 3 + 1));
  
  RETURN v_currSeason;
  
  END;

#### RULE-31011

- Type: TODO 

- Description: If the data type of associative array is defined using %TYPE or %ROWTYPE, it should be manually converted. 

- Original SQL Text:
  
    CREATE PACKAGE tripLog_pkg AS
  
    curr_date DATE := SYSDATE;
  
    TYPE at_city IS TABLE OF **curr_date%TYPE** INDEX BY VARCHAR2(100);
  
    v_cityList at_city;
  
    PROCEDURE addCity(p_city VARCHAR2, p_date DATE);
  
    PROCEDURE delCity(p_city VARCHAR2);
  
    PROCEDURE printCityList;
  
    END;

- Processed SQL Text:
  
  CREATE PACKAGE tripLog_pkg AS
  
  curr_date DATE := SYSDATE;
  
  TYPE at_city IS TABLE OF **curr_date%TYPE /\* [TODO] RULE-31011 : The %TYPE or
  %ROWTYPE attribute must be converted manually \*/** INDEX BY VARCHAR2(100);
  
  v_cityList at_city;
  
  PROCEDURE addCity(p_city VARCHAR2, p_date DATE);
  
  PROCEDURE delCity(p_city VARCHAR2);
  
  PROCEDURE printCityList;
  
  END;

#### RULE-31012

- Type: CONVERTED 

- Description: The index data type of Associative array has been converted. 

- Original SQL Text:
  
  CREATE PACKAGE tripLog_pkg AS
  
  curr_date DATE := SYSDATE;
  
  TYPE at_city IS TABLE OF **curr_date%TYPE INDEX BY VARCHAR2(100)**;
  
  v_cityList at_city;
  
  PROCEDURE addCity(p_city VARCHAR2, p_date DATE);
  
  PROCEDURE delCity(p_city VARCHAR2);
  
  PROCEDURE printCityList;
  
  END;

- Processed SQL Text
  
  CREATE PACKAGE tripLog_pkg AS
  
  curr_date DATE := SYSDATE;
  
  TYPE at_city IS TABLE OF **curr_date%TYPE INDEX BY VARCHAR(65534)**;
  
  v_cityList at_city;
  
  PROCEDURE addCity(p_city VARCHAR2, p_date DATE);
  
  PROCEDURE delCity(p_city VARCHAR2);
  
  PROCEDURE printCityList;
  
  END;

#### RULE-32001

- Version Scope:Altibase version tag 6.3.1.0.0 or earlier

- Type: REMOVED 

- Description: Cannot COMMIT while cursor is still open. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER)
  
  AS
  
  m1 INTEGER;
  
  m2 INTEGER;
  
  m3 INTEGER;
  
  m4 INTEGER;
  
  CURSOR cur1 IS
  
  SELECT c1, c2, c3, c4 FROM t1;
  
  BEGIN
  
  OPEN cur1;
  
  FOR i IN 1 .. 5 LOOP
  
  FETCH cur1 INTO m1, m2, m3, m4;
  
  EXIT WHEN cur1%NOTFOUND;
  
  INSERT INTO t2 VALUES(m1, m2, m3, m4);
  
  END LOOP;
  
  **COMMIT**;
  
  CLOSE cur1;
  
  COMMIT;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(a1 NUMBER)
  
  AS
  
  m1 INTEGER;
  
  m2 INTEGER;
  
  m3 INTEGER;
  
  m4 INTEGER;
  
  CURSOR cur1 IS
  
  SELECT c1, c2, c3, c4 FROM t1;
  
  BEGIN
  
  OPEN cur1;
  
  FOR i IN 1 .. 5 LOOP
  
  FETCH cur1 INTO m1, m2, m3, m4;
  
  EXIT WHEN cur1%NOTFOUND;
  
  INSERT INTO t2 VALUES(m1, m2, m3, m4);
  
  END LOOP;
  
  **/\* COMMIT; \*/ /\* [REMOVED] RULE-32001 : Cannot COMMIT while cursor is still
  open \*/**
  
  CLOSE cur1;
  
  COMMIT;
  
  END;

#### RULE-32002

- Version Scope: Altibase version tag 6.3.1.0.0 or earlier

- Type: REMOVED 

- Description: Cannot ROLLBACK while a cursor is still open 

- Original SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE DELETE ON t1
  
  DECLARE
  
  m1 INTEGER;
  
  m2 INTEGER;
  
  m3 INTEGER;
  
  m4 INTEGER;
  
  CURSOR cur1 IS
  
  SELECT c1, c2, c3, c4 FROM t1;
  
  BEGIN
  
  OPEN cur1;
  
  FOR i IN 1 .. 5 LOOP
  
  FETCH cur1 INTO m1, m2, m3, m4;
  
  EXIT WHEN cur1%NOTFOUND;
  
  INSERT INTO t2 VALUES(m1, m2, m3, m4);
  
  END LOOP;
  
  **ROLLBACK**;
  
  CLOSE cur1;
  
  ROLLBACK;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE TRIGGER trig1
  
  BEFORE DELETE ON t1
  
  DECLARE
  
  m1 INTEGER;
  
  m2 INTEGER;
  
  m3 INTEGER;
  
  m4 INTEGER;
  
  CURSOR cur1 IS
  
  SELECT c1, c2, c3, c4 FROM t1;
  
  BEGIN
  
  OPEN cur1;
  
  FOR i IN 1 .. 5 LOOP
  
  FETCH cur1 INTO m1, m2, m3, m4;
  
  EXIT WHEN cur1%NOTFOUND;
  
  INSERT INTO t2 VALUES(m1, m2, m3, m4);
  
  END LOOP;
  
  **/\* ROLLBACK; \*/ /\* [REMOVED] RULE-32002 : Cannot ROLLBACK while cursor is
  still open \*/**
  
  CLOSE cur1;
  
  ROLLBACK;
  
  END;

#### RULE-32003

- Type: REMOVED 

- Description: The SET TRANSACTION statement is removed 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1IS
  
  BEGIN
  
  NULL;
  
  **SET TRANSACTION READ ONLY NAME 'Test Rule 13019'**;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1IS
  
  BEGIN
  
  NULL;
  
  **/\* SET TRANSACTION READ ONLY NAME 'Test Rule 13019'; \*/ /\* [REMOVED]
  RULE-32003 : The SET TRANSACTION statement is removed \*/**
  
  END;

#### RULE-32006

- Type: CONVERTED 

- Description: The FORALL statement is converted to the FOR statement 

- Original SQL Text:
  
  CREATE PROCEDURE delEmp
  
  AS
  
  TYPE nt_state IS TABLE OF CHAR(1);
  
  stateList nt_state := nt_state('Q', 'V');
  
  BEGIN
  
  **FORALL i IN 1..stateList.LAST**
  
  **DELETE FROM employees WHERE state=stateList(i);**
  
  END;

- Processed SQL Text:
  
  CREATE PROCEDURE delEmp
  
  AS
  
  TYPE nt_state IS TABLE OF CHAR(1);
  
  stateList nt_state := nt_state('Q', 'V');
  
  BEGIN
  
  **FOR i IN 1 .. stateList.LAST LOOP**
  
  **DELETE FROM employees WHERE state=stateList(i);**
  
  **END LOOP;**
  
  END;

#### RULE-32007

- Type: TODO

- Description: The FORALL statement must be converted manually 

- Original SQL Text:
  
  CREATE PROCEDURE delEmp
  
  AS
  
  TYPE nt_state IS TABLE OF CHAR(1);
  
  stateList nt_state := nt_state('Q', 'V');
  
  BEGIN
  
  FORALL i IN **INDICES OF stateList**
  
  DELETE FROM employees WHERE state=stateList(i);
  
  END;

- Processed SQL Text:
  
  CREATE PROCEDURE delEmp
  
  AS
  
  TYPE nt_state IS TABLE OF CHAR(1);
  
  stateList nt_state:= nt_state('Q', 'V');
  
  BEGIN
  
  FORALL i IN **INDICES OF stateList**
  
  DELETE FROM employees WHERE state=stateList(i); **/\* [TODO] RULE-32007 : The
  FORALL statement must be converted manually \*/**
  
  END;

#### RULE-32008

- Type: CONVERTED 

- Description: A whitespace is appended before and after the range value in the FOR LOOP statement 

- Original SQL Text:
  
  CREATE FUNCTION getCityList RETURN tripLog_pkg.nt_city PIPELINED AS
  
  BEGIN
  
  FOR i IN 1**..**tripLog_pkg.v_cityList.LAST LOOP
  
  PIPE ROW(tripLog_pkg.v_cityList(i));
  
  END LOOP;
  
  RETURN;
  
  END;

- Processed SQL Text:
  
  CREATE FUNCTION getCityList
  
  RETURN tripLog_pkg.nt_city PIPELINED AS
  
  BEGIN
  
  FOR i IN 1 **..** tripLog_pkg.v_cityList.LAST LOOP
  
  PIPE ROW(tripLog_pkg.v_cityList(i));
  
  END LOOP;
  
  RETURN;
  
  END;

#### RULE-32009

- Type: CONVERTED 

- Description: The condition in the CONTINUE statement is converted 

- Original SQL Text:
  
  CREATE PROCEDURE showMail(p_from DATE)
  
  AS
  
  v_cnt INTEGER;
  
  v_title VARCHAR2(256);
  
  v_date DATE;
  
  BEGIN
  
  SELECT COUNT(\*) INTO v_cnt FROM mailbox;
  
  IF v_cnt \> 0 THEN
  
  FOR i IN 1..v_cnt LOOP
  
  SELECT datetime INTO v_date FROM mailbox WHERE id = i;
  
  IF p_from != SYSDATE THEN
  
  **CONTINUE WHEN v_date \< p_from;**
  
  END IF;
  
  SELECT title INTO v_title FROM mailbox WHERE id = i;
  
  DBMS_OUTPUT.PUT_LINE('Title: ' \|\| v_title \|\| ', Date: ' \|\| v_date);
  
  END LOOP;
  
  END IF;
  
  END;

- Processed SQL Text:
  
  CREATE PROCEDURE showMail(p_from DATE)
  
  AS
  
  v_cnt INTEGER;
  
  v_title VARCHAR2(256);
  
  v_date DATE;
  
  BEGIN
  
  SELECT COUNT(\*) INTO v_cnt FROM mailbox;
  
  IF v_cnt \> 0 THEN
  
  FOR i IN 1 .. v_cnt LOOP
  
  SELECT datetime INTO v_date FROM mailbox WHERE id = i;
  
  **IF p_from != SYSDATE THEN**
  
  **IF v_date \< p_from THEN**
  
  **CONTINUE;**
  
  **END IF;**
  
  END IF;
  
  SELECT title INTO v_title FROM mailbox WHERE id = i;
  
  SYSTEM_.PRINTLN('Title: ' \|\| v_title \|\| ', Date: ' \|\| v_date);
  
  END LOOP;
  
  END IF;
  
  END;

#### RULE-32010

- Type: TODO 

- Description: The host variabels should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  SELECT c2 BULK COLLECT INTO **:v_arr** FROM t1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  SELECT c2 BULK COLLECT INTO **:v_arr /\* [TODO] RULE-32010 : The host variable
  must be converted manually \*/** FROM t1;
  
  END;

#### RULE-32012

- Type: TODO 

- Description: The host variabels should be manually converted. 

- Original SQL Text:
  
  CREATE FUNCTION getCitiList
  
  RETURN tripLog_pkg.nt_city PIPELINED
  
  AS
  
  BEGIN
  
  FOR i IN 1..tripLog_pkg.v_cityList.LAST LOOP
  
  **PIPE ROW(tripLog_pkg.v_cityList(i));**
  
  END LOOP;
  
  RETURN;
  
  END;

- Processed SQL Text:
  
  CREATE FUNCTION getCitiList
  
  RETURN tripLog_pkg.nt_city PIPELINED
  
  AS
  
  BEGIN
  
  FOR i IN 1..tripLog_pkg.v_cityList.LAST LOOP
  
  **PIPE ROW(tripLog_pkg.v_cityList(i)) /\* [TODO] RULE-32012 : The PIPE ROW
  statement must be converted manually \*/;**
  
  END LOOP;
  
  RETURN;
  
  END;

#### RULE-32013

- Type: CONVERTED 

- Description: The label in the CONTINUE statement is converted 

- Original SQL Text:
  
  CREATE PROCEDURE showMail(p_from DATE)
  
  AS
  
  v_cnt INTEGER;
  
  v_title VARCHAR2(256);
  
  v_date DATE;
  
  BEGIN
  
  SELECT COUNT(\*) INTO v_cnt FROM mailbox;
  
  IF v_cnt \> 0 THEN
  
  \<\<for_loop\>\>
  
  FOR i IN REVERSE 1..v_cnt LOOP
  
  SELECT datetime INTO v_date FROM mailbox WHERE id = i;
  
  IF v_date \<= p_from THEN
  
  **CONTINUE for_loop**;
  
  END IF;
  
  SELECT title INTO v_title FROM mailbox WHERE id = i;
  
  DBMS_OUTPUT.PUT_LINE('Title: ' \|\| v_title \|\| ', Date: ' \|\| v_date);
  
  END LOOP for_loop;
  
  END IF;
  
  END;

- Processed SQL Text:
  
  CREATE PROCEDURE showMail(p_from DATE)
  
  AS
  
  v_cnt INTEGER;
  
  v_title VARCHAR2(256);
  
  v_date DATE;
  
  BEGIN
  
  SELECT COUNT(\*) INTO v_cnt FROM mailbox;
  
  IF v_cnt \> 0 THEN
  
  \<\<for_loop\>\>
  
  FOR i IN REVERSE 1 .. v_cnt LOOP
  
  SELECT datetime INTO v_date FROM mailbox WHERE id = i;
  
  IF v_date \<= p_from THEN
  
  **GOTO O2A_generated_label_00;**
  
  END IF;
  
  SELECT title INTO v_title FROM mailbox WHERE id = i;
  
  SYSTEM_.PRINTLN('Title: ' \|\| v_title \|\| ', Date: ' \|\| v_date);
  
  **\<\<O2A_generated_label_00\>\>**
  
  **NULL;**
  
  END LOOP for_loop;
  
  END IF;
  
  END;

#### RULE-32014

- Type: TODO 

- Description: SCN (System Change Number) cannot be assigned to the transaction 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  COMMIT FORCE 'ORCL.C50E231F042A.10.5.109239', **143217566**;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  COMMIT FORCE 'ORCL.C50E231F042A.10.5.109239', **143217566 /\* [TODO] RULE-32014
  : SCN cannot be assigned to the transaction \*/;**
  
  END;

#### RULE-32015

- Type: TODO 

- Description: The corrupt transaction cannot be committed 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  COMMIT FORCE **CORRUPT_XID_ALL;**
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  COMMIT FORCE **CORRUPT_XID_ALL; /\* [TODO] RULE-32015 : The corrupt transaction
  cannot be committed \*/**
  
  END;

#### RULE-32016

- Type: REMOVED 

- Description: The WRITE clause in the COMMIT statement is removed 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1
  
  AS
  
  v_cnt INTEGER;
  
  BEGIN
  
  SELECT COUNT(\*) INTO v_cnt FROM t1;
  
  INSERT INTO t1 VALUES(v_cnt, CURRENT_TIMESTAMP);
  
  **COMMIT WRITE NOWAIT IMMEDIATE;**
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1
  
  AS
  
  v_cnt INTEGER;
  
  BEGIN
  
  SELECT COUNT(\*) INTO v_cnt FROM t1;
  
  INSERT INTO t1 VALUES(v_cnt, CURRENT_TIMESTAMP);
  
  COMMIT **/\* WRITE NOWAIT IMMEDIATE \*/ /\* [REMOVED] RULE-32016 : The WRITE
  clause in the COMMIT statement is removed \*/;**
  
  END;

#### RULE-32017

- Type: REMOVED 

- Description: The COMMENT clause in the COMMIT statement is removed 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  COMMIT **COMMENT 'PROCEDURE proc1 committed';**
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  COMMIT **/\* COMMENT 'PROCEDURE proc1 committed' \*/ /\* [REMOVED] RULE-32017 :
  The COMMENT clause in the COMMIT statement is removed \*/;**
  
  END;

#### RULE-32018

- Type: CONVERTED 

- Description: The TO SAVEPOINT clause in the ROLLBACK statement is converted 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  **ROLLBACK TO sp1;**
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  **ROLLBACK TO SAVEPOINT sp1;**
  
  END;

#### RULE-32019

- Type: REMOVED 

- Description: The label in the CASE statement has been removed. 

- Original SQL Text
  
  CREATE OR REPLACE FUNCTION func1(a1 VARCHAR2)
  
  RETURN VARCHAR2 AS
  
  v1 VARCHAR2(25);
  
  BEGIN
  
  \<\<test\>\>
  
  CASE UPPER(a1)
  
  WHEN 'ROCK' THEN v1 := 'Paper';
  
  WHEN 'PAPER' THEN v1 := 'Scissor';
  
  WHEN 'SCISSOR' THEN v1 := 'Rock';
  
  ELSE v1 := 'Unavailable input value';
  
  **END CASE test;**
  
  RETURN v1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(a1 VARCHAR2)
  
  RETURN VARCHAR2 AS
  
  v1 VARCHAR2(25);
  
  BEGIN
  
  \<\<test\>\>
  
  CASE UPPER(a1)
  
  WHEN 'ROCK' THEN v1 := 'Paper';
  
  WHEN 'PAPER' THEN v1 := 'Scissor';
  
  WHEN 'SCISSOR' THEN v1 := 'Rock';
  
  ELSE v1 := 'Unavailable input value';
  
  END CASE ;
  
  RETURN v1;
  
  END;

#### RULE-32020

- Version Scope : Altibase 6.5.1.0.0 or earlier

- Type: TODO 

- Description: The BULK COLLECT INTO clause of the FETCH statement should be 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  TYPE at_1 IS TABLE OF NUMBER;
  
  CURSOR cur1 IS SELECT c1 FROM t1;
  
  arr1 at_1;
  
  BEGIN
  
  OPEN cur1;
  
  **FETCH cur1 BULK COLLECT INTO arr1;**
  
  DBMS_OUTPUT.PUT_LINE(arr1.COUNT);
  
  CLOSE cur1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  TYPE at_1 IS TABLE OF NUMBER;
  
  CURSOR cur1 IS SELECT c1 FROM t1;
  
  arr1 at_1;
  
  BEGIN
  
  OPEN cur1;
  
  **FETCH cur1 BULK COLLECT INTO arr1 /\* [TODO] RULE-32020 : BULK COLLECT INTO
  clause of the FETCH statement must be converted manually \*/;**
  
  DBMS_OUTPUT.PUT_LINE(arr1.COUNT);
  
  CLOSE cur1;
  
  END;

#### RULE-32021

- Type: TODO 

- Description: The dynamic RETURNING clause should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  v1 t1%ROWTYPE;
  
  BEGIN
  
  **EXECUTE IMMEDIATE 'DELETE FROM t1 WHERE c1=SYSDATE' RETURNING INTO v1;**
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  v1 t1%ROWTYPE;
  
  BEGIN
  
  **EXECUTE IMMEDIATE 'DELETE FROM t1 WHERE c1=SYSDATE' RETURNING INTO v1 /\*
  [TODO] RULE-32021 : Dynamic returning clause must be converted manually \*/;**
  
  END;

#### RULE-32022

- Type: REMOVED 

- Description: THE in front of the subquery has been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  v1 NUMBER;
  
  BEGIN
  
  SELECT t1.c1 INTO v1
  
  FROM **THE** (SELECT EXTRACT(MONTH FROM SYSDATE) curr_month FROM dual) dt, t1
  
  WHERE t1.c2 = dt.curr_month;
  
  DBMS_OUTPUT.PUT_LINE(v1);
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  v1 NUMBER;
  
  BEGIN
  
  SELECT t1.c1 INTO v1
  
  FROM **/\* THE \*/ /\* [REMOVED] RULE-32022 : The THE keyword is removed \*/**
  (SELECT EXTRACT(MONTH FROM SYSDATE) curr_month FROM dual) dt, t1
  
  WHERE t1.c2 = dt.curr_month;
  
  DBMS_OUTPUT.PUT_LINE(v1)**;**
  
  END;

#### RULE-32024

- Type: REMOVED 

- Description: The target procedure has been removed. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 ( p_emp_no IN integer ) AS
  
  v1 NUMBER;
  
  BEGIN
  
  **DBMS_OUTPUT.ENABLE;**
  
  SELECT i1 INTO v1
  
  FROM t1 WHERE i1 = p_emp_no;
  
  DBMS_OUTPUT.PUT_LINE( 'i1 : ' \|\| v1 );
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 ( p_emp_no IN integer ) AS
  
  v1 NUMBER;
  
  BEGIN
  
  **/\* DBMS_OUTPUT.ENABLE; \*/ /\* [REMOVED] RULE-32024 : The target procedure
  has been removed \*/**
  
  SELECT i1 INTO v1
  
  FROM t1 WHERE i1 = p_emp_no;
  
  DBMS_OUTPUT.PUT_LINE( 'i1 : ' \|\| v1 );
  
  END;

#### RULE-33001

- Type: TODO 

- Description: This is an exception which is not supported. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  NULL;
  
  EXCEPTION
  
  WHEN **ACCESS_INTO_NULL** THEN
  
  DBMS_OUTPUT.PUT_LINE('Exception Name: ACCESS_INTO_NULL, Error Code: -6530');
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  NULL;
  
  EXCEPTION
  
  WHEN **ACCESS_INTO_NULL /\* [TODO] RULE-33001 : Unsupported exception \*/** THEN
  
  DBMS_OUTPUT.PUT_LINE('Exception Name: ACCESS_INTO_NULL, Error Code: -6530');
  
  END;
  
  /

#### RULE-33002

- Version Scope : Altibase 6.3.1.0.0 or later

- Type: TODO 

- Description: The user should check whether the built-in package is installed in Altibase. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  NULL;
  
  EXCEPTION
  
  WHEN **UTL_FILE.INVALID**\_FILENAME THEN
  
  DBMS_OUTPUT.PUT_LINE('Exception Name: UTL_FILE.INVALID_FILENAME, Error Code:');
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  NULL;
  
  EXCEPTION
  
  WHEN **UTL_FILE.INVALID_FILENAME /\* [TODO] RULE-33002 : Confirm the target
  built-in package is installed at Altibase \*/** THEN
  
  DBMS_OUTPUT.PUT_LINE('Exception Name: UTL_FILE.INVALID_FILENAME, Error Code:');
  
  END;

#### RULE-33003

- Type: CONVERTED 

- Description: The exception has been converted according to Altibase grammar. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  NULL;
  
  EXCEPTION
  
  WHEN **UTL_FILE.INVALID_PATH** THEN
  
  DBMS_OUTPUT.PUT_LINE('Exception Name: UTL_FILE.INVALID_PATH, Error Code:');
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  NULL;
  
  EXCEPTION
  
  WHEN **INVALID_PATH** THEN
  
  DBMS_OUTPUT.PUT_LINE('Exception Name: UTL_FILE.INVALID_PATH, Error Code:');
  
  END;

### Expression Conversion Rules

#### RULE-40001

- Type: CONVERTED 

- Description: The built-in package has been converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  **DBMS_OUTPUT.PUT('Hello');**
  
  **DBMS_OUTPUT.PUT_LINE('world!');**
  
  END;

- 변환된 SQL 문장:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  **SYSTEM_.PRINT('Hello ');**
  
  **SYSTEM_.PRINTLN('world!');**
  
  END;

#### RULE-40002

- Version Scope: Altibase 6.3.1.0.0 or later

- Type: TODO

- Description: The user should chek whether the target built-in package is installed in Altibase.

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  **DBMS_OUTPUT.NEW_LINE;**
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1 AS
  
  BEGIN
  
  **DBMS_OUTPUT.NEW_LINE /\* [TODO] RULE-40002 : Confirm the target built-in
  package is installed at Altibase \*/;**
  
  END;

#### RULE-40003

- Type: TODO 

- Description: The target built-in packages should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(p_file FILE_TYPE) AS
  
  BEGIN
  
  **UTL_FILE.PUTF(p_file, 'Hello %s!', 'world');**
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1(p_file FILE_TYPE) AS
  
  BEGIN
  
  **UTL_FILE.PUTF(p_file, 'Hello %s!', 'world') /\* [TODO] RULE-40003 : The target
  built-in package must be converted manually \*/;**
  
  END;

#### RULE-40004

- Type: CONVERTED 

- Description: The target SQL functions have been converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT **UID** FROM dual;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT **USER_ID()** FROM dual;

#### RULE-40005

- Type: TODO 

- Description: This function is not supported.

- Original SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(p1 VARCHAR2)
  
  RETURN NUMBER AS
  
  v1 NUMBER := **LENGTHC(p1)**;
  
  BEGIN
  
  RETURN v1;
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE FUNCTION func1(p1 VARCHAR2)
  
  RETURN NUMBER AS
  
  v1 NUMBER := **LENGTHC(p1) /\* [TODO] RULE-40005 : Unsupported function \*/;**
  
  BEGIN
  
  RETURN v1;;
  
  END;

#### RULE-40006

- Type: CONVERTED 

- Description: The arguments of TRIM have been converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW stats AS
  
  SELECT **TRIM(LEADING 0 FROM total_stats)**
  
  FROM test_result WHERE date = SYSDATE;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW stats AS
  
  SELECT **LTRIM(total_stats, 0)**
  
  FROM test_result WHERE date = SYSDATE;

#### RULE-40007

- Type: CONVERTED 

- Description: The arguments of BIN_TO_NUM function have been converted by being connected with '||'. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW status_view AS
  
  SELECT **BIN_TO_NUM(cp_plan, hp_plan, tv_plan, net_plan)** status
  
  FROM service_tbl WHERE ym = TO_CHAR(SYSDATE, 'YYYYMM');

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW status_view AS
  
  SELECT **BIN_TO_NUM(cp_plan \|\| hp_plan \|\| tv_plan \|\| net_plan)** status
  
  FROM service_tbl WHERE ym = TO_CHAR(SYSDATE, 'YYYYMM');

#### RULE-40008

- Type: TODO

- Description: The CAST function containing a subquery as an argument should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT c1, **CAST(MULTISET(SELECT c1 FROM t2 ORDER BY c2) AS tmp_tbl)**
  
  FROM t1 ORDER BY c1;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT c1, **CAST(MULTISET(SELECT c1 FROM t2 ORDER BY c2) AS tmp_tbl) /\* [TODO]
  RULE-40008 : The CAST function containing a subquery as an argument should be
  manually converted \*/**
  
  FROM t1 ORDER BY c1;

#### RULE-40009

- Type: TODO

- Description: The DUMP function contains multiple arguments should be manually converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT **DUMP(c3, 8, 3, 2)**
  
  FROM t1 WHERE c3 = 100 ORDER BY c2;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW v1 AS
  
  SELECT **DUMP(c3, 8, 3, 2) /\* [TODO] RULE-40009 : The DUMP function contains
  multiple arguments should be manually converted \*/**
  
  FROM t1 WHERE c3 = 100 ORDER BY c2;

#### RULE-40010

- Type: CONVERTED 

- Description: The EXTRACT function has been converted. 

- Original SQL Text:
  
  CREATE OR REPLACE VIEW rsvStats_year AS
  
  SELECT **EXTRACT(YEAR FROM rsv_date)** year, COUNT(\*) cnt
  
  FROM rsv_table GROUP BY **EXTRACT(YEAR FROM rsv_date)**;

- Processed SQL Text:
  
  CREATE OR REPLACE VIEW rsvStats_year AS
  
  SELECT **EXTRACT(rsv_date, 'YEAR')** year, COUNT(\*) cnt
  
  FROM rsv_table GROUP BY **EXTRACT(rsv_date, 'YEAR')**;

#### RULE-40011

- Type: TODO 

- Description: The datetime field prefixed with 'TIMEZONE' in the EXTRACT function should be manually converted. 

- Original SQL Text:
  
    CREATE VIEW view1 AS
  
    SELECT EXTRACT(**TIMEZONE_REGION** FROM CURRENT_TIMESTAMP) FROM dual;

- Processed SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT EXTRACT(**TIMEZONE_REGION /\* [TODO] RULE-40011 : The datetime field
  prefixed 'TIMEZONE' in the EXTRACT function should be manually converted \*/**
  FROM CURRENT_TIMESTAMP) FROM dual;

#### RULE-40012

- Type: TODO 

- Description: The EXTRACT function containing XMLType instance as parameters should be manually converted. 

- Original SQL Text:
  
    CREATE VIEW view1 AS
  
    SELECT **EXTRACT(emp_into, 'Employee/Name')** emp_name FROM dual;

- Processed SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT **EXTRACT(emp_info, 'Employee/Name') /\* [TODO] RULE-40012 : The EXTRACT
  function containing XMLType instance as parameters should be manually converted
  \*/** emp_name FROM dual;

#### RULE-40013

- Type: CONVERTED 

- Description: The SYS_CONTEXT function has been converted. 

- Original SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT SYS_CONTEXT('USERENV', 'SESSION_USER') FROM dual;

- Processed SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT USER_NAME() FROM dual;

#### RULE-40014

- Type: TODO

- Description: The precision degree, which is an optional argument, of the CURRENT_TIMESTAMP function should be manually converted. 

- Original SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT **CURRENT_TIMESTAMP(0)** FROM dual;

- Processed SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT CURRENT_TIMESTAMP**(0) /\* [TODO] RULE-40014 : The optional argument of
  the function CURRENT_TIMESTAMP, precision must be converted manually \*/** FROM
  dual;

#### RULE-40015

- Type: TODO 

- Description: The nlsparam,which is an optional argument, specifying languages should be manually converted. 

- Original SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT TO_CHAR(SYSDATE, 'DL', **'NLS_DATE_LANGUAGE = korean'**) FROM dual;

- Processed SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT TO_CHAR(SYSDATE, 'DL', **'NLS_DATE_LANGUAGE = korean' /\* [TODO]
  RULE-40015 : The optional argument, nlsparam must be converted manually \*/)**
  FROM dual;

#### RULE-40016

- Type: TODO 

- Description: The optional argument match_param, which may affect operating a function, should be manually converted. 

- Original SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT REGEXP_SUBSTR(content, '(Name: )(([a-z]+) ([a-z]+))', 1, 1, **'i'**,
  **3**) "First Name" FROM page_pi;

- Processed SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT REGEXP_SUBSTR(content, '(Name: )(([a-z]+) ([a-z]+))', 1, 1, **'i' /\*
  [TODO] RULE-40016 : The optional argument, match_param must be converted
  manually \*/, 3**) "First Name" FROM page_pi;

#### RULE-40017

- Version Scope: Altibase 6.3.1.0.0 or above 

- Type: TODO 

- Description: The optional argument subexpr should be manually converted. 

- Original SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT REGEXP_SUBSTR(content, '(Name: )(([a-z]+) ([a-z]+))', 1, 1, 'i', **4**)
  "Family Name" FROM page_pi;

- Processed SQL Text:
  
  CREATE VIEW view1 AS
  
  SELECT REGEXP_SUBSTR(content, '(Name: )(([a-z]+) ([a-z]+))', 1, 1, 'i', **4 /\*
  [TODO] RULE-40017 : The optional argument, subexpr must be converted manually
  \*/**) "Family Name" FROM page_pi;

#### RULE-40018

- Type: CONVERTED 

- Description: The MOD operator has been converted as a function. 

- Original SQL Text:
  
  CREATE FUNCTION func1(p1 PLS_INTEGER) RETURN PLS_INTEGER AS
  
  v1 PLS_INTEGER := **p1 MOD 2**;
  
  BEGIN
  
  RETURN v1;
  
  END;

- Processed SQL Text:
  
  CREATE FUNCTION func1(p1 PLS_INTEGER) RETURN PLS_INTEGER AS
  
  v1 PLS_INTEGER := **MOD(p1, 2)**;
  
  BEGIN
  
  RETURN v1;
  
  END;

#### RULE-40019

- Type: CONVERTED 

- Description: The built-in package has been converted.

- Original SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1
  
  AS
  
  BEGIN
  
  DBMS_MVIEW.REFRESH('CAL_MONTH_SALES_MV, FWEEK_PSCAT_SALES_MV', 'CF', '', TRUE,
  FALSE, 0,0,0, FALSE, FALSE);
  
  END;

- Processed SQL Text:
  
  CREATE OR REPLACE PROCEDURE proc1
  
  AS
  
  BEGIN
  
  REFRESH_MATERIALIZED_VIEW(USER_NAME(), 'CAL_MONTH_SALES_MV');
  
  REFRESH_MATERIALIZED_VIEW(USER_NAME(), 'FWEEK_PSCAT_SALES_MV');
  
  END;

#### RULE-40020

- Type: CONVERTED 

- Description: The WM_CONCAT function has been converted to the LISTAGG function. 

- Original SQL Text:
  
  SELECT WM_CONCAT(val) FROM t1;

- Processed SQL Text:
  
  SELECT LISTAGG(val, ',') WITHIN GROUP(ORDER BY val) FROM t1;

#### RULE-40021

- Type: TODO 

- Description: The parameter in the function 'SYS_CONTEXT' should be converted manually. 

- Original SQL Text:
  
  CREATE VIEW v_r40021 AS SELECT **SYS_CONTEXT('USERENV', 'INSTANCE_NAME')** FROM
  dual;

- Processed SQL Text:
  
  CREATE VIEW v_r40021 AS SELECT **SYS_CONTEXT('USERENV', 'INSTANCE_NAME') /\*
  [TODO] RULE-40021 : The parameter in the function 'SYS_CONTEXT' should be
  converted manually. \*/** FROM dual;

#### RULE-40022

- Type: CONVERTED

- Description: The third argument of the function 'SYS_CONTEXT', which indicates the length of the return value, is converted to the function 'SUBSTR' surrounding 'SYS_CONTEXT'.

- Original SQL Text:

CREATE VIEW v_r40022 AS SELECT **SYS_CONTEXT('USERENV', 'INSTANCE_NAME', 100)**
FROM dual;

- Processed SQL Text:

CREATE VIEW v_r40022 AS SELECT **SUBSTR(SYS_CONTEXT('USERENV', 'INSTANCE_NAME'),
0, 100)** FROM dual;

<br/>

