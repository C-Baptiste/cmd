! ipcs -a

show parameters db_create_f

$ORACLE_HOME
find $ORACLE_HOME -name *spfile*
/opt/oracle/product/23ai/dbhomeFree/dbs/spfileFREE.ora

create pfile='/tmp/pfile.txt' from spfile;
create spfile from pfile='/tmp/pfile.txt';

select count(*) from v$parameter;  ->540

startup force
startup 

# need an instance to open database
# need an spfile to construct that instance

# usefull troubleshooting file
find $ORACLE_BASE -name *alert*log*
/opt/oracle/diag/rdbms/free/FREE/trace/alert_FREE.log

select * from dba_data_files;

SQL> col tablespace_name format a15
SQL> col file_name format a50 
SQL> select file_name, tablespace_name from dba_data_files;

FILE_NAME					                      TABLESPACE_NAME
-------------------------------------------------- ---------------
/opt/oracle/oradata/FREE/system01.dbf		   SYSTEM
/opt/oracle/oradata/FREE/sysaux01.dbf		   SYSAUX
/opt/oracle/oradata/FREE/undotbs01.dbf		   UNDOTBS1
/opt/oracle/oradata/FREE/users01.dbf		   USERS

CREATE TABLE emps TABLESPACE USERS AS SELECT username from dba_users;

# controle file stores:
- systeme checkpoint info 
- RMAN info
- physical database structure

SQL> show parameter control_files

NAME				     TYPE	 VALUE
------------------------------------ ----------- ------------------------------
control_files		string	 /opt/oracle/oradata/FREE/contr
          						 ol01.ctl, /opt/oracle/oradata/
          						 FREE/control02.ctl


file                                       system critical        business critical
server param file                          yes                    no
alert log file                             no                     no
data files                                 mostly no              yes
control file                               yes                    no
online/redo logs                           yes                    yes
password file                              no                     no

#redo logs
- instance recovery (system crash)
- media recovery (drive failure)
- standby db process
- replication 
- logminer

ACID test 
atomicity consistency isolation durabilaty

select g.group#, g.status, f.member from v$log g, v$logfile f where g.group# = f.group#; 
	 3 INACTIVE
/opt/oracle/oradata/FREE/redo03.log

	 2 INACTIVE
/opt/oracle/oradata/FREE/redo02.log

	 1 CURRENT
/opt/oracle/oradata/FREE/redo01.log


alter system switch logfile (eg: during a maintenance)


------AUTH----------
system = user
sys = root
OS auth & password file


