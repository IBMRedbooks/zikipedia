---
layout: default
permalink: dictionary/L
---

# L

### LE *(Link Edit)*
* Just an acronym for Link Edit.

### LEPARM *(Link Edit Parameter)*
* Used to specify the link edit parameters needed by SMP/E to create load modules.

### Library
* Synonym for Partitioned Data Set. Since partitioned data sets represent a logical collection of disparate files contained in a single data set, they can be used as 'libraries' used for organizing data.

### LINKLIB
> _Think of `/usr/bin` on Linux_
* General use execuable modules available for use on the system.

### LINKLIST
> 💡 _This is the z/OS equivalent of `PATH` on Linux._
* This is a concatenation of executable load modules on the system that allows them to be called and executed easily.

### LinuxOne
* An IBMZ mainframe built specifically for running Linux.

### Listing
* This is the metadata that is produced by the copiler about the built binary object.

### LLQ *(Low Level Qualifier)*
> 💡 _Think of the the last `/` delimited segment in a Linux path (`/home/user/hello.txt`)_
* This is the most specific part of a data set name. Given the data set `RICKY.JCL.JOB1`, `JOB1` would be the low level qualifier because it identifies the exact data set being referenced.

### Load Module
> 💡 _Think of a `.exe` file on Windows._
* This is an an executable program on z/OS.

### LPA *(Link Pack Area)*
* Set of authorized programs to submit queries, make changes, an perform operational procedures against the system.

### LPALIB *(Link Pack Area Library)*
* Exacutable modules that are loaded into the Link Pack Area during IPL.

### LPAR *(Logical Partition)*
* A segment of the z/OS system that is reserved for one operating system. There can be many LPARs on one z/OS system, meaning that multiple operating systems can be run on one z/OS box. Note that this is not a virtual machine hypervisor. The operating systems are not virtualized.

### LRECL *(Logical Record Length)*
> 💡 _Think of punch cards. A punch card, which represents one record or line can only contain 80 characters._
* This is the maximum size that one line or stream of data can be in a data set. For example, a data set with a Logical Record Length of 80 allows only 80 bytes or characters to be written to one record or line.
