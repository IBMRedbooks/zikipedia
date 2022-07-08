---
layout: default
parent: References
---

# JES2 Command Reference

<hr>
&nbsp;

| **Command** | **What it does** | 
| `/$DI` | Displays all initiators and associated classes. | 
| `/$D INITDEF` | Displays number of JES2-managed init’s. |
| `/$DJES2` | Displays current activity in JES2 address space. | 
| `/$DJQ,DAYS>3` | Displays jobs whose spool volume is > 3 days old. |
| `/$D SPOOLDEF` | Displays SPOOL dataset characteristics. | 
| `/$JD STATUS` | Checks and displays if JES2 is functioning. | 
| `/$P14` | Stops initiator 14 after current job processed. | 
| `/$P JES2` | Terminates JES2 normally. |
| `/$P JES2,ABEND,FORCE` | Terminates JES2 abnormally and instantly. | 
| `/$S` | Starts JES2 processing, initiators, etc.... |
| `/$S14` | Starts JES2 initiator 14. |
| `/$TSPOOLDEF,TGSPACE=(WARN=400), LARGEDS=ALLOWED,TGSIZE=80` | Increases the SPOOL dataset size. |
| `/$djq,spl=(%>5)` | Displays all jobs which occupy > 5% of spool. |
| `/$D I1` | Displays initiator 1. |
| `/$DN,Q=XEQA` | Displays class A spool utilization. | 
| `/$JD DETAILS` | Shows JQES + JNUM + JOES + BERT | 
| `/$JD HISTORY(JNUM)` | Shows running history of JNUM parm. |
| `/$D JOBDEF` | Shows JOBDEF specifications./$D CKPTSPACE,BERTUSEShows BERT utilization |