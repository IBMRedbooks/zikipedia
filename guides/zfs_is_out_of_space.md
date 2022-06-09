---
layout: default
permalink: guides/zfs_is_out_of_space/
---

# ZFS is Out of Space

So, someone just came to you complaining that their ZFS is out of space.  Here is a nifty utility that you can use to bump the ZFS up.

&nbsp;

1. First, make sure `/usr/sbin/` is in your `$PATH` variable in USS. You can easily do this by adding it to your `.profile` in USS. Here is an example:
  ```shell
PATH=/usr/sbin:$PATH
echo $PATH
  ```

2. Next, allocate and format the new ZFS to the size you need.
```
//ZFSALLOC JOB NOTIFY=&SYSUID,CLASS=1                                               
/*JOBPARM S=*                                           
//********************************************************************* 
//* Allocate ZFS.                                                     * 
//* Instructions:                                                     * 
//*   1. Change @ZFS_DSN@ to the ZFS dataset name.                    * 
//*   2. Change @OWNER@   to the userid in USS to be owner.           * 
//*   3. Change @GROUP@   to the group  in USS to be owner. (DEFLT1)  * 
//*   4. Submit and grab hold of somethin'.                           * 
//********************************************************************* 
//STEP01   EXEC   PGM=IDCAMS                                            
//SYSPRINT DD     SYSOUT=H                                              
//SYSUDUMP DD     SYSOUT=H 
//AMSDUMP  DD     SYSOUT=H                                              
//SYSIN    DD     *                                                     
  DEFINE CLUSTER(                    
            NAME(@ZFS_DSN@) -
            VOLUMES(ZF8D1D) -
            DATACLAS(DCZFS) -
            LINEAR -
            MEGABYTES(2000,100) -
            SHAREOPTIONS(3,3) -
                 )
/*                                                                      
//IF1      IF STEP01.RC=0 THEN                                          
//********************************************************************* 
//* Format the new ZFS.                                               * 
//********************************************************************* 
//ZFSNEWA  EXEC   PGM=IOEAGFMT,REGION=0M,                               
//      PARM=('-aggregate @ZFS_DSN@                                    
//              -compat -owner @OWNER@ -group @GROUP@')                 
//SYSPRINT DD   SYSOUT=H                                                
//STDOUT   DD   SYSOUT=H                                                
//STDERR   DD   SYSOUT=H              
//SYSUDUMP DD   SYSOUT=H                                                
//CEEDUMP  DD   SYSOUT=H                                                
//         ENDIF                                                        
/*
```

3. Next, instead of playing games with ZFS swapping and copying, etc... just run this job:
```
//ZFSBIGGR JOB NOTIFY=&SYSUID,CLASS=1                                              
/*JOBPARM S=*                                                          
//*********************************************************************
//* Swap-a-roo a ZFS file system to make it bigger (or smaller).      *
//* ----------------------------------------------------------------- *
//* Instructions:                                                     *
//*   1. Separate from this job, allocate your NEW ZFS.               *
//*   2. Validate your source ZFS is mounted.                         *
//*   3. Change @S_DSN@ to the source ZFS dataset name.               *
//*   4. Submit.                                                      *
//* ----------------------------------------------------------------- *
//*  This JCL will run the BPXWMIGF (xFS to zFS migration)            *
//*  command.  This job just initiates the request, look              *
//*  in SYSLOG for the BPXU008I messagefor completion or              *
//*  use TSO BPXWMIGF zfsname to check status                         *
//*  or                                                               *
//*  bpxwmigf -query                                                  *
//*                                                                   *
//*  -The source zFS must be mounted.                                 *
//*  -The job needs to run on the system where the source zFS         *
//*    is mounted.                                                    *
//*  -The new zFS must be pre-allocated.                              *
//*  -The source will be renamed to .OLD and the target will          *
//*    be renamed to the source.                                      *
//*  set -o xtrace will echo the command                              *
//*********************************************************************
//STEP1   EXEC  PGM=BPXBATCH,REGION=0M                                 
//STDOUT  DD  SYSOUT=*                                                 
//STDERR  DD  SYSOUT=*
//STDPARM DD  *                                                        
SH set -o xtrace &&                                                    
      bpxwmigf -source @S_DSN@ \
  -target @S_DSN@.new \
  -trename \
  -srename @S_DSN@.old \
  -swap                        
//*    
```