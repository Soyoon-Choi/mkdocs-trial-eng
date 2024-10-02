# Appendix D. Sample Applications

This Appendix gives the location of the sample programs used in the manual.

### Executing the Sample Applications

#### Elements

The $ALTIBASE_HOME/sample/APRE directory includes sample files, header files, schema creation files, and makefiles. 

The list of included files is as follows:

```
argument.sc
arrays1.sc
arrays2.sc
binary.sc
connect1.sc
connect2.sc
cparsefull.sc
cursor1.sc
cursor2.sc
date.sc
declare_stmt.sc
delete.sc
dynamic1.sc
dynamic2.sc
dynamic3.sc
free.sc
indicator.sc
insert.sc
mc1.sc
mc2.sc
mc3.sc
mt1.sc
mt2.sc
pointer.sc
psm1.sc
psm2.sc
runtime_error_check.sc
select.sc
update.sc
varchar.sc
whenever1.sc
whenever2.sc
include/hostvar.h
include/hostvar2.h
schema/schema.sql
Makefile
```

#### Installation

When Altibase is installed, the $ALTIBASE_HOME/sample/APRE directory will be automatically created. For more information about installing Altibase, please refer to the *Altibase Installation Guide*.

#### Execution

The user can use the makefile saved in the $ALTIBASE_HOME/sample/APRE directory to create an executable file. 

The Makefile included in example program is written by gmake rule, so gmake must be installed and then use it. Otherwise, an error will be generated when creating the executable file using the Makefile.

##### Compile

```
make file_name
```

[Example1] This example shows how to compile the delete.sc sample program.

```
$ make delete
apre -t cpp delete.sc
-----------------------------------------------------------------
     Altibase C/C++ Precompiler.
     Release Version 7.1.0.0.0
     Copyright 2000, Altibase Corporation or its subsidiaries.
     All Rights Reserved.
-----------------------------------------------------------------
g++ -D_GNU_SOURCE -W -Wall -pipe -D_POSIX_PTHREAD_SEMANTICS -D_POSIX_THREADS -D_POSIX_THREAD_SAFE_FUNCTIONS -D_REENTRANT -DPDL_HAS_AIO_CALLS -g -DDEBUG -fno-implicit-templates -fno-exceptions -fcheck-new -DPDL_NO_INLINE -DPDL_LACKS_PDL_TOKEN -DPDL_LACKS_PDL_OTHER -c -I/home/trunk/work/altidev4/altibase_home/include -I. -o delete.o delete.cpp

g++ -L. -g -L/home/trunk/work/altidev4/altibase_home/lib -o delete delete.o -lapre -lodbccli -ldl -lpthread -lcrypt -lrt
```

##### Execution

```
./file_name
```

[Example2] This example shows how to execute delete, the executable file created using the delete.sc sample application, and how to check the result.

```
$ ./delete
<DELETE>
-------------------------------------------------------
[Scalar Host Variables]                                           
-------------------------------------------------------
7 rows deleted
```

### Table Information of the Example Programs

Please refer to Appendix A in the *SQL Reference*.

