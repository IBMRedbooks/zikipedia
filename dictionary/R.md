---
layout: default
permalink: dictionary/R/
---

# R

&nbsp;

### RACF *(Resource Access Control Facility)*
> 💡 _RACF can be though of as the database that contains all of the securuity policies for the system. z/OS uses security policies in RACF to decide if someone requesting access to a resource should have access or not._

* This is a security manager for z/OS that maintains security policies that define who and what kind of access users have to resources on the system.

### RACROUTE
* This is a z/OS macro that is used to interface with SAF. In other words, this macro is the interface for making security related requests to the system.

### RAS *(Reliability, Availability, Serviceability)*
* These are guidelines and standards for ensuring that applications are production ready.

### Real Storage
* Real physical view of storage/memory accessible to an LPAR. There is no abstraction or logical view in this context. In other words, real storage is storage/memory as the z/OS operating system running on an LPAR sees it.

### RECFM *(Record Format)*
> _Think of punch cards. A punch card represents one line or stream of bytes that contains 80 characters/bytes. So, if you were to read a series of punch cards into a data set, you would define the record format of that data set as **fixed (F)** or **fixed blocked (FB)** since each punch card has a fixed length of 80 characters/bytes per record/line._

* This is a data set attribute that defines how data is organized in a data set.

### Record
* Data sets are composed of a series lines or records. This is different from Unix files which are just byte streams that use the `\n` character to indicate the start of a new line. For data sets, however, lines/records are an architected data structure. When a user reads or writes to a data set, the operating system treats and interprets all of the lines as individual lines/records that make up the entire data set instead of a contiguous byte streams like on Unix based systems.

### RELFILE *(Relative File)*
> 💡 _Think of the metadata files that come with a Python or Debian package._

* These are the files that describe the layout of a product so that SMP/E knows how to find and install the files contained in the product.

### Rexx *(Restructured Extended Executor)*
> 💡 _Rexx is a lot like Python._

* High level programming language developed at IBM UK.

### RMF *(Resource Measurement Facility)*
> 💡 _Think of **Task Manager** on Windows._

* Optional z/OS product used for measuring system performance.

### RMODE *(Residency Mode)*
> 💡 _Think 32 bit and 64 bit intel._

* Defines what address ranges a program can run in (i.e. 24 bit, 31 bit 64 bit). For example, a RMODE24 program cannot run in an area in memroy/storage that is not addressable with 24 bits.

### RRDS *(Relative Record Data Set)*
> 💡 _This is functionally similar to an array in high level programming languages where elements are accessed by their index._

* A VSAM data set where data is accessed by it's record number where each record number is relative to the first record.