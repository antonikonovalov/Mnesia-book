## 1.2  The Mnesia DataBase Management System (DBMS)

### Features

Mnesia contains the following features which combine to produce a fault-tolerant, distributed database management system written in Erlang:

Database schema can be dynamically reconfigured at runtime.
Tables can be declared to have properties such as location, replication, and persistence.
Tables can be moved or replicated to several nodes to improve fault tolerance. The rest of the system can still access the tables to read, write, and delete records.
Table locations are transparent to the programmer. Programs address table names and the system itself keeps track of table locations.
Database transactions can be distributed, and a large number of functions can be called within one transaction.
Several transactions can run concurrently, and their execution is fully synchronized by the database management system. Mnesia ensures that no two processes manipulate data simultaneously.
Transactions can be assigned the property of being executed on all nodes in the system, or on none. Transactions can also be bypassed in favor of running so called "dirty operations", which reduce overheads and run very fast.
Details of these features are described in the following sections.

### Add-on Applications

QLC and Mnesia Session can be used in conjunction with Mnesia to produce specialized functions which enhance the operational ability of Mnesia. Both Mnesia Session and QLC have their own documentation as part of the OTP documentation set. Below are the main features of Mnesia Session and QLC when used in conjunction with Mnesia:

* **QLC** has the ability to optimize the query compiler for the Mnesia Database Management System, essentially making the DBMS more efficient.
* **QLC**, can be used as a database programming language for Mnesia. It includes a notation called "list comprehensions" and can be used to make complex database queries over a set of tables.
* **Mnesia** Session is an interface for the Mnesia Database Management System
* **Mnesia** Session enables access to the Mnesia DBMS from foreign programming languages (i.e. other languages than Erlang).

### When to Use Mnesia

Use Mnesia with the following types of applications:

* Applications that need to replicate data.
* Applications that perform complicated searches on data.
* Applications that need to use atomic transactions to update several records simultaneously.
* Applications that use soft real-time characteristics.

On the other hand, Mnesia may not be appropriate with the following types of applications:

* Programs that process plain text or binary data files
* Applications that merely need a look-up dictionary which can be stored to disc can utilize the standard library module dets, which is a disc based version of the module ets.
* Applications which need disc logging facilities can utilize the module disc_log by preference.
* Not suitable for hard real time systems.

### Scope and Purpose

This manual is included in the OTP document set. It describes how to build Mnesia database applications, and how to integrate and utilize the Mnesia database management system with OTP. Programming constructs are described, and numerous programming examples are included to illustrate the use of Mnesia.

### Prerequisites

Readers of this manual are assumed to be familiar with system development principles and database management systems. Readers are also assumed to be familiar with the Erlang programming language.

### About This Book

This book contains the following chapters:

* Chapter 2, "Getting Started with Mnesia", introduces Mnesia with an example database. Examples are shown of how to start an Erlang session, specify a Mnesia database directory, initialize a database schema, start Mnesia, and create tables. Initial prototyping of record definitions is also discussed.
* Chapter 3, "Building a Mnesia Database", more formally describes the steps introduced in Chapter 2, namely the Mnesia functions which define a database schema, start Mnesia, and create the required tables.
* Chapter 4, "Transactions and other access contexts", describes the transactions properties which make Mnesia into a fault tolerant, real-time distributed database management system. This chapter also describes the concept of locking in order to ensure consistency in tables, and so called "dirty operations", or short cuts which bypass the transaction system to improve speed and reduce overheads.
* Chapter 5, "Miscellaneous Mnesia Features", describes features which enable the construction of more complex database applications. These features includes indexing, checkpoints, distribution and fault tolerance, disc-less nodes, replication manipulation, local content tables, concurrency, and object based programming in Mnesia.
* Chapter 6, "Mnesia System Information", describes the files contained in the Mnesia database directory, database configuration data, core and table dumps, as well as the important subject of backup, fall-back, and disaster recovery principles.
* Chapter 7, "Combining Mnesia with SNMP", is a short chapter which outlines Mnesia integrated with SNMP.
* Appendix A, "Mnesia Errors Messages", lists Mnesia error messages and their meanings.
* Appendix B, "The Backup Call Back Interface", is a program listing of the default implementation of this facility.
* Appendix C, "The Activity Access Call Back Interface", is a program outlining of one possible implementations of this facility.
