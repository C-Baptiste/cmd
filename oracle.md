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
- col name format a10

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

## Multitenancy and sharding
- pluggable database PDB
- find $ORACLE_HOME -name *login.sql*
- select con_id, name, open_mode from v$containers;
- root container = CDB$root
- PDB$seed

## Dictionary prefixes 
- static non-multitenant views
	- DBA_
	- ALL_
	- USER_
- dynamic performance views
	- V$_
 	- GV$_
- static multitenant related views
	- CDB_
## Common dictionary suffixes
- _INDEXES
- _JOBS
- _TRIGGERS
- _SYNONYMS
- _PROCEDURES
- _SEQUENCES
- _CONTAINERS
- _VIEWS
- _TABLES
- _OBJECTS
- select distinct object_type from dba_objects;
- select distinct object_type from all_objects;
- select distinct object_type from user_objects;

## instance
- startup (sqlplus cmd)
- select name, value from v$parameter where name like '%spf%' or name like 'co%es';
- select name from v$datafile;
- select member from v$logfile;

#### stages of startup
- nomount -> spfile -> build the instance
- mount -> control files -> mounts the database
- open -> data files + redo logs -> opens the db  
select status from v$instance;  
ALTER DATABASE OPEN;
# manage users, roles and privileges

## create user account
- show con_name;
- SELECT CON_ID, NAME, OPEN_MODE FROM V$PDBS;
- alter session set container = FREEPDB1;
- create user zoo identified by oracle default tablespace zoospace password expire;
- select username, account_status,default_tablespace from dba_users;


## common system privileges
- create session (lets users connect)
- alter database
- alter system
- create tablespace
- create table (own schema)
- grant any object privileges
- create any table (all schema)
- select any table

#### command
- grant create any table to zoo with admin option;
- select distinct(privilege) from dba_sys_privs;

### object privileges
- grant select on zoo.table to ...
- select
- insert
- update
- delete
- alter
- execute
- grant create session zoo;

### roles
- create role xxx;
- managing privilege with roles
- grant select ... to role xxx
- grant role to user...
#### predefined role
- connect
- resource
- dba
- select_catalog_tolr
- public

### profiles
- composite_limit
- connect_time
- cpu_per_call
- cpu_per_session
- idle_time
- logical_reads_per_call
- logical_reads_per_session
- private_sga
- sessions_per_user
- ...

### PDB
- ALTER SYSTEM SET DB_CREATE_FILE_DEST = '/opt/oracle/oradata';
- CREATE PLUGGABLE DATABASE zoo ADMIN USER zoo IDENTIFIED BY zoo;

