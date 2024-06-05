CREATE TABLE flight (
    no INT PRIMARY KEY,
    frm VARCHAR(100),
    too VARCHAR(100),
    distance INT,
    departs TIME,
    arrives TIME,
    price DECIMAL(10, 2)
);

create table aircraft(
     aid INT,
     aname VARCHAR(20),
     cruisingrange INT,
     PRIMARY KEY(aid));

 create table employees(
     eid INT,
     ename VARCHAR(20),
     salary INT,
     PRIMARY KEY(eid));
    
 create table certified(
     eid INT,
     aid INT,
     PRIMARY KEY(eid,aid),
     FOREIGN KEY(eid) REFERENCES employees(eid),
     FOREIGN KEY(aid) REFERENCES aircraft(aid));

INSERT INTO flight (no,frm,too,distance,departs,arrives,price) VALUES 
            (1,'Bangalore','Mangalore',360,'10:45:00','12:00:00',10000),
            (2,'Bangalore','Delhi',5000,'12:15:00','04:30:00',25000),
            (3,'Bangalore','Mumbai',3500,'02:15:00','05:25:00',30000),
            (4,'Delhi','Mumbai',4500,'10:15:00','12:05:00',35000),
            (5,'Delhi','Frankfurt',18000,'07:15:00','05:30:00',90000),
            (6,'Bangalore','Frankfurt',19500,'10:00:00','07:45:00',95000),
            (7,'Bangalore','Frankfurt',17000,'12:00:00','06:30:00',99000);

INSERT INTO aircraft (aid,aname,cruisingrange) values 
        (123,'Airbus',1000),
        (302,'Boeing',5000),
        (306,'Jet01',5000),
        (378,'Airbus380',8000),
        (456,'Aircraft',500),
        (789,'Aircraft02',800),
        (951,'Aircraft03',1000);
        
INSERT INTO employees (eid,ename,salary) VALUES
        (1,'Ajay',30000),
        (2,'Ajith',85000),
        (3,'Arnab',50000),
        (4,'Harry',45000),
        (5,'Ron',90000),
        (6,'Josh',75000),
        (7,'Ram',100000);

INSERT INTO certified (eid,aid) VALUES
        (1,123),
        (2,123),
        (1,302),
        (5,302),
        (7,302),
        (1,306),
        (2,306),
        (1,378),
        (2,378),
        (4,378),
        (6,456),
        (3,456),
        (5,789),
        (6,789),
        (3,951),
        (1,951),
        (1,789);
        
1.Find the eids of pilots certified for some Boeing aircraft.
     SELECT distinct e.eid
     FROM employees e,aircraft a,certified c
     WHERE e.eid=c.eid
     AND c.aid=a.aid
     AND a.aname='Boeing';

2. Find the names of pilots certified for some Boeing aircraft.
    SELECT distinct e.ename
     FROM employees e,aircraft a,certified c
     WHERE e.eid=c.eid
     AND c.aid=a.aid
     AND a.aname='Boeing';


3. Find the aids of all aircraft that can be used on non-stop flights from Bonn
to Madras.
 SELECT A.aid FROM Aircraft A, Flight F WHERE F.frm = 'Bonn' AND F.too = 'Madras' AND A.cruisingrange > F.distance;

4. Identify the flights that can be piloted by every pilot whose salary is more
than $100,000.
 SELECT    DISTINCT  F.frm, F.too FROM  Flight F WHERE NOT EXISTS  ( SELECT  * FROM  Employees E WHERE  E.salary > 100000 AND NOT EXISTS (SELECT  * FROM  Aircraft A, Certified C WHERE  A.cruisingrange > F.distance AND  E.eid = C.eid
AND A.aid = C.aid) );


5. Find the names of pilots who can operate planes with a range greater than 3,000 miles but are not certified on any Boeing aircraft.
	SELECT E.ename FROM Certified C, Employees E, Aircraft A WHERE A.aid = C.aid AND E.eid = C.eid AND A.cruisingrange > 3000 AND E.eid NOT IN ( SELECT C2.eid FROM Certified C2, Aircraft A2 WHERE C2.aid = A2.aid AND A2.aname = 'Boeing' );
