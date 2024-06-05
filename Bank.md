 CREATE TABLE branch(
     `branch-name` char(20) primary key,
     `branch-city` char(20),
     assets int);

 CREATE TABLE customer(
     `customer-name` char(20) primary key,
     `customer-street` char(20),
     `customer_city` char(20));

 CREATE TABLE account(
     `account-number` int primary key,
     `branch-name` char(20),
     balance int);

 CREATE TABLE loan(
     `loan-number` int primary key,
     `branch-name` char(20),
     amount int);

 CREATE TABLE depositer(
     `customer-name` char(20),
     `account-number` int,
     primary key(`customer-name`,`account-number`));

 CREATE TABLE borrower(
     `customer-name` char(20),
     `loan-number` int,
     primary key(`customer-name`,`loan-number`));

 CREATE TABLE employee(
     `employee-name` char(20),
     `branch-name` char(20),
     salary int,
     primary key(`employee-name`,`branch-name`));



INSERT INTO branch (branch-name, branch-city, assets)
VALUES ('Ahmedabad-bank', 'Ahmedabad', 500),
       ('Brighton-bank', 'Brighton', 2000000),
       ('Chennai-bank', 'Chennai', 5000000);


INSERT INTO customer (`customer-name`, `customer-street`, `customer_city`)
VALUES ('aditya', '001 ahmedabad-street', 'ahmedabad'),
       ('ben', '002 brighton-street', 'brighton'),
       ('connor', '003 brighton-street', 'brighton'),
       ('davey', '004 chennai-street', 'chennai');



INSERT INTO account (`account-number`, `branch-name`, `balance`)
VALUES (101, 'Ahmedabad-bank', 100000),
       (102, 'Brighton-bank', 200000),
       (103, 'Brighton-bank', 700),
       (104, 'Chennai-bank', 300);


INSERT INTO loan (`loan-number`, `branch-name`, `amount`)
VALUES (201, 'Ahmedabad-bank', 100000),
       (202, 'Brighton-bank', 200000),
       (203, 'Brighton-bank', 700),
       (204, 'Chennai-bank', 300);


INSERT INTO depositer (`customer-name`, `account-number`)
VALUES ('aditya', 101),
       ('ben', 102),
       ('connor', 103),
       ('davey', 104);


INSERT INTO borrower (`customer-name`, `loan-number`)
VALUES ('aditya', 201),
       ('ben', 202),
       ('connor', 203),
       ('davey', 204);



INSERT INTO employee (`employee-name`, `branch-name`, `salary`)
VALUES ('aditya', 'Ahmedabad-bank', 1000000),
       ('ben', 'Brighton-bank', 20000),
       ('connor', 'Brighton-bank', 30000),
       ('davey', 'Chennai-bank', 40000);



1) find names of all customers
	 SELECT `customer-name` FROM customer;


2) find names of all branches in loan relation, dont display duplicates
	 SELECT DISTINCT `branch-name` FROM loan;

3) display the entire branch table
	SELECT * FROM branch;

4) find the account number of all accounts where the bal is > $700
	SELECT `account-number` FROM account WHERE balance > 700 ;


5) find the acc number and bal for all accounts from Brighton where bal > $800
	SELECT * FROM account WHERE (`branch-name` = 'Brighton-bank' AND balance > 800 );

6) display the branch name and assets from all branches in thousands of $'s and rename the assets to 'assets in thousands'
	SELECT `branch-name`,assets/1000 AS `assets in thousands` FROM branch WHERE assets > 1000 ;


7) find the names of all branches with assets between 1 to 4 million $'s
	SELECT * FROM branch WHERE (assets > 1000000 AND assets < 4000000);


8) find the name, acc num and balance of all customers who have an account
	SELECT d.`customer-name`, d.`account-number`, a.balance FROM depositer d JOIN account a ON d.`account-number` = a.`account-number`;


9) find the name, acc num and balance of all customers who have an account with a balance of $400 or less
	SELECT d.`customer-name`, d.`account-number`, a.balance FROM depositer d JOIN account a ON d.`account-number` = a.`account-number` WHERE a.balance <= 400 ;
