# Preface

### About This Manual

This manual describes how to tune Altibase for optimized performance.

#### Audience

This manual has been prepared for the following Altibase users:

-   Database managers
-   Performing managers
-   Database users
-   Application developers
-   Technical support engineers

It is recommended for those reading this manual possess the following background knowledge:

-   Basic knowledge in the use of computers, operating systems, and operating system utilities
-   Experience in using relational databases and an understanding of database concepts
-   Computer programming experience
-   Experience in database server management, operating system management, or network administration



#### Organization

This manual has been organized as follows: 

-   Chapter 1: Introduction to Performance Tuning  
    This chapter provides an overview of Altibase performance tuning.

-   Chapter 2: Altibase Server Tuning  
    This chapter describes the factors to consider when operating Altibase server.

-   Chapter 3: The Query Optimizer  
    This chapter examines the structure of the optimizer and explains what steps it takes to optimize queries
    
-   Chapter 4: The Explain Plan  
    This chapter describes the EXPLAIN PLAN, which represents the access path that the Altibase server takes to execute optimized queries
    
-   Chapter 5: The Optimizer and Statistics  
    This chapter describes why statistics are important in optimizing queries, and explains how users can collect and configure statistics.
    
-   Chapter 6: SQL Hints  
    This chapter describes SQL hints that let users to change the execution plan of an SQL statement.
    
-   Chapter 7: SQL Plan Cache  
    This chapter describes the concepts and features of Altibase's SQL Plan Cache functionality.

#### Documentation Conventions

This section describes the conventions used in this manual. Understanding these conventions will make it easier to find information in this manual and other manuals in the series.

There are two sets of conventions:

-   syntax diagram conventions 
-   sample code conventions

##### Syntax diagrams

This manual describes command syntax using diagrams composed of the following elements:

| Elements                                   | Meaning                                                      |
| ------------------------------------------ | ------------------------------------------------------------ |
| ![image1](media/GettingStarted/image1.gif) | Indicates the start of a command. If a syntactic element starts with an arrow, it is not a complete command. |
| ![](media/Admin/image006.gif)              | Indicates that the command continues to the next line. If a syntactic element ends with this symbol, it is not a complete command. |
| ![](media/Admin/image008.gif)              | Indicates that the command continues from the previous line. If a syntactic element starts with this symbol, it is not a complete command. |
| ![](media/Admin/image010.gif)              | Indicates the end of a statement.                            |
| ![](media/Admin/image012.gif)              | Indicates a manatory element.                                |
| ![](media/Admin/image014.gif)              | Indicates an optional element.                               |
| ![](media/Admin/image016.gif)              | Indicates a mandatory element comprised of options. One, and only one, option must be specified. |
| ![](media/Admin/image018.gif)              | Indicates an optional element comprised of options.          |
| ![](media/Admin/image020.gif)              | Indicates an optional element in which multiple elements may be specified. A comma must precede all but the first element. |

##### Sample Code Conventions

The code examples explain SQL statements, stored procedures, iSQL statements, and other command line syntax.
The following table describes the printing conventions used in the code examples.

| Rules            | Meaning                                                      | Example                                                      |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ ]              | Indicates an optional item                                   | VARCHAR [(*size*)] [[FIXED \|] VARIABLE]                     |
| { }              | Indicates a mandatory field for which one or more items must be selected. | { ENABLE \| DISABLE \| COMPILE }                             |
| \|               | A delimiter between optional or mandatory arguments.         | { ENABLE \| DISABLE \| COMPILE } [ ENABLE \| DISABLE \| COMPILE ] |
| . . .            | Indicates that the previous argument is repeated, or that sample code has been omitted. | SQL\> SELECT ename FROM employee;<br/> ENAME<br/>  -----------------------<br/> SWNO<br/>  HJNO<br/>  HSCHOI<br/>  .<br/> .<br/> .<br/> 20 rows selected. |
| Other Symbols    | Symbols other than those shown above are part of the actual code. | EXEC :p1 := 1; acc NUMBER(11,2)                              |
| Italics          | Statement elements in italics indicate variables and special values specified by the user. | SELECT \* FROM *table_name*; <br/>CONNECT *userID*/*password*; |
| Lower case words | Indicate program elements set by the user, such as table names, column names, file names, etc. | SELECT ename FROM employee;                                  |
| Upper case words | Keywords and all elements provided by the system appear in upper case. | DESC SYSTEM_.SYS_INDICES_;                                   |

#### Related Documents

For more detailed information, please refer to the following documents: 

-   Installation Guide

-   Getting Started Guide

-   SQL Reference

-   Stored Procedures Manual

-   iSQL User’s Manual

-   Utilities Manual

-   Error Message Reference

#### Altibase Welcomes Your Comments and Feedbacks

Please let us know what you like or dislike about our manuals. To help us with better future versions of our manuals, please tell us if there is any corrections or classifications that you would find useful.

Include the following information:

- The name and version of the manual that you are using
- Any comments about the manual
- Your name, address, and phone number

If you need immediate assistance regarding any errors, omissions, and other technical issues, please contact [Altibase's Support Portal](http://support.altibase.com/en/).

Thank you. We always welcome your feedbacks and suggestions.

