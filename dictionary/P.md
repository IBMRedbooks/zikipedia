---
layout: default
permalink: dictionary/P/
---

# P

&nbsp;

### Page
* A 4 kilobyte segment of virtual storage, meaning that this is a segment of storage as it appears to a user program.

### Page Fault
> 💡 _This is a mechanism that the operating system uses to get stuff that has been sent to swap._

* When a page is not in central storage, a page fault interrupt is raised to bring the page into central storage.

### Paging
> 💡 _Take something in memory that hasn't been used in a while and put it in disk until needed again and vice versa._

* The act of moving 4 kilobyte segments between real storage and auxiliary storage.

### Parallel Sysplex
> 💡 _This is a mechanism for linking a bunch of z/OS systems together and have them work together as one optimized logical system._

* A type of sysplex that enables multiple z/OS systems to share resources through a coupling facility.

### PARMLIB *(Parameter Library)*
> 💡 _like `/etc` on Linux._

* IPL/boot parameters for z/OS. This is like the configuration for z/OS.

### part 
* This is a term used to refer to a file or a member of a partitioned data set.

### PDS *(Partitioned Data Set)*
> 💡 _Like a z/OS version of Unix folder._

* A type of data set that is segmented into multiple logical files so that it acts like a folder that contains multiple files.

### PDSE *(Partitioned Data Set Extended)*
* A specialized version of a PDS.

### PKI *(Public Key Infrastructure)*
> 💡 _Think of RSA public/private keys._

* z/OS component for managing cryptographic certificates.

### PL/X, PL/I, PL/S, PL/AS
* These can be thought of as analogues to C and C++.

### POR *(Power On Reset)*
* A way of making configuration changes to the system without restarting it.

### Prefixing
* This is a feature of ISPF that allows the high level qualifier for your user id to be prepended to all data set references when you don't surround the data set reference in quotes.

### Proc *(Procedure)*
> 💡 _A reusable piece of JCL that acts like a C header file or an assembly macro._

* Like a JCL version of utilitarian shell script that can take arguments and do work on your behalf.

### PROCLIB *(Procedure Library)*
* A partitioned data set that contains a bunch of JCL procedures.

### Profile
* In RCAF a profile is used to define the permissons assigned to a resource.

### Problem State
* This is a program that is running as a normal user program. A problem program has no special operating system privileges and it runs in virtual storage.

### PR/SM *(Processor Resource/Systems Manager)*
* This is the firmware that is used to configure LPARs, which are segments oF the z/OS box dedicated to running an operating system.

### PSW *(Program Status Word)*
* This is a processor register used to store information about the program that is currently running.

### PSW Key
* The are 4 bits of the program status word that are used to identify the running program and protect its storage/memory pages.

### PTF *(Program Temporary Fix)*
* A PTF can be thought of as the z/OS version of a bug fix. A single PTF package can contain a number of fixes that are shipped to the customer.