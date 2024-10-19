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


