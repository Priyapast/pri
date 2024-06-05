CREATE TABLE salesman(
    salesman_id int primary key,
    name char(20),
     city char(20),
    commission int);

CREATE TABLE customer(
     customer_id int primary key,
     cust_name char(20),
     city char(20),
     grade int,
     salesman_id int,
     FOREIGN KEY (salesman_id) REFERENCES salesman(salesman_id) ON DELETE CASCADE ON UPDATE CASCADE);

CREATE TABLE orders (
     ord_no int primary key,
     purchase_amt int,
     ord_date date,
     customer_id int,
     salesman_id int,
     FOREIGN KEY (customer_id) REFERENCES customer(customer_id) ON DELETE CASCADE ON UPDATE CASCADE,
     FOREIGN KEY (salesman_id) REFERENCES salesman(salesman_id) ON DELETE CASCADE ON UPDATE CASCADE);

INSERT INTO salesman VALUES (1000,'aditya','bangalore','10');
INSERT INTO salesman VALUES (1001,'ben','chennai','20');
INSERT INTO salesman VALUES (1002,'cara','bangalore','30');
INSERT INTO salesman VALUES (1003,'dave','bangalore','40');
INSERT INTO salesman VALUES (1004,'evelyn','mumbai','50');

INSERT INTO customer VALUES (2000,'frank','bangalore',15,1000);
INSERT INTO customer VALUES (2001,'grace','bangalore',25,1002);
INSERT INTO customer VALUES (2002,'hank','chennai',35,1001);
INSERT INTO customer VALUES (2003,'ivan','chennai',45,1001);

INSERT INTO orders VALUES (3000,1500,'2024-01-01',2000,1000);
INSERT INTO orders VALUES (3001,2500,'2024-02-02',2001,1002);
INSERT INTO orders VALUES (3002,3500,'2024-03-03',2002,1001);
INSERT INTO orders VALUES (3003,4500,'2024-04-04',2003,1001);

SELECT count(customer_id) FROM customer WHERE grade > (SELECT avg(grade) FROM customer where city = 'bangalore');

select s.name,s.salesman_id,count(*) as customer_count from salesman s inner join customer c on s.salesman_id=c.salesman_id group by s.name,s.salesman_id having count(*)>1;

 (select 'salesman with customer' as status,s.salesman_id,s.name,s.city from salesman s  inner join customer c on s.salesman_id=c.salesman_id)
    union
  (select 'salesman with customer' as status,s.salesman_id,s.name,s.city from salesman s  left join customer c on s.salesman_id=c.salesman_id where c.customer_id is null);

SELECT salesman.salesman_id,salesman.name FROM salesman JOIN orders ON salesman.salesman_id = orders.salesman_id ORDER BY orders.purchase_amt DESC LIMIT 1;

DELETE FROM salesman WHERE salesman_id = 1000;

