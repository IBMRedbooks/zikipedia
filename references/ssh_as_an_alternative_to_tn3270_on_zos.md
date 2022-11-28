---
layout: default
parent: References
---

# Using SSH as an Alternative to TN3270 on z/OS

<hr>
&nbsp;

Many people think that the only way to do anything on **z/OS** is through **TN3270** and **ISPF panels**. That has not been true for a very long time. **z/OS UNIX System Services** is a fantastic alternative to ISPF panels. The best part is that if you are familiar with standard command processing languages like **shell**, **Bash**, and **Windows Powershell**, using z/OS UNIX System Services will be straight forward. This reference provides a myriad of z/OS UNIX System Services **alternatives** to traditional TN3270 interfaces on z/OS.

&nbsp;

{: .warning }
> _**ZOAU** is a **prerequisite** for many of these examples. A dependency on ZOAU will be noted for each example where ZOAU is required. This reference also assumes that you are using ZOAU 1.2.x or greater. See **[Install ZOAU 1.2.x](https://www.ibm.com/docs/en/zoau/1.2.x?topic=installing-configuring-zoau)** for inscructions on how to install ZOAU._

&nbsp;

{: .warning }
> _Having **SSH login** enabled is also a **prerequisite**._

&nbsp;

{: .note }
> _Installing **BASH** and setting it as your **default shell** is not required, but **highly recommended**._

## Working With Data Sets

In the world of z/OS, data sets are **functionally the same** as UNIX files. So, for all intents and purposes data sets are just files. 

&nbsp;

However, it is worth noting that data sets are fundamentally different from UNIX files architecturally speaking. So, the way data sets are managed, located, manipulated, and created is highly esoteric from the perspective of someone with a distributed systems background.

&nbsp;

Data sets are relevant because they compose the entire **MVS** _(Multiple Virtual Storage)_ side of z/OS. For context, the combination MVS and Unix System Services is what makes z/OS what it is. So, if you are unable to manage data sets, you are essentially limited to half of the system's capabilities. 

&nbsp;

ISPF panels are inherently oriented towards working with data sets, but given the objective of this guide is to make doing most tasks through z/OS Unix System Services possible, this segment will focus on the interfaces available for working with data sets through z/OS UNIX System Services.

&nbsp;


{: .warning }
>  _The scope of this section is limited to **non-VSAM data sets** only._

### List Data Sets (Requires ZOAU)

The following example uses the `dls` command to display a list of **data sets** that match a **pattern**. In the following example all data sets that match the pattern `USER.LLO.*` are displayed. The `dls` command is functionally similar to the standard standard UNIX `ls` command.

&nbsp;

```console
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
```

### List Data Sets With Details (Requires ZOAU)

If the `-s` flag is used with the `dls` command, the following information is also provided with each data set listed.

&nbsp;

* **Data Set Organization**
  * PS _(Physical Sequential)_
    * This is just a fancy way of saying it is just a flat file.
  * PO _(Partitioned Organization)_
    * This is just a fancy way of saying that it functions like a folder that can contain a number of files.
* **Logical Record Format**
  * This describes the structure of that data. For example, *FB* _(Fixed Blocked)_ means that each _“line“_ or record is a fixed length and that the data set uses blocking.
* **Logical Record Length**
  * The is the length of each _“line”_ or record in the data set.
* **Volume**
  * The name or ID of the **DASD** _(Direct Access Storage Device)_ device that the data set resides on.
* **Total Bytes Used**
* **Total Bytes Available**

&nbsp;

```console
$ dls -s USER.LLQ.*
USER.LLQ.DATASET1                            PS  FB      80 VOL001           11        56664
USER.LLQ.DATASET2                            PO  FBA    133 VOL002         1021        56664
USER.LLQ.DATASET3                            PO  U        0 VOL003         2192        56664
```

If the `-b` flag is used with the `dls` command, **block size** is included as part of the additional details instead of total bytes used and total bytes available. The block size is used to **optimize I/O** and is generally a multiple of the record length. Block size can also be significant for things other than I/O.

&nbsp;

{: .note }
> _All of the data set listed in this example have a **block size** of `32720`._

&nbsp;

```console
$ dls -b USER.LLQ.*
USER.LLQ.DATASET1                            PS  FB      80 32720 VOL001
USER.LLQ.DATASET2                            PO  FBA    133 32718 VOL002
USER.LLQ.DATASET3                            PO  U        0 32720 VOL003
```

### List Data Sets and Include Migrated Data Sets (Requires ZOAU)

Migrated data sets are those that have been migrated from **DASD** _(Direct Access Storage Device)_ to **low cost tape**. These migrated data sets are not displayed by default when using the `dls` command, but if you use the `-m` flag with the `dls` command, migrated data sets will also be listed.

&nbsp;

```console
$ dls -m USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
USER.LLQ.MIGRATE1
USER.LLQ.MIGRATE2
```

### List All Data Sets on a DASD Volume (Requires ZOAU)

The following example uses the `vtocls` command to list a **DASD** _(Direct Access Storage Device)_ volume’s **VTOC** _(Volume Table of Contents)_ which is a listing of all the data sets the reside on the volume.

&nbsp;

```console
$ vtocls VOL001
USER.LLQ.DATASET1                             PS  FB   80  VOL001
USER2.LLQ2.DATASET5                           PO FBA   133 VOL001
USER2.LLQ2.DATASET7                           PO   U   0   VOL001
USER3.LLQ3.DATASET1                           PS  FB   80  VOL001
```

### List Partitioned Data Set Members (Requires ZOAU)

The **members** or entries contained in a **partitioned data set** can me listed using the `mls` command.

&nbsp;

```console
$ mls USER.LLQ.DATASET2
MEMBER1
MEMBER2
MEMBER3
```

### List Partitioned Data Set Members And Include Aliases

The **aliases** to **data set members** in a **partitioned data** set can be displayed using `tsocmd` to run the **TSO** _(Time Sharing Option)_ `LISTDS` command with the **last positional argument** being `MEMBERS`, which tells `LISTDS` to display a **member listing**.

&nbsp;

```console
$ tsocmd "LISTDS USER.LLQ.DATASET2' MEMBERS"
LISTDS 'USER.LLQ.DATASET2' MEMBERS
USER.LLQ.DATASET2
--RECFM-LRECL-BLKSIZE-DSORG
  FBA   133   32718   PO
--VOLUMES--
  VOL002
--MEMBERS--
  MEMBER1  ALIAS(ALIAS1)
  MEMBER2
  MEMBER3  ALIAS(ALIAS2,ALIAS3)
```

### Rename a Data Set (Requires ZOAU)

A **data set** can be **renamed** using the `dmv` command. This command functions exactly like the standard UNIX `mv` command.

&nbsp;

```console
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
$ dmv USER.LLQ.DATASET2 USER.LLQ.RENAME
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET3
USER.LLQ.RENAME
```

### Rename a Partitioned Data Set Member (Requires ZOAU)

A **partitioned data set member** can be renamed using the `mmv` command. This command functions almost exactly like the standard UNIX `mv` command. The only difference is that the **first positional argument** must be the name of the **partitioned data set**.

&nbsp;

```console
$ mls USER.LLQ.DATASET2
MEMBER1
MEMBER2
MEMBER3
$ mmv USER.LLQ.DATASET2 MEMBER2 RENAME
$ mls USER.LLQ.DATASET2
MEMBER1
MEMBER3
RENAME
```

### Delete a Data Set (Requires ZOAU)
A data set can be **deleted** using the `drm` command. This command functions almost exactly like the standard UNIX `rm` command.

&nbsp;

```console
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
$ drm USER.LLQ.DATASET3
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
```

Just like the standard UNIX `rm` command, the `drm` command can all delete **several** data sets based on a **pattern**. Therefore, this command should similarly be used with caution.

&nbsp;

```console
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
$ drm USER.LLQ.*
$ dls USER.LLQ.*
BGYSC1103E No datasets match pattern: USER.LLQ.*.
```

### Delete a Partitioned Data Set Member (Requires ZOAU)
A **partitioned data set member** can be **deleted** using the `mrm` command. This command functions almost exactly like the standard UNIX `rm` command. 

&nbsp;

```console
$ mls USER.LLQ.DATASET2
MEMBER1
MEMBER2
MEMBER3
$ mrm 'USER.LLQ.DATASET2(MEMBER3)'
$ mls USER.LLQ.DATASET2
MEMBER1
MEMBER2
```

Just like the standard UNIX `rm` command, `mrm` can delete **several** partitioned data set members based on a **pattern**. Therefore, this command should similarly be used with caution.

&nbsp;

```console
$ mls USER.LLQ.DATASET2
MEMBER1
MEMBER2
MEMBER3
$ mrm 'USER.LLQ.DATASET2(MEMBER*)'
$ mls USER.LLQ.DATASET2
BGYSC1505E No members match pattern * in dataset USER.LLQ.DATASET2.
```

### Display The Contents of a Sequential Data Set

The standard UNIX `cat` command can be used to **display** the **contents** of a **sequential data** set as follows.

&nbsp;

```console
$ cat "//'USER.LLQ.DATASET1'"
Hello World!
```

{: .note }
> _The standard UNIX `head` and `tail` commands can be used in the same exact way._

&nbsp;

```console
$ head "//'USER.LLQ.DATASET1'"
Hello World!
$ tail "//'USER.LLQ.DATASET1'"
Hello World!
```

{: .warning }
> _The standard UNIX `more` command must be used with **DSFS** (Data Set File System) notation instead. The `/user` **folder name** corresponds to the **High Level Qualifier** `USER` and the `/llq.dataset1` **file name** coresponds to the **low level qualfiers** `LLQ.DATASET1`._

&nbsp;

```console
$ more /dsfs/txt/user/llq.dataset1
```

### Display The Contents of a Partitioned Data Set Member

The standard UNIX `cat` command can be used to **display** the **contents** of a **partitioned data set member** as follows.

&nbsp;

```console
$ cat "//'USER.LLQ.DATASET2(MEMBER1)'"
Hello World!
```

{: .note }
> _The standard UNIX `head` and `tail` commands can be used in the same exact way._

&nbsp;

```console
$ head "//'USER.LLQ.DATASET2(MEMBER1)'"
Hello World!
$ tail "//'USER.LLQ.DATASET2(MEMBER1)'"
Hello World!
```

{: .warning }
> _The standard UNIX `more` command must be used with **DSFS** (Data Set File System) notation instead. The `/user` **folder name** corresponds to the **High Level Qualifier** `USER`, the `/llq.dataset2` **folder name** coresponds to the **low level qualfiers** `LLQ.DATASET2`, and the **file name** `/member1` corresponds to the **member name** `MEMBER1`._

&nbsp;

```console
$ more /dsfs/txt/user/llq.dataset2/member1
```

### Compare Data Sets (Requires ZOAU)

The `ddiff` command can be used to show the **difference** between two data sets. Note that `ddiff` only works when comparing **plain text data**.

#### Compare a Sequential Data Set to a Sequential Data Set

```console
$ ddiff USER.LLQ.SEQ USER.LLQ.LARGE
NEW: USER.LLQ.LARGE                                          OLD: USER.LLQ.SEQ

                     LISTING OUTPUT SECTION (LINE COMPARE)

ID       SOURCE LINES                                                                                                                                                                TYPE  LEN N-LN# O-LN#

I - New Message.                                                                                                                                                                     RPL=    1 00001 00001
D - Old Message.
```

#### Compare a Data Set Member to a Partitioned Data Set Member

```console
$ ddiff 'USER.LLQ.PDS(MEMBER1)' 'USER.LLQ.PDSE(MEMBER1)'
NEW: USER.LLQ.PDSE(MEMBER1)                                  OLD: USER.LLQ.PDS(MEMBER1)

                     LISTING OUTPUT SECTION (LINE COMPARE)

ID       SOURCE LINES                                                                                                                                                                TYPE  LEN N-LN# O-LN#

I - New Message.                                                                                                                                                                     RPL=    1 00001 00001
D - Old Message.
```

#### Compare a Sequential Data Set to a Partitioned Data Set Member

```console
$ ddiff USER.LLQ.SEQ 'USER.LLQ.PDS(MEMBER1)'
NEW: USER.LLQ.PDS(MEMBER1)                                   OLD: USER.LLQ.SEQ

                     LISTING OUTPUT SECTION (LINE COMPARE)

ID       SOURCE LINES                                                                                                                                                                TYPE  LEN N-LN# O-LN#

I - New Message.                                                                                                                                                                     RPL=    1 00001 00001
D - Old Message.
```

#### Compare a Partitioned Data Set Member to a Sequential Data Set


```console
$ ddiff 'USER.LLQ.PDS(MEMBER1)' USER.LLQ.SEQ
NEW: USER.LLQ.SEQ                                            OLD: USER.LLQ.PDS(MEMBER1)

                     LISTING OUTPUT SECTION (LINE COMPARE)

ID       SOURCE LINES                                                                                                                                                                TYPE  LEN N-LN# O-LN#

I - New Message.                                                                                                                                                                     RPL=    1 00001 00001
D - Old Message.
```

### Allocate a Data Set (Requires ZOAU)

The `dtouch` command is the functional equivalent of the standard UNIX `touch` command for **data sets**. The following example allocates a **FB** _(Fixed Blocked)_ **PDSE data set** with a **logical record length** of **80** and a **1MB primary extent**.

&nbsp;

{: .note }
> _Explaining how all of the data set attributes work is beyond the scope of this reference. For more details on data sets, see **[What is a data set?](https://www.ibm.com/docs/en/zos-basic-skills?topic=more-what-is-data-set)**._

&nbsp;

```console
$ dtouch -t PDSE -r FB -l 80 -s 1MB USER.LLQ.NEW
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
USER.LLQ.NEW
```

{: .note }
>  _You may also optionally use the `tsocmd` command to run the **TSO** (Time Sharing Option) `ALLOCATE` command to allocate a data set instead. The main difference with this example is that it uses **tracks** as the unit for defining the **primary extent** instead of **megabytes**. Also note that `dsntype(library,2)` is used to indicate that the data set being allocated should be a **PDSE**. See **[Quick reference: Data set structure](https://www.ibm.com/docs/en/zos-basic-skills?topic=set-quick-reference-data-structure)** for more information on **tracks** and **cylinders** as a unit for measuring and allocating disk space._

&nbsp;

```console
$ tsocmd "allocate dsname('USER.LLQ.NEW'),dsorg(po),dsntype(library,2),recfm(f,b),lrecl(80),unit(3390),space(100) TRACK"
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
USER.LLQ.NEW
```

### Create a New Partitioned Data Set Member (Requires ZOAU)

The following example uses the `decho` command to create an **empty data set member**.

&nbsp;

```console
$ mls USER.LLQ.DATASET2
MEMBER1
MEMBER2
MEMBER3
$ decho '' 'USER.LLQ.DATASET2(NEW)'
$ mls USER.LLQ.DATASET2
MEMBER1
MEMBER2
MEMBER3
NEW
```

### Echo Text to a Sequential Data Set (Requires ZOAU)

The following example uses the `decho` command to echo text to a **sequential data set**.

&nbsp;

```console
$ cat "//'USER.LLQ.SEQ'"
Old message.
$ decho 'New message.' USER.LLQ.SEQ
$ cat "//'USER.LLQ.SEQ'"
New message.
```

### Echo Text to a Partitioned Data Set Member (Requires ZOAU)

The following example uses the `decho` command to echo text to a **partitioned data set member**.

&nbsp;

```console
$ cat "//'USER.LLQ.PDS(MEMBER1)'"
Old message.
$ decho 'New message.' 'USER.LLQ.PDS(MEMBER1)'
$ cat "//'USER.LLQ.PDS(MEMBER1)'"
New message.
```

### Create a New Partitioned Data Set Member Alias

The following example uses the `tsocmd` command to run the **TSO** _(Time Sharing Option)_ `RENAME` command with **positional arguments** telling it to create an **alias** of **partitioned data set member** `USER.LLQ.DATASET2(MEMBER1)` called `ALIAS1`.

&nbsp;

```console
$ tsocmd "RENAME 'USER.LLQ.DATASET2(MEMBER1)' (ALIAS1) ALIAS"
RENAME 'USER.LLQ.DATASET2(MEMBER1)' (ALIAS1) ALIAS
$ tsocmd "LISTDS 'USER.LLQ.DATASET2' MEMBERS"
LISTDS 'USER.LLQ.DATASET2' MEMBERS
USER.LLQ.DATASET2
--RECFM-LRECL-BLKSIZE-DSORG
  FBA   133   32718   PO
--VOLUMES--
  VOL002
--MEMBERS--
  MEMBER1  ALIAS(ALIAS1)
  MEMBER2
  MEMBER3
```

### Use The vi Command Line Editor to Edit Data Sets

**DSFS** _(Data Set File System)_ makes data sets look like **UNIX files** such that UNIX programs and utilities can interact with data sets in the same way they would with any UNIX file. This means that data sets can be edited with the `vi` command line editor as an **alternative** to the **ISPF editor**.

#### Edit a Sequential Data Set

In this example, the **sequential data** set `USER.LLQ.SEQ` is being opened with the `vi` command line editor. The **highest level qualifier** `USER` corresponds to the the **subfolder** `/user` and the **low level qualifiers** `LLQ.SEQ` correspond to the **file name** `/llq.seq`.

&nbsp;

```console
$ vi /dsfs/txt/user/llq.seq
```

If a port of `vim` is installed on the system, you may also edit a **sequential data set** using `vim` in the exact same way.

&nbsp;

```console
$ vim /dsfs/txt/user/llq.seq
```

{: .note }
> _Other **UNIX-based command line editors** should be able to be used the same way using **DSFS** (Data Set File System) notation._

#### Edit a Partitioned Data Set Member

In this example, the **partitioned data set member** `USER.LLQ.PDS(MEMBER1)` is being opened with the `vi` command line editor. The **highest level qualifier** `USER` corresponds to the the **subfolder** `/user`, the **low level qualifiers** `LLQ.PDS` correspond to the **subfolder** `/llq.pds`, and the **member name** `MEMBER1` corresponds to the **file name** `/member1`.

&nbsp;

```console
$ vi /dsfs/txt/user/llq.pds/member1
```

If a port of `vim` is installed on the system, you may also edit a **sequential data set** using `vim` in the exact same way.

&nbsp;

```console
$ vim /dsfs/txt/user/llq.pds/member1
```

{: .note }
> _Other **UNIX-based command line editors** should be able to be used the same way using **DSFS** (Data Set File System) notation._

### Move Data Between z/OS Unix System Services and MVS

The following examples show how the **z/OS UNIX System Services** `cp` command can be used copy data from **data sets** to the **z/OS Unix System Services filesystem** and vice versa.

&nbsp;

**Copy Modes:**
* **Plain Text:**
  * Use the `-T` flag to copy **plain text** data.
* **Binary:**
  * Use the `-B` flag to copy **binary** data.
* **Executable:**
  * Use the `-AIX` flag to copy **Link Edited Program/Load Module** data.

{: .note }
> _The `-AIX` flag also **preserves aliases**._

* **Auto:**
  * Use the `-A` flag to allow **binary** and **plain text** data to be **automatically detected** to enable the decision of whether to use the **Plain Text copy mode** or the **Binary copy mode** to be determined automatically.

{: .warning }
> _The `-A` flag should **NOT** be used with **Link Edited Program/Load Module data**. Also be aware that **plain text data** should contain **NO null bytes** or any bytes that do **NOT** map to the **codec** being used by **MVS** for data sets._

&nbsp;

The following examples use the `-T` to copy **plain text** data. The same actions can be performed using the other **Copy Modes** described above.

#### Copy The Contents of a Sequential Data Set to a UNIX File

```console
$ cp -T "//'USER.LLQ.DATASET1'" hello.txt
```

#### Copy The Contents of a UNIX File to a Sequential Data Set

```console
$ cp -T hello.txt "//'USER.LLQ.DATASET1'"
```

#### Copy The Contents of a Partitioned Data Set Member to a UNIX File

```console
$ cp -T "//'USER.LLQ.DATASET2(MEMBER1)'" hello.txt
```

#### Copy The Contents of a UNIX File to a Partitioned Data Set Member

```console
$ cp -T hello.txt "//'USER.LLQ.DATASET2(MEMBER1)'"
```

#### Copy All Members From a Partitioned Data Set to a UNIX Folder

```console
$ cp -T "//'USER.LLQ.DATASET2'" folder/
```

#### Copy All Files in a UNIX Folder to a Partitioned Data Set

```console
$ cp -T folder/* "//'USER.LLQ.DATASET2'"
```

### Migrate an SMS-Managed Data Set

The following example using the `tsocmd` command to run the **TSO** _(Time Sharing Option)_ `HMIGRATE` command which tells **DFSMShsm** to **migrate** a data set from **DASD** to **tape**.

&nbsp;

{: .warning }
> _The data set being migrated **MUST** be **SMS-managed** and **DFSMShsm** **MUST** be **active** in order for the `HMIGRATE` command to work._

&nbsp;

{: .warning }
> _`HMIGRATE` is **asynchronous**. You will **NOT** get any **return codes** or **feedback** indicating whether or not the request was successful. You need to verify that `HMIGRATE` migrated the data set you requested yourself afterwards._

&nbsp;

```console
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
$ tsocmd "HMIGRATE 'USER.LLQ.DATASET1'"
HMIGRATE 'USER.LLQ.DATASET1'
MIGRATE REQUEST XXXXXXXX SENT TO DFSMSHSM
$ dls USER.LLQ.*
USER.LLQ.DATASET2
USER.LLQ.DATASET3
```

### Recall an SMS-Managed Data Set

The following example uses the `tsocmd` command to run the **TSO** _(Time Sharing Option)_ `HRECALL` command which tells **DFSMShsm** to **recall** a **migrated** data set back to **DASD**. 

&nbsp;

{: .warning }
> _The data set being recalled **MUST** be **SMS-managed** and **DFSMShsm** **MUST** be **active** in order for the `HRECALL` command to work._

&nbsp;

{: .warning }
> _`HRECALL` is **asynchronous**. You will **NOT** get any **return codes** or **feedback** indicating whether or not the request was successful. You need to verify that `HRECALL` recalled the data set you requested yourself afterwards._

&nbsp;

```console
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
$ tsocmd "HRECALL 'USER.LLQ.MIGRATED'"
HRECALL 'USER.LLQ.MIGRATED'
 RECALL REQUEST XXXXXXXX SENT TO DFSMSHSM
$ dls USER.LLQ.*
USER.LLQ.DATASET1
USER.LLQ.DATASET2
USER.LLQ.DATASET3
USER.LLQ.MIGRATED
```

### Convert a PDS Data Set to a PDSE (Requires ZOAU)

The following example uses the `mvscmdauth` command and the **DFSMSdss** utility `ADRDSSU` to convert the **PDS** data set `USER.LLQ.PDS` to a **PDSE** data set named `USER.LLQ.PDSE`.

&nbsp;

The `DS(INC(USER.LLQ.PDS))` statement tells `ADRDSSU` that `USER.LLQ.PDS` is the **source data set**. The `CONVERT (PDSE(**))` statement tells `ADRDSSU` that the **target data set** should be converted to a **PDSE** data set. The `RENUC` statement tells `ADRDSSU` to **copy** the **source data set** `USER.LLQ.PDS` to the **target data set** `USER.LLQ.PDSE`.

&nbsp;

```console
$ mvscmdauth --pgm=ADRDSSU \
             --sysprint=stdout \
             --sysin=stdin <<zz
 COPY  -
   DS(INC(USER.LLQ.PDS)) -
       CONVERT (PDSE(**)) -
       RENUNC ( -
           USER.LLQ.PDS, -
           USER.LLQ.PDSE)
zz
```

### Convert a PDSE Data Set to a PDS (Requires ZOAU)

The following example uses the `mvscmdauth` command and the **DFSMSdss** utility `ADRDSSU` to convert the **PDSE** data set `USER.LLQ.PDSE` to a **PDS** data set named `USER.LLQ.PDS`.

&nbsp;

The `DS(INC(USER.LLQ.PDSE))` statement tells `ADRDSSU` that `USER.LLQ.PDSE` is the **source data set**. The `CONVERT (PDS(**))` statement tells `ADRDSSU` that the **target data set** should be converted to a **PDS** data set. The `RENUC` statement tells `ADRDSSU` to **copy** the **source data set** `USER.LLQ.PDSE` to the **target data set** `USER.LLQ.PDS`.

&nbsp;

```console
$ mvscmdauth --pgm=ADRDSSU \
             --sysprint=stdout \
             --sysin=stdin <<zz
 COPY  -
   DS(INC(USER.LLQ.PDSE))  -
       CONVERT (PDS(**)) -
       RENUNC ( -
           USER.LLQ.PDSE, -
           USER.LLQ.PDS)
zz
```

### One to One Copy (Requires ZOAU)

The following examples use the `mvscmd` command and the **DFSMSdfp** utility `IEBGENER` to copy **sequential data** from one data set to another. The `sysut1` **data definition** represents the **source** and the `sysut2` **data definition** represents the **target**.

&nbsp;

{: .warning }
> _The **record length** and **record format** of both `sysut1` and `sysut2` should be the **same**. For example, you **CANNOT** copy sequential data from a **fixed blocked 133 source** to a **fixed block 80 target** and vice versa._

&nbsp;

{: .note }
> _`dummy` is **analogous** to `/dev/null`._

#### Copy The Contents of a Sequential Data Set to Another Sequential Data Set

```console
$ mvscmd --pgm=IEBGENER \
         --sysin=dummy \
         --sysprint=stdout \
         --sysut1=USER.FB80.LARGE \
         --sysut2=USER.FB80.SEQ
```

#### Copy The Contents of a Partitioned Data Set Member to Another Partitioned Data Set Member

```console
$ mvscmd --pgm=IEBGENER \
         --sysin=dummy \
         --sysprint=stdout \
         --sysut1='USER.FB80.PDSE(MEMBER1)' \
         --sysut2='USER.FB80.PDS(MEMBER1)'
```

#### Copy The Contents of a Sequential Data Set to a Partitioned Data Set Member

```console
$ mvscmd --pgm=IEBGENER \
         --sysin=dummy \
         --sysprint=stdout \
         --sysut1=USER.FB80.SEQ \
         --sysut2='USER.FB80.PDS(MEMBER1)'
```

#### Copy The Contents of a Partitioned Data Set Member to a Sequential Data Set

```console
$ mvscmd --pgm=IEBGENER \
         --sysin=dummy \
         --sysprint=stdout \
         --sysut1='USER.FB80.PDS(MEMBER1)' \
         --sysut2=USER.FB80.SEQ
```

### Clone A Data Set (Requires ZOAU)

The following example uses the `mvscmdauth` command and the **DFSMSdss** utility `ADRDSSU` to **clone** data set `USER.LLQ.DATASET2` to a new data set named `USER.LLQ.CLONE`.

&nbsp;

The `DS(INC(USER.LLQ.PDS))` statement tells `ADRDSSU` that `USER.LLQ.DATASET2` is the **source data set**. The `RENUC` statement tells `ADRDSSU` to **copy** the **source data set** `USER.LLQ.DATASET2` to the **target data set** `USER.LLQ.CLONE`.

&nbsp;

```console
$ mvscmdauth --pgm=ADRDSSU \
             --sysprint=stdout \
             --sysin=stdin <<zz
 COPY  -
   DS(INC(USER.LLQ.DATASET2))  -
       RENUNC ( -
           USER.LLQ.DATASET2, -
           USER.LLQ.CLONE)
zz
```

### Clone Several Data Sets Based on A Pattern (Requires ZOAU)

The following example uses the `mvscmdauth` command and the **DFSMSdss** utility `ADRDSSU` to **clone** all data sets that match the pattern `USER.LLQ.**` to new data sets with the naming convention `USER.CLONE.**`.

&nbsp;

The `DS(INC(USER.LLQ.**))` statement tells `ADRDSSU` that all data sets that match the **pattern** `USER.LLQ.**` are the **source data sets**. The `RENUC` statement tells `ADRDSSU` to **copy** the **source data sets** from `USER.LLQ.**` to **target data sets** with the naming convention `USER.CLONE.**`.

&nbsp;

```console
$ mvscmdauth --pgm=ADRDSSU \
             --sysprint=stdout \
             --sysin=stdin <<zz
 COPY  -
   DS(INC(USER.LLQ.**))  -
       RENUNC ( -
           USER.LLQ.**, -
           USER.CLONE.**)
zz
```

### Copy and Merge Partitioned Data Sets (Requires ZOAU)

The following example uses the `mvscmd` command and **DFSMSdfp** utility `IEBCOPY` to **copy and merge** all of the **partitioned data set members** from `USER.LLQ.FROM` into **partitioned data set** `USER.LLQ.TO`. 

&nbsp;

The `COPYGROUP INDD=IN,OUTDD=OUT` statement tells `IEBCOPY` that the **source** data set is the `in` **data definition** _(`USER.LLQ.FROM`)_ and the **target** data set is the `out` **data definition** _(`USER.LLQ.TO`)_.

&nbsp;

```console
$ mvscmd --pgm=IEBCOPY \
         --sysprint=stdout \
         --in=USER.LLQ.FROM \
         --out=USER.LLQ.TO \
         --sysin=stdin <<zz
        COPYGROUP INDD=IN,OUTDD=OUT
zz
```

### Copy and Merge Partitioned Data Sets and Include Aliases (Requires ZOAU)

The following example uses the `mvscmd` command and the **DFSMSdfp** utility `IEBCOPY` to **copy and merge** all of the **partitioned data set members** and **aliases** from `USER.LLQ.FROM` into **partitioned data set** `USER.LLQ.TO`. 

&nbsp;

The `COPYGROUP INDD=((IN,R)),OUTDD=OUT` statement tells `IEBCOPY` that the **source** data set is the `in` **data definition** _(`USER.LLQ.FROM`)_ and the **target** data set in the `out` **data definiton** _(`USER.LLQ.TO`)_. 

&nbsp;

The thing that enables **aliases** to be **preserved** is is the `INDD=((IN,R))` portion of the `COPYGROUP` statement which tells `IEBCOPY` to also copy and merge the **aliases** in the **source** data set _(`USER.LLQ.TO`)_ into the **target** data set _(`USER.LLQ.TO`)_.

&nbsp;

```console
$ mvscmd --pgm=IEBCOPY \
         --sysprint=stdout \
         --in=USER.LLQ.FROM \
         --out=USER.LLQ.TO \
         --sysin=stdin <<zz
        COPYGROUP INDD=((IN,R)),OUTDD=OUT
zz
```

### Copy and Merge Partitioned Data Sets and Filter Members (Requires ZOAU)

The following example uses the `mvscmd` and and the **DFSMSdfp** utility `IEBCOPY` to **copy and merge** only a **subset** of the **partitioned data set members** from `USER.LLQ.FROM` into **partitioned data set** `USER.LLQ.TO`. 

&nbsp;

The `COPYGROUP INDD=IN,OUTDD=OUT` statement tells `IEBCOPY` that the **source** data set is the `in` **data definition** _(`USER.LLQ.FROM`)_ and the **target** data set is the `out` **data definition** _(`USER.LLQ.TO`)_.

&nbsp;

The `SELECT MEMBER=` statement is what tells `IBECOPY` to **filter** the the partitioned data set members being copied from the **source**. This example uses a **comma separated list** of **member names** to specify a **subset** of partitioned data set members that should be copied and merged from the **source** into the **target**.

&nbsp;

```console
$ mvscmd --pgm=IEBCOPY \
         --sysprint=stdout \
         --in=USER.LLQ.FROM \
         --out=USER.LLQ.TO \
         --sysin=stdin <<zz
        COPYGROUP INDD=IN,OUTDD=OUT
        SELECT MEMBER=(MEMBER1,MEMBER2,MEMBER3)
zz
```

{: .note }
> _**Wildcarding** is also supported when using the `SELECT MEMBER=` statement with `IEBCOPY`. The following example will **copy and merge** all **partitioned data set members** that match the naming convention `MEMBER*` and `A*` from `USER.LLQ.FROM` into `USER.LLQ.TO`._

&nbsp;

```console
$ mvscmd --pgm=IEBCOPY \
         --sysprint=stdout \
         --in=USER.LLQ.FROM \
         --out=USER.LLQ.TO \
         --sysin=stdin <<zz
        COPYGROUP INDD=IN,OUTDD=OUT
        SELECT MEMBER=(MEMBER*,A*)
zz
```

{: .note }
> _If you need to specify a large amount of **partitioned data set member patterns**, you can use **multiple** `SELECT MEMBER=` statements._

&nbsp;

```console
$ mvscmd --pgm=IEBCOPY \
         --sysprint=stdout \
         --in=USER.LLQ.FROM \
         --out=USER.LLQ.TO \
         --sysin=stdin <<zz
        COPYGROUP INDD=IN,OUTDD=OUT
        SELECT MEMBER=(MEMBER1,MEMBER2,MEMBER3,MEMBER4)
        SELECT MEMBER=(MEMBER5,MEMBER6,MEMBER7,MEMBER8)
zz
```

### Display The Directory Information For a Partitioned Data Set (Requires ZOAU)

The following example uses the `mvscmd` command and the **DFSMSdfp** utility `IEHLIST` to **extract and print** the **directory information** of **partitioned data set** `USER.LLQ.DATASET2`. The `LISTPDS` statement describes the **function** that `IEHLIST` will perform. The `VOL=3390=VOL001` statement tells `IEHLIST` that the **data set** being worked with resides on a **3390 DASD** _(Direct Access Storage)_ device and that the **volume serial** of that device is `VOL002`. The `DSNAME=USER.LLQ.DATASET2` statement tells `IEHLIST` to use the `USER.LLQ.DATASET2` data set.

&nbsp;

{: .warning }
> _The **volume serial** of the **DASD volume** that the data set **resides** on is **required**. In this example, the **volume serial** `VOL002` is the the volume that the data set `USER.LLQ.DATASET2` resides on._

&nbsp;

{: .warning }
> _The capital letter `X` on column **72** tells `IEHLIST` that there is more information on the next line. The `X` also **MUST** be on column **72**._

&nbsp;

```console
$ mvscmd --pgm=IEHLIST \
         --sysprint=stdout \
         --dd1=USER.LLQ.DATASET2,shr,volumes=VOL002 \
         --sysin=stdin <<zz
 LISTPDS VOL=3390=VOL002,FORMAT,                                       X
               DSNAME=USER.LLQ.DATASET2
zz
```

## Working with JES

Though it is often **beneficial** and even **essential** to keep work in the **foreground** so that it can be easily driven though **CI/CD tools** and **open programming languages**, it would be remiss to not consider how use of **JES** _Job Entry Subsystem_ is to submit work **asynchronously in batch** is hugely significant. Therefore, the following examples show how users can **submit JCL jobs** and **interact with resources** that would otherwise require the use of **SDSF panels** from **z/OS Unix System Services** instead.

### Submit a JCL Job From a Sequential Data Set (Requires ZOAU)

The following example uses the `jsub` command to **submit a JCL job** from the **sequential data set** `USER.LLQ.JCL`. The **JOBID** assigned by **JES** _(Job Entry Subsystem)_ is printed to the console. In this example, a **JOBID** of `JOB00002` is assigned to the submitted job.

&nbsp;

```console
$ jsub USER.LLQ.JCL
JOB00002
```

### Submit a JCL Job From a Partitioned Data Set Member (Requires ZOAU)

The following example uses the `jsub` command to **submit a JCL job** from the **partitioned data set member** `USER.LLQ.JCL(JOB)`. The **JOBID** assigned by **JES** _(Job Entry Subsystem)_ is printed to the console. In this example, a **JOBID** of `JOB00002` is assigned to the submitted job.

&nbsp;

```console
$ jsub 'USER.LLQ.JCL(JOB)'
JOB00002
```

### Submit a JCL Job From a UNIX File (Requires ZOAU)

The following example uses the `jsub` command to **submit a JCL job** from the **UNIX file** `/u/user/job.jcl`. The **JOBID** assigned by **JES** _(Job Entry Subsystem)_ is printed to the console. In this example, a **JOBID** of `JOB00002` is assigned to the submitted job.

&nbsp;

```console
$ jusb -f /u/user/job.jcl
JOB00002
```

### Cancel a Running JCL Job (Requires ZOAU)

The following example uses the `jcan` command to **cancel** a **running JCL job** with a **job name** of `JOB2` and a **JOBID** of `JOB00002`.

&nbsp;

```console
$ jcan C JOB2 JOB00002
```

### Cancel and Purge a JCL Job (Requires ZOAU)

The following example uses the `jcan` command to **cancel and purge** _(delete)_ a **JCL job** with a **job name** of `JOB2` and a **JOBID** of `JOB00002`.

&nbsp;

{: .warning }
> _**Additional authorization** is required to use the **purge** option._

&nbsp;

```console
$ jcan P JOB2 JOB00002
```

### Get a Listing of JCL Jobs (Requires ZOAU)

The following example uses the `jls` command to get a **listing of JCL jobs** that are **owned** by the userid `USER`.

&nbsp;

```console
$ jls /USER
USER     JOB1     JOB00001 CC       0000
USER     JOB2     JOB00002 CC       0000
```

### Display The Data Definitons Allocated to a JCL Job (Requires ZOAU)

The following examples uses the `ddls` command to **list all of data defininions** associated with a **JCL job** with a **JOBID** of `JOB00002`.

&nbsp;

```console
$ ddls JOB00002
JES2     -        JESMSGLG UA         133        23   2
JES2     -        JESJCL   V          136         4   3
JES2     -        JESYSMSG VA         137        20   4
CLONE    -        SYSPRINT VBA        137        26   102
```

### Display The JCL Used By a Submitted JCL Job (Requires ZOAU)

The following example uses the `pjdd` command to display the `JESJCL` **data definition** which contains the **JCL** used by the **JCL job** with a **JOBID** of `JOB00002`.

&nbsp;

```console
$ pjdd JOB00002 JESJCL
        1 //JOB2 JOB                                                           JOB00002
        2 //CLONE     EXEC PGM=ADRDSSU,REGION=0K
        3 //SYSPRINT  DD SYSOUT=A
        4 //SYSIN     DD *
```

### Display The Output Produced by a JCL Job (Requires ZOAU)

The following example uses the `pjdd` commad to display the `SYSPRINT` *data definition* which contains the **output** produced by the **JCL job** with a **JOBID** of `JOB00002`.

&nbsp;

```console
$ pjdd JOB00002 SYSPRINT
...
```

### Convert JCL Job Steps to mvscmd Commands (Requires ZOAU)

The following JCL job runs one **job step** named `CLONE` that uses the **DFSMSdss** utility `ADRDSSU` to **clone** a data set. The `CLONE` job step specifies a `SYSPRINT` **data definition** of `SYSOUT=A` and a `SYSIN` **data definition** of `*` where all of the code on the lines following the `SYSIN` **data definition** gets used by the `SYSIN` **data definition**.

&nbsp;

```console
//CLONEJOB JOB
//CLONE     EXEC PGM=ADRDSSU,REGION=0K
//SYSPRINT  DD SYSOUT=A
//SYSIN     DD *
 COPY  -
   DS(INC(USER.LLQ.DATASET2))  -
       RENUNC ( -
           USER.LLQ.DATASET2, -
           USER.LLQ.CLONE)
```

JCL is a **batch oriented language** meaning that JCL is intened to be scheduled and run **asynchronously** in **batch**. However, there is a myriad of powerful and even essential utilities that are **JCL oriented** that would provide a huge amount of value and ease of use if we were able to run these utilities in the **foreground** in the **z/OS UNIX System Services environment**.

&nbsp;

The following example is a conversion of the JCL above to an `mvscmdauth` command that can be run directly in the **z/OS UNIX System Services** environement in the **foreground**. The `SYSPRINT` **data definition** is changed to **UNIX** `stdout` and the `SYSIN` **data definition** is changed to **UNIX** `stdin` where the `zz` block tells **z/OS UNIX System Serivces** to treat the contained code which is identical to the code used in the JCL above as `stdin`. 

&nbsp;

{: .note }
> _The `mvscmd` command can be used in place of `mvscmdauth` to run programs that are **NOT APF** (Authorized Program Facility) **authorized**._

&nbsp;

```console
$ mvscmdauth --pgm=ADRDSSU \
             --sysprint=stdout \
             --sysin=stdin <<zz
 COPY  -
   DS(INC(USER.LLQ.DATASET2))  -
       RENUNC ( -
           USER.LLQ.DATASET2, -
           USER.LLQ.CLONE)
zz
```

## Running TSO Commands From z/OS UNIX System Services

Most **TSO** commands that **DON'T** invoke a **panel application** can be used from **z/OS UNIX System Services** using the `tsocmd` command and the `tso` command. The `tsocmd` command is functionally the **same** as the `tso` command, but the `tsocmd` command can execute **APF**  _(Authorized Program Facility)_ **authorized programs**.

&nbsp;

```console
$ tsocmd "HELP"
HELP
LANGUAGE PROCESSING COMMANDS:
...
$ tso "HELP"
HELP
LANGUAGE PROCESSING COMMANDS:
...
```

Here is an example that executes an **executable** stored in a **partitioned data set member**.

&nbsp;

```console
$ tso "CALL 'USER.LLQ.DATASET3(MAIN)'"
```

## Additional Resources

### ZOAU Commands:

* [`dls`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-dls)
* [`mls`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-mls)
* [`dmv`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-dmv)
* [`mmv`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-mmv)
* [`drm`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-drm)
* [`mrm`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-mrm)
* [`ddiff`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-ddiff)
* [`dtouch`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-dtouch)
* [`decho`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-decho)
* [`mvscmd`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-mvscmd)
* [`mvscmdauth`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-mvscmdauth)
* [`jsub`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-jsub)
* [`jcan`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-jcan)
* [`jls`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-jls)
* [`ddls`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-ddls)
* [`pjdd`](https://www.ibm.com/docs/en/zoau/1.2.x?topic=reference-pjdd)

### Built-in z/OS UNIX System Services Commands:

* [`tsocmd`](https://www.ibm.com/docs/en/zos/2.5.0?topic=scd-tsocmd-run-tsoe-command-from-shell-including-authorized-commands)
* [`tso`](https://www.ibm.com/docs/en/zos/2.5.0?topic=descriptions-tso-run-tsoe-command-from-shell)
* [`cat`](https://www.ibm.com/docs/en/zos/2.5.0?topic=descriptions-cat-concatenate-display-text-files)
* [`head`](https://www.ibm.com/docs/en/zos/2.5.0?topic=descriptions-head-display-first-part-file)
* [`tail`](https://www.ibm.com/docs/en/zos/2.5.0?topic=descriptions-tail-display-last-part-file)
* [`cp`](https://www.ibm.com/docs/en/zos/2.5.0?topic=descriptions-cp-copy-file)

### TSO Commands:

* [`LISTDS`](https://www.ibm.com/docs/en/zos/2.5.0?topic=subcommands-listds-command)
* [`RENAME`](https://www.ibm.com/docs/en/zos/2.5.0?topic=subcommands-rename-command)
* [`CALL`](https://www.ibm.com/docs/en/zos/2.5.0?topic=subcommands-call-command)

### DFSMShsm:

* [`HMIGRATE`](https://www.ibm.com/docs/en/zos/2.5.0?topic=hmds-using-tso-commands)
* [`HRECALL`](https://www.ibm.com/docs/en/zos/2.5.0?topic=hrds-using-tso-commands)

### DFSMSdss:

* [`ADRDSSU`](https://www.ibm.com/docs/en/zos/2.1.0?topic=volumes-using-dfsmsdss-dump-restore-commands)

### DFSMSdfp:

* [`IEBGENER`](https://www.ibm.com/docs/en/zos/2.5.0?topic=utilities-iebgener-sequential-copygenerate-data-set-program)
* [`IEBCOPY`](https://www.ibm.com/docs/en/zos/2.5.0?topic=utilities-iebcopy-library-copy-program)
* [`IEHLIST`](https://www.ibm.com/docs/en/zos/2.5.0?topic=utilities-iehlist-list-system-data-program)

### MISC:

* [`DSFS`](https://www.ibm.com/docs/en/zos/2.5.0?topic=guide-overview-zos-data-set-file-system)