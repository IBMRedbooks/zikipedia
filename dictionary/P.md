---
layout: default
parent: Dictionary
---

# P

<hr>
&nbsp;

### Page
* A 4 kilobyte segment of virtual storage, meaning that this is a segment of memory/storage as it appears to a user program.

### Page Fault
* When a page is not in central storage, a page fault interrupt is raised to bring the page into central storage. In other words, this is a mechanism that the operating system uses to get stuff that has been swaped to auxiliary storage *(disk)* back into memory/storage.

### Paging

{: .note }
> _Take something in memory that hasn't been used in a while and put it in disk until needed again and vice versa._

* The act of moving 4 kilobyte segments between real storage and auxiliary storage.

### Parallel Sysplex

{: .note }
> _This is a mechanism for linking a bunch of z/OS systems together and having them work together as one optimized logical system._

* A type of sysplex that enables multiple z/OS systems to share resources through a coupling facility.

### PARMLIB *(Parameter Library)*

{: .note }
> _like `/etc` on Linux._

* IPL/boot parameters for z/OS.

### part 
* This is a term used to refer to a file or a member of a partitioned data set.

### Pervasive Encryption
* This is a z/OS capability that allows all data in flight and at rest to be encrypted transparently. The CPACF, which is a cryptographic excelerator on s390x CPUs encrypts and decrypts data at the processor, meaning that data is only decrypted when in use by the processor or when loaded into storage/memory.

### PDS *(Partitioned Data Set)*

{: .note }
> _Like the z/OS version of Unix folder._

* A type of data set that is segmented into multiple logical files so that it acts like a folder that contains multiple files.

### PDSE *(Partitioned Data Set Extended)*
* A enhanced version of a PDS. In most cases, a PDSE can and should be used instead of a PDS.

### PKI *(Public Key Infrastructure)*

{: .note }
> _Think of RSA public/private keys._

* z/OS component for managing cryptographic certificates.

### PL/X, PL/I, PL/S, PL/AS
* These can be thought of as analogues to C and C++.

### POR *(Power On Reset)*
* A way of making configuration changes to the system without restarting it.

### PRE
* In SMP/E, PRE is specified as part of the ++VER SMPMCS statement to define an FMID/product that the FMID/product being installed depends on and requires.

### Prefixing
* This is a feature of ISPF that allows the high level qualifier for your user id to be prepended to all data set references when you don't surround the data set name in quotes.

### Proc *(Procedure)*

{: .note }
> _A reusable piece of JCL that acts like a C header file or an assembly macro._

* Like a JCL version of utilitarian shell script that can take arguments and provide abstraction.

### PROCLIB *(Procedure Library)*
* A partitioned data set that contains a bunch of JCL procedures *(Reusable JCL that provides abstraction)*.

### Profile
* In RCAF, a profile is used to define the permissons assigned to a resource.

### Problem State
* This is a program that is running as a normal user program. A problem program has no special operating system privileges and it runs in virtual storage.

### PR/SM *(Processor Resource/Systems Manager)*
* This is the firmware that is used to configure LPARs, which are segments of the z/OS box dedicated to running operating systems.

### PSW *(Program Status Word)*
* This is a processor register used to store information about the program that is currently running.

### PSW Key
* The 4 bits of the program status word that are used to identify the running program and protect its storage/memory pages.

### PTF *(Program Temporary Fix)*
* A PTF can be thought of as the z/OS version of a bug fix. A single PTF package can contain a number of fixes that are shipped to the customer.