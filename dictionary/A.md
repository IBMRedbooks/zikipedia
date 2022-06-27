---
layout: default
parent: Dictionary
---

# A

&nbsp;

### ABEND *(Abnormal End)*
* This means that the program crashed.

### Absolute Storage
> 💡 _Think about your PC. Your PC might have two DIMM cards installed on it with a total of 16 Gigabytes availabile to the entire system. This can be thought of as the Absolute Storage available to you PC._

* Full range of physical memory/storage addresses on the entire z/OS box.

### ACEE *(Accessor Environment Element)*
> 💡 _Think about your web browser. When you login to your bank, some browser cookies might get created to enable the server to do authorizations checks throughout the session. These browser cookies can be thought of as analogous to ACEEs._

* This is the security context for an address space. The ACEE contains the information necessary for doing authorization checks for the user/owner of the address space. The ACEE is created upon address space creation.

### ACL *(Access Control List)*
> 💡 _Just a list of groups and users that defines what kind of access they have to a resource._

* List of groups and users who have access to a resource protected by a RACF profile.

### Address Space
* A user's isolated slice of real storage/memory.

### AMODE *(Addressing Mode)*
> 💡 _Think 32 bit and 64 bit Intel._

* Defines memory addressing mode *(i.e. 24 bit, 31 bit, 64 bit)*. For example, a AMODE24 program can address memory/storage that can be addressed with 24 bits.

### APAR *(Authorized Program Analysis Report)*
> 💡 _Think of a GitHub issue or a Jira card._

* For all intents and purposes, an APAR can be thought of as a bug report for z/OS.

### APF *(Authorized Program Facility)*
> 💡 _Think of `sudo` or running as **root** on Linux._

* Allows programs to be identified as APF authorized so that they can execute control instructions, which means that the program is marked as allowed to run with operating system level privileges.

### APF List
> 💡 _Think of programs that need to be run with `sudo` or as the **root** user on Linux._

* List of installation specified programs that are identified as APF authorized. In other words, programs in this list are marked as allowed to run as the operating system.

### Auxiliary Storage
> 💡 _An area on disk where pieces of a loaded program can be temporarily stored until they are needed again._

* This is DASD/disk where real storage that is not seeing a lot of use gets temporarily stored until program that owns it needs it again.

### ASCB *(Address Space Control Block)*
> 💡 _The metadata attached to the user's area in memory_

* This is a block of storage that contains metadata and pointers that describe the user's address space.