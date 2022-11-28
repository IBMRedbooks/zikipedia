---
layout: default
parent: References
---

# z/OS Unix System Services Command Reference for Sysprogs

<hr>
&nbsp;

| **Command** | **What it does** | 
| `mkdir J1.7` | Makes a new directory where you are. |
| `chmod 777 J1.7` | Grants authority to directory J1.7. |
| `df` | Shows mount points. |
| `/samples/copytree -sa /from_dir/ /to_dir/` | Copies directory + contents preserving UID, etc... |
| `extattr +p ./bbgzsrv` | Set “program” extended attribute on a file. |
| `rm –rf <dir-name>` | Removes directory and EVERYTHING under it. |
| `chown ivandov:SYSPROG /u/ivandov` | Change ownership of USS userid to SYSPROG. |
| `ls -EL` | Shows extended attributes. |
| `ps –ef` | Shows processes running | 
| `kill -9 [pid]` | Substitute the PID from above command for [pid]. |
| `stty columns 1500` | Allows you to scroll right to see stuff off the screen. |
| `du -s *` | Recursive file size –shows directory sizes! |
| `find /etc -name .bashrc` | Searches /etc/ directory for “.bashrc” string. |
| `find -L /var/www/ -type l` | Searches for attribute for symlink. | 
| `chtag -t -c ISO8859-1 /some/dir/file` | Change encoding to ASCII. |
| `tsocmd “addsd (‘....` | Do TSO commands from Unix. |
| `ls -lR /tmp/ > /u/vanwag/test` | Recursively list all directories and put to file. | 
| `zospt ps -a` | Tells if z/OSPT is setup. |
| `ls -alT /usr/lpp/IBM/izoda` | Shows tagging. |
| `java -XshowSettings` | Shows Java settings |
| `ln-s /usr/lpp/IBM/izoda/some_file /mylink_somefile` | Creates SymLink. |
| `jar tf dv-jdbc.jar` | Shows jars available in a Sym Link. |
| `ps-ef | grep GROUP` | Shows threads in use. | 
| `du -a /var | sort -n -r | head -n 10` | Shows top 10 largest files in directory /var. |