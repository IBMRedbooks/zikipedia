---
layout: default
parent: References
---

# SYSLOG Command Reference

<hr>
&nbsp;

| **Command** | **What it does** | 
| `/d parmlib` | Shows current PARMLIB concatenation currently used by system. | 
| `/$D PROCLIB` | Shows current PROCLIB concatenation. | 
| `/d symbols` | Shows current symbols set on system. |
| `/d prog,apf` | Shows list of programs that are APF athorized. |
| `/d prog,lnklst` | Shows what libraries are in current linklist. |
| `/d asm` | Displays page information. |
| `/d asm,plpa` | Displays page info about PLPA. |
| `/d xcf` | Shows what sysplex the current lpar is part of. | 
| `/d ts,l` | Shows time-sharing tasks (users). |
| `/k c,a,0000000-9999999` | Deletes all “/d r,l” messages. |
| `Xdc –In SDSF next to file.` | Dumps output to a file. |
| `/DS QD,7517,1` | Shows device level info for device 7517. |
| `/devserv p,B1C4` | Shows device level info for device B1C4.| 
| `/d IOS,CONFIG` | Shows which IODF config file is active. |
| `/f hsm,backvol cds` | Peforms HSM CDS backup.  Will create incremental HSM.*CDS file. |
| `/f hsm,query cdsversionbackup` | Tells info about HSM backup. |
| `/f dbp1irlm,abend,nodump` | If don’t have authority to take down DB2, use this work-around. |
| `/d m=stor` | Shows how much storage memory (CPU) allocated. |
| `/d m=cpu` | Shows how many CP’s are allocated. |
| `/d m=chp(25)` | Shows CHPID info. |
| `/d consoles,l,f` | Shows console devices and connected devices. |
| `/d vs,lfarea` | Display virtual storage statistics. |
| `/d omvs,a=all` | Display info about OMVS. | 
| `/d omvs,o` | Display OMVS settings from BPXPRM member. | 
| `/d omvs,pipes` | Display OMVS user usage. |
| `/d omvs,f` | Displays mounts. | 
| `/d u,,alloc,72fac,1` | Displays allocations holds on device. | 
| `/f catalog,unallocate(ucatdb2.db12test)` | Unallocates a user catalog. | 
| `/f catalog,allocated` | Shows all the active catalogs. | 
| `/f catalog,report,cache` | Shows catalog cache status for each catalog. |
| `/f catalog,report,performance` | Shows catalog performance statistics. | 
| `/f catalog,report,performance(reset)` | Resets performance stats. | 
| `/d m=dev(7100)` | Shows CHP’s for current device. | 
| `/d m=chp(8d)` | Displays CHPID status. | 
| `/cf chp(61),online` | Configureda CHPID line. |
| `/d sms` | Tells SMS active datasets. | 
| `/d sms,lib(all)` | Shows SMS vitual tape libs activated. |
| `/v sms,lib(zvts),online` | Vary’s on SMS Virtual Tape library. | 
| `/v net,act,id=db2blu,scope=all` | Activates a VTAM LU. |
| `/F CEA,D,PARMS` | Displays CEA parms. |
| `/!MQxx STOP QMGR` | Stop MQ manager. |
| `/!MQS2 START QMGR,PARM=’MQS2PARM’` | Start MQ Manager. |
| `/d ssi` | Shows active subsystems. |
| `/d grs` | Shows some basic system info. |
| `/d logger,l` | Displays any LOGGER info on the system. |
| `/d logger,l,lsname=ifasmf.inmem` | Displays a specific logstream. |
| `/d logger,c` | Shows summary of Logstreams. |
| `/d logger,str` | Shows structures allocated to logstreams |
| `/d logger,c,lsn=ifasmf.ckqradar,detail` | Shows gobs of detail about Logstream. |
| `/s ixgoflds,logstream=<logstream.name>` | Forces offload of logstream. |
| `/d xcf` | Shows what sysplex the current lpar is part of. |
| `/d xcf,pi` | Show XCF/sysplex path-in-addresses. |
| `/d xcf,po` | Show XCF/sysplex path-out-addresses. |
| `/d xcf,couple` | Coupling facility, tells current datasets in use. |
| `/d xcf,cf` | Logical coupling facility information. |
| `/d xcf,policy` | Current active policy of coupling facility. |
| `/d xcf,structure` | Current policy of coupling facility active structure. |
| `/setxcf start,rebuild,strname=istgeneric` | Start and rebuild coupling facility structure. Have to do this when altering structure sizes.
| `/d xcf,str,strname=________` | Snapshot in time of current coupling facility status. | 
| `/d cf` | Hardware coupling facility information. |
| `/d xcf,policy,type=cfrm` | Shows policies pending. |
| `/d xcf,s` | Shows summary of LPARs in Sysplex. |
| `/d xcf,sysplex` | Shows LPAR’s in a Sysplex. |
| `/setxcf force,pndstr,cfname=Z13CF06` | Forces “pending” structures. |
| `/setxcf start,policy,type=cfrm,polname=SHARJUDY` | Start a policy that has been defined by JCL. |
| `/setxcf force,str,strname=DBSG_LOCK1` | Forcefully deletes a structure. Potentially Dangerous. |
| `/setxcf start,reallocate` | A cmd to try when all else fails. | 
| `/d xcf,str,stat=policychange` | See pending things for policy change. |
| `/$D NODE` | Displays JES2 nodes. |
| `/$D MEMBER` | Displays JES2stuff. |
| `/$DSPL` | Displays JES Spool size. |
| `/MVS $P O JQ,A>14` | Purges a bunch of stuff from JES Spool. |
| `/$D NODES` | Shows JES2 nodes. |
| `/$D SPOOL` | Shows info about JES Spool. |
| `/$D SPOOLDEF` | Shows more info about the JES Spool. |
| `/$PJOBQ,JOBMASK=’CTC19*’` | From HMC, purges jobs from JESSPOOL (if full). |
| `/$DJOBQ,SPOOL=(%>5)` | Shows JES Spool jobs > 5%. |
| `/$DSPL` | Displays JES Spool utilization. | 
| `/$T A PQ,'$POJOBQ,READY,Q=ACDEFGHIJKMNOPQRSTUVWXYZ0123456789,DAYS>30'$HASP604 ID PQ   T= 12.47 I= 7200 L=INTERNAL $POJOBQ,READY,Q=ACDEFGHIJKMNOPQRSTUVWXYZ0123456789,DAYS>30` | Sets purge criteria to 30 days. | 
| `/R=LOCAL.*,ALL,CANCEL,DAYS>7` | Purges JESSPOOL jobs from output queue. |
| `/$TESTBYTE,NUM=499999,OPT=1` | Cancels a job if spool size exceeds certain threshold. |
| `/D IPLINFO` | Shows last ipl info. |
| `/c u=vanwag` | Cancels userid/force u=vanwagWhen all else fails to cancel id. |
| `/CANCEL AZKS,A=71` | Force cancel a duplicate started task. A=ASID. | 
| `/d smf,o` | Displays parameters in effect for SMF. |
| `/set smf=00` | Causes SMF to suck in the ...PARMLIB(SMFPRM00) member. | 
| `/d omvs,a=all` | Shows users and states | 
| `/set omvs=pu` | Re-drives the PARMLIB(BPXPRMxx) member. |
| `/f omvs,shutdown` | WARNING! Shutdown OMVS/USS. |
| `/setomvs syntaxcheck(sh)` | Checks syntax for <hlq>.PARMLIB(BPXPRMSH). |
| `/p rmf` | Stops RMF. |
| `/S RMFSLO,JOBNAME=RMF` | Starts RMF – make sure you look in ___.PARMLIB(COMMANDxx) for parms. | 
| `/F RMF,D ZZ` | Displays RMF parms in memory. |
| `/F HZSPROC,DEACTIVATE,CHECK=(IBM*,*)` | Turn off Health Check. |
| `/F ZFS,QUERY,ALL` | Shows cool stats on ZFS. | 
| `/RACF SET LIST` | Shows template version and other misc info. |
| `/d tcpip,,netstat,allconn` | Shows TCP/IP addresses like TSO HOMETEST. |
| `/d tcpip,,netstat,home` | Equivalent of HOMETEST. |
| `/d tcpip,tn3270,prof` | Tells if TCPIP (PCOMM) port is open. Look for “PROF: CURR CONNS:”. Should be “1”. If not starting, make sure PAGENT is running on system. |
| `/d tcpip,tcpip,stor` | TCP/IP storage info. |
| `/d tcpip,tcpip,netstat,stats,protocol=tcp` | More info on TCP. |
| `/d tcpip,telnet` | Shows status of TN3270. |
| `/d tcpip,tn3270,conn` | Shows all TCPIP connections for TN3270. |
| `/d tcpip,tn3270,conn,conn=0006ca3b` | Shows details about particular connection. |
| `/v tcpip,tcpip,obeyfile,dsn=pel.plexb.parmlib(peldata)` | |
| `/d tcpip,,netstat,devl` | Shows info about Hipersockets. |
| `/d net,trl,control=roce` | Shows info about RoCE card. |
| `/d net,id=ATSS1005` | |
| `/d sms` | Tells SMS datasets active on system. |
| `/d sms,volume(pel001)` | If SMS managed, returns info about the volume. |
| `/D SMS,STORGRP(SDUMP),LISTVOL` | Tells what volumes are in storage group (DFSMS). |
| `/DS SMS,7E35,1` | Tells if SMS managed. |
| `/RACFTARGET LIST` | Tells if RACF is synced to another system (RRSF). |
| `/-DB11STO DB2` | Stop DB2. |
| `/-DB11STA DB2` | Start DB2. |
| `/-DB11DIS DDF DETAIL` | Shows info about DB2. |
| `/c asks.starting` | Cancels a task that is in starting mode and duplicate to an already started task. |
| `/d m=dev(5000)` | Displays device 5000. |
| `/d m=chp(50)` | Displays CHPID 50 | 
| `/v path(50000,51),offline` | Varies device 5000 offline from CHPID 51. |
| `/cf chp(51),offline` | Varies CHPID 51 offline. |