
# Oracle Database (Step By Step):

Here you can find solution of Week-9: Q.no: 08



## Login to the System Database User :

##### *Default Username:  system & Password: system*


```bash
SQL> connect system; 

Enter password: system

Connected.....

```

## Creating New Database User & Password

##### *This command will help you create new user which contains fresh database*

```bash
  CREATE USER {insurance_user} IDENTIFIED BY {password};

  GRANT CONNECT, RESOURCE TO {insurance_user};
  
  CONNECT {insurance_user}
  Enter password: {password}
```

##### *You can also use the system database and create tables there but it will contains some predefined tables, constraints etc*



##  Creating TABLES:

##### *This command will help you create new tables*

```bash
CREATE TABLE PERSON(driver_id VARCHAR2(20) PRIMARY KEY, name VARCHAR2(20), address VARCHAR2(20));

CREATE TABLE CAR(reg_no VARCHAR2(20) PRIMARY KEY, model VARCHAR2(20), year NUMBER(12,0));

CREATE TABLE ACCIDENT(report_number NUMBER(12,0) PRIMARY KEY, accd_date DATE, location VARCHAR(20));

CREATE TABLE OWNS(driver_id VARCHAR(20),reg_no VARCHAR(20));

CREATE TABLE PARTICIPATED(driver_id VARCHAR(20), reg_no VARCHAR2(20), report_number NUMBER(12,0), damage_amt NUMBER(12,0));
```

##### *You can also define foreign keys/primarys here, But I will add some of Primary keys & foreign keys using 'ALTER' command.*


##  Adding FOREIGN KEYS in remaining Tables:

##### *This commands will help you alter tables constraints like primary keys & foreign keys.*

```bash
ALTER TABLE OWNS ADD CONSTRAINT fk_id1 FOREIGN KEY (driver_id) REFERENCES PERSON (driver_id);

ALTER TABLE OWNS ADD CONSTRAINT fk_id2 FOREIGN KEY (reg_no) REFERENCES CAR (reg_no);

ALTER TABLE PARTICIPATED ADD CONSTRAINT fk_dri_id FOREIGN KEY (driver_id) REFERENCES PERSON (driver_id);

ALTER TABLE PARTICIPATED ADD CONSTRAINT fk_reg_no FOREIGN KEY (reg_no) REFERENCES CAR (regno);

ALTER TABLE PARTICIPATED ADD CONSTRAINT fk_rep_no FOREIGN KEY (report_number) REFERENCES ACCIDENT (report_number);
```


##  Adding PRIMARY KEY with Two Columns:

##### *This commands will help you add primary keys on the remaining tables with two columns.* 
#### *"To make each column UNIQUE"*

```bash
ALTER TABLE OWNS ADD CONSTRAINTS pk_owns PRIMARY KEY(driver_id,reg_no);

ALTER TABLE PARTICIPATED ADD CONSTRAINTS pk_parti PRIMARY KEY(driver_id, reg_no, report\number);
```


##  Inserting Data in Tables:

##### *This commands will INSERT data in the pre-defined tables.* 

#### *"INSERT one by one"* 

```bash
INSERT INTO PERSON VALUES ('D10001','Altaf','Aligarh');
```

#### *"INSERT multiple rows at once"* 

```bash
INSERT ALL

INTO PERSON VALUES('D10002','Rahul','Mewat')

INTO PERSON VALUES('D10003','Rohan','Aligarh')

INTO PERSON VALUES('D10004','Rakesh','Aligarh')

INTO PERSON VALUES('D10005','Asif','Delhi')

SELECT  * FROM dual;
```

#### *"INSERT data having type DATE: (multiple rows)"* 

```bash
INSERT ALL

INTO ACCIDENT VALUES(101, TO_DATE('30-09-2025', 'DD-MM-YYYY'), 'Aligarh')

INTO ACCIDENT VALUES(102, TO_DATE('29-09-2025', 'DD-MM-YYYY'), 'Delhi')

INTO ACCIDENT VALUES(103, TO_DATE('29-09-2025', 'DD-MM-YYYY'), 'Mewat')

INTO ACCIDENT VALUES(104, TO_DATE('28-09-2025', 'DD-MM-YYYY'), 'Delhi')

INTO ACCIDENT VALUES(105, TO\_DATE('27-09-2025', 'DD-MM-YYYY'), 'Kanpur')

SELECT  * FROM dual;
```

##  UPDATE Data in Tables:

```bash
UPDATE PARTICIPATED

SET DAMAGE_AMT = 25000

WHERE REPORT_NUMBER = 102;

COMMIT;

```

#### *"Oracle Queries are not case sensitive"* 

```bash
update accident

set accd_date = to_date('30-09-2008','DD-MM-YYYY')

where report_number in (101,105,103);

commit;

```

##  EXTRACTING the desired information from Tables:

#### *"Remember I have used these commands as per my research and knowledge, you can apply different approach to solve the same problem, It is Recommended to try by yourself."*
**Here I have used 'WHERE' clause to solve the 2nd last problems, you can use 'JOIN' statements or any other approach.**

```bash
SELECT COUNT(*) AS Total

FROM accident a,

owns o,

participated p

WHERE a.report_number = p.report_number

AND o.driver_id = p.driver_id

AND o.reg_no = p.reg_no

AND EXTRACT(YEAR FROM a.accd_date) = 2008;

```



## More Tutorials

- [Oracle Official](https://www.geeksforgeeks.org/dbms/oracle-database-an-introduction/)

- [GeeksForGeeks Tutorials](https://www.geeksforgeeks.org/dbms/oracle-database-an-introduction/)




# Author

#### *"Made with ❤ By Md Ahmod Akram Choudhury"*

- [mdakram2001](https://github.com/mdakram2001)

