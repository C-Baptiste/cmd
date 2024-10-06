## basics
- ! ipcs -a
- show parameters db_create_f
- $ORACLE_HOME
- find $ORACLE_HOME -name *spfile*
	- /opt/oracle/product/23ai/dbhomeFree/dbs/spfileFREE.ora

- create pfile='/tmp/pfile.txt' from spfile;
- create spfile from pfile='/tmp/pfile.txt';

- select count(*) from v$parameter;  ->540
- startup force
- startup 

### need an instance to open database
### need an spfile to construct that instance

## usefull troubleshooting file
- find $ORACLE_BASE -name *alert*log*
	- /opt/oracle/diag/rdbms/free/FREE/trace/alert_FREE.log
- select * from dba_data_files;
	- SQL> col tablespace_name format a15
	- SQL> col file_name format a50 
	- SQL> select file_name, tablespace_name from dba_data_files;

	FILE_NAME					                      TABLESPACE_NAME
	-------------------------------------------------- ---------------
	/opt/oracle/oradata/FREE/system01.dbf		   SYSTEM
	/opt/oracle/oradata/FREE/sysaux01.dbf		   SYSAUX
	/opt/oracle/oradata/FREE/undotbs01.dbf		   UNDOTBS1
	/opt/oracle/oradata/FREE/users01.dbf		   USERS

- CREATE TABLE emps TABLESPACE USERS AS SELECT username from dba_users;

## controle file stores:
- systeme checkpoint info 
- RMAN info
- physical database structure

	- SQL> show parameter control_files
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

## redo logs
- instance recovery (system crash)
- media recovery (drive failure)
- standby db process
- replication 
- logminer

ACID test 
atomicity consistency isolation durabilaty

- select g.group#, g.status, f.member from v$log g, v$logfile f where g.group# = f.group#; 
		 3 INACTIVE
	/opt/oracle/oradata/FREE/redo03.log
	
		 2 INACTIVE
	/opt/oracle/oradata/FREE/redo02.log
	
		 1 CURRENT
	/opt/oracle/oradata/FREE/redo01.log


- alter system switch logfile (eg: during a maintenance)


## AUTH
- system = user
- sys = root
- OS auth & password file

## PGA and UGA and SGA
- pga process global area -> specific to an individual process
- uga user global area -> stores session state
- sga system/shared global area
- show parameter workarea

## SGA components
- redo buffer (store instance recovery)
- buffer cache (cache block of data, speed up data access)
- shared pool (speed up query)
- large pool 
- ...
- select * from v$sgainfo;
- select table_name, tablespace_name from dba_tables where table_name='EMPS';
- select file_name, tablespace_name from dba_data_files where tablespace_name='USERS';
- select t.table_name, f.file_name from dba_tables t, dba_data_files f where t.tablespace_name = f.tablespace_name and t.table_name = 'EMPS'; (get physical location of table .dbf)
- https://docs.oracle.com/en/database/oracle/oracle-database/23/dbiad/db_sharedpool.html
- select * from v$sgastat
- show parameter sga_t
- show parameter memory_target
- show parameter log_buffer

## PGA process global area
- dedicated server processes
- shared server processes
- background processes
	- LREG listener registrar
 	- SMON system monitor
  	- DBWn database block writer
  	- CKPT checkpoint
  	- LGWR log writer
- select g.group#, g.status, f.member from v$log g, v$logfile f where g.group# = f.group# order by f.group#;
- alter system switch logfile;
- alter system checkpoint;

## Logical structure
- tablespace
	- select t.table_name, t.tablespace_name, f.file_name from dba_tables t, dba_data_files f where t.tablespace_name = f.tablespace_name and t.table_name = 'EMPS'; 
- segments
- extents

## Resume
- datafiles -> physical storage
- tablespace -> organise datafiles
- segments -> represent database objects into tablespace
- extents -> make up segments, contiguous data blocks
