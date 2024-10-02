# Appendix E. FAQ

### Precompiler FAQ

#### C/C++ Precopiler Related FAQs

##### Why were incorrect data inserted into my database table?

The application I created using APRE generates data and inserts the data into a database table. However, I have noticed that my application inserts unexpected values. 

##### Answer

One of the things to keep in mind when using APRE to develop an application is whether it is appropriate to declare host variables as local variables or global variables. Please check whether you have declared all of your host variables properly.

#### Compiler Related FAQ

##### An error occurred when attempting to link libraries in HP-UX. Why?

A link error occurred in HP-UX upon library linking. 

I used the following C compiler options

```
$ cc +DA2.0W -I../include -I/user1/altibase/altibase_home/include/ -I/user1/asn1/include -Ae -D_REENTRANT -DOAM -DSRC_LINE -DDEBUG -DDETAIL_DEBUG -g -c dbinit.c
```

I then linked the resultant object files as follows

```
$ cc +DA2.0W -o MKTDBD dbfunc.o main.o util.o dbif.o shm_msg.o file.o dbinit.o dbresult.o -L/user1/altibase/altibase_home/lib -L/user1/main/KTSLEE/lib -lcom -lprice.1.1.0 -lodbccli -lsesc -lxti -lpthread -lrt -ldld
```

##### Answer

From the execution results, it looks like you used a C compiler, rather than a C++ compiler, for the linking operation.

If you use a C++ compiler, the system library is automatically added during the compiling and linking operations. However, when using a C compiler, you have to add the system libraries manually, as shown below.

```
LIBS += -ldl -lstd -lstream -lCsup -lm -lcl -lc
```

Then make the following changes.

```
$ cc +DA2.0W -o MKTDBD dbfunc.o main.o util.o dbif.o shm_msg.o file.o dbinit.o dbresult.o -L/user1/altibase/altibase_home/lib -L/user1/main/KTSLEE/lib -lcom -lprice.1.1.0 -lodbccli -lsesc -lxti -lpthread -lrt -ldld -ldl -lstd -lstream -lCsup -lm -lcl –lc
$ cc +DA2.0W -o MKTDBD dbfunc.o main.o util.o dbif.o shm_msg.o file.o dbinit.o dbresult.o -L/user1/altibase/altibase_home/lib -L/user1/main/KTSLEE/lib -lcom -lprice.1.1.0 -lodbccli -lsesc -lxti -lpthread -lrt -ldld
```

##### If I want to query data in an MMDB on an authentication server from other servers, on which server should I compile the library file?

I understand that in order to query data in an MMDB on an authentication server from other servers, I will need to access the authentication server with an application written using APRE. 

To accomplish this, do I have to put APRE on all of the other servers and independently make a library for the executable files on each server? Or can I just create the library on the authentication server, distribute it to the other servers, and link them to create executable files? 

If I do it the first way, I have to put APRE on every server that accesses the authentication server, which is painful, and will be difficult to manage in the long term. If I do it the second way, that is, if I create a library on the authentication server, distribute it, and compile and link it on the other servers, will I encounter problems if the servers are different hardware and don't run the same OS? 

I would appreciate a suggestion as to how to go about this, and am curious about how your other clients have tackled this problem. 

Please provide a sample makefile that I should use in such a scenario

##### Answer

Unfortunately, when working with heterogeneous operating systems, there is no way to avoid installing the Altibase client module, which is required for compiling and linking, on all of the other servers. In the typical scenario, in which all machines are running the same OS, the program is compiled on a dedicated compiler machine or standard machine, and then only the library for executable files needs to be copied to the other servers. When working with a variety of operating systems, however, the linker must be available on all of the machines (because it refers to the system library for that OS). 

The makefile is in the $ALTIBASE_HOME/sample/APRE directory. Be sure to use the makefile with the libapre.a and libodbccli.a libraries provided by us when linking. Additionally, the altibase_env.mk contains all of the essential information required to compile and link APRE projects.

##### A compile error occurred while I was testing a simple piece of source code. Why?

It seems to me that there is a C++ standard I/O conflict between Altibase and g++.

##### Answer

The following example shows how to use gcc to compile an application:

```
$ apre -t cpp conn.sc
$ g++ -W -Wall -Wpointer-arith -pipe -D_POSIX_PTHREAD_SEMANTICS -D_REENTRANT  -fno-implicit-templates  -fno-exceptions -fcheck-new -I${ALTIBASE_HOME}/include -I. -c -o conn.o conn.cpp
$ g++ -W -Wall -Wpointer-arith -pipe -D_POSIX_PTHREAD_SEMANTICS -D_REENTRANT  -fno-implicit-templates  -fno-exceptions -fcheck-new -I${ALTIBASE_HOME}/include -I. -c -o main.o main.cpp
$ g++ -L. -L${ALTIBASE_HOME}/lib -o conn main.o conn.o -lapre -lodbccli -lstdc++ -lsocket -ldl -lnsl -lgen -lposix4 -lkvm -lkstat -lthread -lpthread
```

##### What is wrong with the makefile shown below?

I created the makefile shown below but can't use it to compile my project.

```
#include $(ALTIBASE_HOME)/install/altibase_env.mk
COMPILE.c = /bin/cc +DA2.0W +DS2.0W  -DPDL_NDEBUG 
CC_OUTPUT_FLAG = -c
LD = /opt/aCC/bin/aCC
LFLAGS = -L.   +DA2.0W +DS2.0W -Wl,+vnocompatwarnings -L$(ALTIBASE_HOME)/lib
GOPT =
INCLUDES = -I$(ALTIBASE_HOME)/include -I.
LIBDIRS = -L$(ALTIBASE_HOME)/lib
LIBS=-lxti -lpthread -lrt –ldld
SRCS=
OBJS=$(SRCS:.cpp=.o)
BINS=altitest
SESC=altitest.c
SOBJS=$(SESS:.cpp=.o)
%.o: %.c
	$(COMPILE.c) $(INCLUDES) $(CC_OUTPUT_FLAG) $@ $<
%.c : %.sc
	APRE $<
all: $(BINS)

altitest: altitest.o altitest.c
	$(LD) $(LFLAGS) $(GOPT) $(INCLUDES) -o $@ altitest.o -lsesc -lodbccli $(LIBS)
clean:
	-rm $(BINS) $(apre) *.o core *.class

```

##### Answer

```
%.c : %.sc
	apre $< 
```

As shown above, change APRE (in upper-case) to apre (in lower-case).

##### Why can't I link my project?

The precompile operation is successful, but the following error occurs during the link operation.

```
$ cc +DA2.0W -o MKTDBD dbfunc.o main.o util.o dbif.o shm_msg.o file.o dbinit.o dbresult.o -L/user1/altibase/altibase_home/lib -L/user1/main/KTSLEE/lib  -lcom -lprice.1.1.4 -lodbccli -lapre -lxti -lpthread -lrt -ldld -ldl -lstd -lstream -
lCsup -lm -lcl -lc 
ld: Unsatisfied symbol "SESStmtCount" in file /user1/altibase/altibase_home/lib/libsesc.a[sesSqlcli.o]
1 errors.
*** Error exit code 1
Stop.
```

I am running HP-UX 11.0 on an HP L class server.

##### Answer

Change the order in which the -lodbccli and -lapre link options are specified.
