## sql
- CREATE TABLE "C##ECONOMICS".STOCK (
	ID NUMBER(38,0),
	NAME VARCHAR2(20),
	OWNERSHIP NUMBER(38,0),
	LAST_PRICE FLOAT,
	LAST_BUY DATE,
	EARNINGS DATE,
	CONSTRAINT STOCK_PK PRIMARY KEY (ID),
	CONSTRAINT SYS_C008470 CHECK ("ID" IS NOT NULL)
);
CREATE UNIQUE INDEX STOCK_PK ON "C##ECONOMICS".STOCK (ID);

- CREATE TABLE "C##ECONOMICS".ACCOUNT (
	ID INTEGER NOT NULL,
	NAME VARCHAR2(100) NULL,
	CONSTRAINT ACCOUNT_PK PRIMARY KEY (ID)
);
- ALTER TABLE "C##ECONOMICS".STOCK ADD ACCOUNT INTEGER NULL;
- ALTER TABLE "C##ECONOMICS".ACCOUNT ADD VALUE FLOAT NULL;
- ALTER TABLE "C##ECONOMICS".ACCOUNT RENAME COLUMN "account_id" TO ACCOUNT_ID;
- ALTER TABLE "C##ECONOMICS".STOCK ADD CONSTRAINT STOCK_ACCOUNT_FK FOREIGN KEY (ACCOUNT_ID) REFERENCES "C##ECONOMICS".ACCOUNT(ACCOUNT_ID) ON DELETE CASCADE;
- CREATE TABLE "C##ECONOMICS".VALUE (
	VALUE_ID INTEGER NOT NULL,
	ACCOUNT_ID INTEGER NULL,
	DAILY DATE NULL,
	VALUE FLOAT NULL,
	CONSTRAINT VALUE_PK PRIMARY KEY (VALUE_ID),
	CONSTRAINT VALUE_ACCOUNT_FK FOREIGN KEY (ACCOUNT_ID) REFERENCES "C##ECONOMICS".ACCOUNT(ACCOUNT_ID) ON DELETE CASCADE
);
- ALTER TABLE "C##ECONOMICS".ACCOUNT DROP COLUMN VALUE;
- CREATE TABLE "C##ECONOMICS".DIVIDEND (
	DIVIDEND_ID INTEGER NULL,
	STOCK_ID INTEGER NULL,
	YEARLY INTEGER NULL,
	DIVIDEND FLOAT NULL,
	CONSTRAINT DIVIDEND_STOCK_FK FOREIGN KEY (STOCK_ID) REFERENCES "C##ECONOMICS".STOCK(STOCK_ID) ON DELETE CASCADE
);


## oracle symfony 

- symfony console list make
- symfony console make:entity -h
- symfony console list doctrine
- symfony console doctrine:database:create -h
- composer require symfony/orm-pack
- composer require --dev symfony/maker-bundle
- symfony console doctrine:database:create
- symfony console doctrine:database:create -h
- dnf search php-pear
- dnf search php-pecl
- sudo dnf install php-pear
- whereis pecl
- sudo pecl channel-update pecl.php.net
- sudo dnf install php-devel
- sudo pecl install oci8
- dnf search instantclient
- sudo dnf install oracle-instantclient-basic-23.5.0.24.07-1.el9.x86_64.rpm 
- sudo dnf install oracle-instantclient-devel-23.5.0.24.07-1.el9.x86_64.rpm 
- sudo pecl install oci8
- [root@lab0 ~]# grep oci8 /etc/php.ini   
extension=oci8.so
- symfony console doctrine:database:create
- symfony console doctrine:database:drop


- grant create session to C##ECONOMICS;
- SELECT tablespace_name, file_name FROM dba_data_files;
- alter session set container = economics;
- create tablespace economics datafile 'economics01.dbf' size 100M autoextend on next 100M maxsize 500M;
- select tablespace_name, con_id from cdb_tablespaces;
- alter user c##economics default tablespace economics;
- alter user economics quota 300M on economics;

- col username format a10;SQL> col account_status format a10;SQL> select username, account_status,default_tablespace from dba_users;


- CREATE PLUGGABLE DATABASE economics
  ADMIN USER economics IDENTIFIED BY economics
  ROLES = (dba)
  DEFAULT TABLESPACE economics
    DATAFILE '/opt/oracle/oradata/FREE/economics/economics01.dbf' SIZE 250M AUTOEXTEND ON
  FILE_NAME_CONVERT = ('/opt/oracle/oradata/FREE/pdbseed/',
                       '/opt/oracle/oradata/FREE/economics/')
  STORAGE (MAXSIZE 1G)
  PATH_PREFIX = '/opt/oracle/oradata/FREE/economics/';
- show pdbs;
- alter pluggable database economics open;



### .env db url connection
- https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/configuration.html#configuration
- service (boolean): Whether to use Oracle's SERVICE_NAME connection parameter in favour of SID when connecting. The value for this will be read from Doctrine's servicename if given, dbname otherwise.
- doctrine.yaml -> service: true
- other idea: not tested
	- USE_SID_AS_SERVICE_listener=on -> add this in listener.ora file
- symfony console doctrine:query:sql "select table_name from user_tables"
- DATABASE_URL="oci8://user:pwd@oracleServer:1521/dbnameOrServicename"

- cd economics_b2
- code .
- .env & doctrine.yaml
- symfony console doctrine:query:sql "select table_name from user_tables"
- symfony console make:entity
- symfony console make:migration
- symfony console doctrine:migrations:migrate
- symfony console doctrine:query:sql "select table_name from user_tables"
- composer require easycorp/easyadmin-bundle
- php bin/console make:admin:dashboard
- php bin/console make:admin:crud

- symfony console doctrine:migrations:execute 'DoctrineMigrations\Version20241021163532' --down
