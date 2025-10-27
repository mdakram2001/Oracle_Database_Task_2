
# Oracle Database (Step By Step):

Here you can find solution of Week-10: Q.no: 08



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
  CREATE USER {banking_user} IDENTIFIED BY {password};
```
```bash
  GRANT CONNECT, RESOURCE TO {banking_user};
```
```bash
  CONNECT {banking_user}
  Enter password: {password}
```

##### *You can also use the system database and create tables there but it will contains some predefined tables, constraints etc*


##  Creating TABLES:

##### *This command will help you create new tables*

```bash
CREATE TABLE BRANCH(branch_name VARCHAR2(20) PRIMARY KEY, branch_city VARCHAR2(20), assets REAL);
```
```bash
CREATE TABLE ACCOUNT(accno NUMBER(12,0) PRIMARY KEY, branch_name VARCHAR2(20), balance REAL);
```
```bash
CREATE TABLE DEPOSITOR(customer_name VARCHAR2(20), accno NUMBER(12,0));
```
```bash
CREATE TABLE CUSTOMER(customer_name VARCHAR2(20),customer_street VARCHAR2(20), customer_city VARCHAR2(20));
```
```bash
CREATE TABLE LOAN(loan_number NUMBER(12,0) PRIMARY KEY, branch_name VARCHAR2(20), amount REAL);
```
```bash
CREATE TABLE BORROWER(customer_name VARCHAR2(20),loan_number NUMBER(12,0));
```

##### *You can also define foreign keys(constraints)/primarys here, But I will add some of Primary keys & foreign keys using 'ALTER' command.*


##  Adding FOREIGN KEYS in remaining Tables:

##### *This commands will help you alter tables constraints like primary keys & foreign keys.*

```bash
ALTER TABLE ACCOUNT ADD CONSTRAINT fk_branch_name1 FOREIGN KEY (branch_name) REFERENCES BRANCH (branch_name);
```
```bash
ALTER TABLE DEPOSITOR ADD CONSTRAINT fk_accno FOREIGN KEY (accno) REFERENCES ACCOUNT (accno);
```
```bash
ALTER TABLE DEPOSITOR  ADD CONSTRAINT fk_customer_name FOREIGN KEY(customer_name) REFERENCES CUSTOMER (customer_name);
```
```bash
ALTER TABLE LOAN ADD CONSTRAINT fk_branch_name2 FOREIGN KEY (branch_name) REFERENCES BRANCH (branch_name);
```
```bash
ALTER TABLE BORROWER ADD CONSTRAINT fk_customer_name FOREIGN KEY (customer_name) REFERENCES CUSTOMER (customer_name);
```
```bash
ALTER TABLE BORROWER ADD CONSTRAINT fk_loan_number FOREIGN KEY (loan_number) REFERENCES LOAN (loan_number);
```


##  Adding PRIMARY KEY with Two or More Columns:

##### *This commands will help you add primary keys on the remaining tables with two or more columns.* 
#### *"To make each row UNIQUE"*

```bash
ALTER TABLE DEPOSITOR ADD CONSTRAINT pk_depositor PRIMARY KEY (customer_name, accno);
```
```bash
ALTER TABLE BORROWER ADD CONSTRAINT pk_borrower PRIMARY KEY (customer_name, loan_number);

```

##  Setting Constraints 'ON DELETE':

##### *This commands is important because it will decide what to do if we DELETE record from a table(parent) whose column is a foreign key of another table (important for solving last question).* 
#### *"SET NULL ---> will set the value null."*
#### *"CASCADE ---> will delete the value corresponding to foreign key."*

```bash
ALTER TABLE ACCOUNT 
ADD CONSTRAINT fk_account_branch 
FOREIGN KEY (branch_name) 
REFERENCES BRANCH (branch_name)
ON DELETE SET NULL;
```

##  Inserting Data in Tables:

##### *This commands will INSERT data in the pre-defined tables.* 

#### *"INSERT one by one"* 

```bash
INSERT INTO BRANCH VALUES('Downtown', 'New York', 9000000);
```
```bash
INSERT INTO CUSTOMER VALUES('John', 'Main_St', 'New York');
```
```bash
INSERT INTO ACCOUNT VALUES(101, 'Downtown', 7000);
```
```bash
INSERT INTO DEPOSITOR VALUES('John', 101);
```
```bash
INSERT INTO LOAN VALUES(5001, 'Downtown', 10000);
```
```bash
INSERT INTO BORROWER VALUES('John', 5001);
```


#### *"INSERT multiple rows at once"* 

```bash
INSERT ALL
INTO BRANCH VALUES('Central', 'Boston', 4500000)
INTO BRANCH VALUES('Midtown', 'Houston', 3800000)
SELECT * FROM dual;
```

##  Find all the customers who have atleast two accounts at the main branch:

```bash
SELECT d.customer_name
FROM DEPOSITOR d
JOIN ACCOUNT a ON d.accno = a.accno
WHERE a.branch_name = 'Downtown'
GROUP BY d.customer_name
HAVING COUNT(d.accno) >= 2;

```

#### *"Oracle Queries are not case sensitive"* 

```bash
select d.customer_name
from depositor d
join account a on d.accno = a.accno
where a.branch_name = 'Downtown'
group by d.customer_name
having count(d.accno) >= 2;

```

##  Find all customers who have an account at all branches located in a specific city (for example: 'New York'):

```bash
SELECT d.customer_name
FROM DEPOSITOR d
JOIN ACCOUNT a ON d.accno = a.accno
JOIN BRANCH b ON a.branch_name = b.branch_name
WHERE b.branch_city = 'New York'
GROUP BY d.customer_name
HAVING COUNT(DISTINCT b.branch_name) = (
    SELECT COUNT(branch_name)
    FROM BRANCH
    WHERE branch_city = 'New York'
);

```


## Demonstrate how you delete all ACCOUNT tuples at every branch located in a specific city (for example: 'New York').

##### *If your ACCOUNT table has foreign key constraints with ON DELETE CASCADE,deleting from BRANCH may also automatically remove related ACCOUNT records. But if your constraint uses ON DELETE SET NULL, accounts won’t be deleted — their branch_name will become NULL instead.*

```bash
DELETE FROM ACCOUNT
WHERE branch_name IN (
    SELECT branch_name
    FROM BRANCH
    WHERE branch_city = 'New York'
);

```



## More Tutorials

- [Oracle Official](https://www.geeksforgeeks.org/dbms/oracle-database-an-introduction/)

- [GeeksForGeeks Tutorials](https://www.geeksforgeeks.org/dbms/oracle-database-an-introduction/)




# Author

#### *"Made with ❤ By Md Ahmod Akram Choudhury"*

- [mdakram2001](https://github.com/mdakram2001)

