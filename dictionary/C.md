---
layout: default
parent: Dictionary
---

# C

<hr>
&nbsp;

### CB *(Control Block)*
* These are blocks of storage/memory that describe entities on the system *(users, tasks, jobs, etc...)*.

### CEC *(Central Electronics Complex)*
* This term is used to refer to the entire z/OS box.

### CF *(Coupling Facility)*
> 💡 _This can be thought of as being analogous to Kubernetes._

* This is mechanism for enabling resource sharing between disparate z/OS systems. More specifically, it is used to link separate z/OS systems into one optimized system.

### CICS *(Customer Information Control System)*
* z/OS product for managing online transactions. This product enables fast transactions with high degree of data integrity.

### Class
* In RACF, this is a mechanism for categorizing types of resources to be protected by RACF profiles.

### CLIST (Command List)
* This is like the TSO version of a shell script. This is just a list of TSO commands to run.

### COMP (Component)
> 💡 _If you compare z/OS to an application built using microservices, the microservices can be thought of as analogous to z/OS components._

* A distict piece of the operating system.

### Control Instructions 
* These are assembly language instructions that only intended for use by programs running as part of the operating system.

### Couple Data Set
* This is a data set managed by the coupling facility in a sysplex that can be shared by all or some of the z/OS systems in the sysplex.

### CP (Central Processor)
* This term term is used to refer to a general purpose mainframe processor that is not specialized or delegated for running any particular type of workload.

### CPACF *(CP Assist for Cryptographic Functions)*
* This is specicailized cryptographic hardware built into mainframe processors that allow fast and low cost encryption to be done on the processor. This mechanism is used to enable pervasive encryption.

### CPC *(Central Processing Complex)*
> 💡 _Think of the motherboard in your PC. It has a CPU socket and a few DIMM card slots. A mainframe has multiple draws that each contain multiple CPU sockets and dozens or DIMM card slots._

* The draws in the z/OS mainframe box that contain the processors and the DIMM cards.

### CSECT *(Control Section)*
* This is a low level mechanism for segmenting a link edited program.

### CSS *(Channel Subsystem)*
* The Channel Subsystem is used to manage the allocation of I/O devices such as disks and tapes to the operating system.

### CYL *(Cylinder)*
> 💡 _This can be thought of as an alternative to bytes in the same way that Farenheit is an alternative to Celcius._

* This is a metric for measuring the amount of data stored on DASD disks. This unit refers to the physical size of the cylinders on the DASD disk drives.