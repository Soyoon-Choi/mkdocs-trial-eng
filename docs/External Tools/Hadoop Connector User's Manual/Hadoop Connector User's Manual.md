Altibase Hadoop Connector User's Manual
=======================================

#### Altibase 7.3

Altibase® Tools & Utilities

<br><br><br><br><br><br>
<!-- PDF 변환을 위한 여백입니다. --> 







































<!-- PDF 변환을 위한 여백입니다. --> 

<div align="left">
    <img src="media/common/e5cfb3761673686d093a3b00c062fe7a.png">
</div>

<br><br><!-- PDF 변환을 위한 여백입니다. --> 





























<!-- PDF 변환을 위한 여백입니다. --> 

<pre>
Altibase Tools & Utilities Altibase Hadoop Connector User's Manual
Release 7.3
Copyright ⓒ 2001~2023 Altibase Corp. All Rights Reserved.<br>
This manual contains proprietary information of Altibase® Corporation; it is provided under a license agreement containing restrictions on use and disclosure and is also protected by copyright patent and other intellectual property law. Reverse engineering of the
software is prohibited.<br>
All trademarks, registered or otherwise, are the property of their respective owners.<br>
<b>Altibase Corp</b>
10F, Daerung PostTower II,
306, Digital-ro, Guro-gu, Seoul 08378, Korea
Telephone : +82-2-2082-1000 
Fax       : +82-2-2082-1099
Customer Service Portal : <a href='http://support.altibase.com/en/'>http://support.altibase.com/en/</a>
Homepage                : <a href='http://www.altibase.com'>http://www.altibase.com</a></pre>


<br>

# Table Of Contents

- [Preface](#preface)
  - [About This Manual](#about-this-manual)
- [1. Introduction to Altibase Hadoop Connector](#1-introduction-to-altibase-hadoop-connector)
  - [Background Knowledge](#background-knowledge)
  - [What is the Altibase Hadoop Connector?](#what-is-the-altibase-hadoop-connector)
- [2. Installing the Altibase Hadoop Connector](#2-installing-the-altibase-hadoop-connector)
  - [Software Requirements](#software-requirements)
  - [Installing the Altibase Hadoop Connector](#installing-the-altibase-hadoop-connector)
  - [Executing & Testing](#executing--testing)
- [3. Functions](#3-functions)
  - [Command-line Options](#command-line-options)
  - [Import](#import)
  - [Export](#export)
  - [list-databases](#list-databases)
  - [list-tables](#list-tables)
- [Appendix A: Data Types](#appendix-a-data-types)
  - [Supported Data Type](#supported-data-type)



Preface
====

### About This Manual

This manual explains how to use the Altibase Hadoop Connector with Altibase and Hadoop.

#### Audience

This manual has been prepared for the following Altibase users:

-   Database administrators
-   Performance administrators
-   Database users
-   Application developers
-   Technical Supporters

It is recommended for those reading this manual possess the following background knowledge:

-   Basic knowledge in the use of computers, operating systems, and operating system utilities
-   Experience in using relational database and an understanding of database concepts
-   Computer programming experience
-   Experience in database server management, operating system management, or network administration

#### Organization

The manual is organized as follows:

-   Chapter 1: Introduction to Altibase Hadoop Connector  
    This chapter introduces the Altibase Hadoop Connector and provides background knowledge.

-   Chapter 2: Installing the Altibase Hadoop Connector  
    This chapter explains how to install the Altibase Hadoop Connector and required software.
    
-   Chapter 3: Functions  
    This chapter describes the functionality of the Altibase Hadoop connector with examples.

-   Appendix A: Data Types  
    This appendix describes the data types supported by the Altibase Hadoop Connector.

#### Documentation Convention

This section describes the convention used in this manual. Understanding this convention will make it easier to find information in this manual and in the other manuals in the series. 

This convention described here is as follow: 

- Sample Code Convention

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

#### Related Documentations

For more detailed information, please refer to the following documents.

-   Installation Guide

-   Getting Started Guide

-   Administrator’s Manual

-   General Reference

-   Error Message Reference

#### Altibase Welcomes Your Comments and Feedbacks

Please let us know what you like or dislike about our manuals. To help us with better future versions of our manuals, please tell us if there is any corrections or classifications that you would find useful.

Include the following information:

- The name and version of the manual that you are using
- Any comments about the manual
- Your name, address, and phone number

If you need immediate assistance regarding any errors, omissions, and other technical issues, please contact [Altibase's Support Portal](http://support.altibase.com/en/).

Thank you. We always welcome your feedbacks and suggestions.
