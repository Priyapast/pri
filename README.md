B. Consider the following schema for Order Database:
 SALESMAN(Salesman_id, Name, City, Commission)
 CUSTOMER(Customer_id, Cust_Name, City, Grade, Salesman_id)
 ORDERS(Ord_No, Purchase_Amt, Ord_Date, Customer_id, Salesman_id)
 Write SQL queries to
 1. Count the customers with grades above Bangalore’s average.
 2. Find the name and numbers of all salesmen who had more than one customer.
 3. List all salesmen and indicate those who have and don’t have customers in their cities (Use
 UNION
 operation.)
 4. Create a view that finds the salesman who has the customer with the highest order of a day.
 5. Demonstrate the DELETE operation by removing salesman with id 1000. All his orders must also
 CREATE TABLE SALESMAN
 (SALESMAN_ID NUMBER (4),
 NAMEVARCHAR2(20),
 CITY VARCHAR2(20),
 COMMISSION VARCHAR2(20),
 PRIMARY KEY(SALESMAN_ID));
 CREATE TABLE CUSTOMER1
 (CUSTOMER_ID NUMBER (4),
 CUST_NAMEVARCHAR2(20),
 CITY VARCHAR2(20),
 GRADE NUMBER(3),
 PRIMARY KEY(CUSTOMER_ID),
 SALESMAN_ID REFERENCES SALESMAN(SALESMAN_ID) ONDELETE SET NULL);
 CREATE TABLE ORDERS
 (ORD_NO NUMBER(5),
 PURCHASE_AMT NUMBER(10, 2),
 ORD_DATE DATE,
 PRIMARY KEY(ORD_NO),
 CUSTOMER_ID REFERENCES CUSTOMER1(CUSTOMER_ID) ONDELETE CASCADE,
 SALESMAN_ID REFERENCES SALESMAN(SALESMAN_ID) ONDELETE CASCADE);
 INSERT INTO SALESMANVALUES(1000, 'JOHN','BANGALORE','25 %');
 INSERT INTO SALESMANVALUES(2000, 'RAVI','BANGALORE','20 %');
 INSERT INTO SALESMANVALUES(3000, 'KUMAR','MYSORE','15 %');
 INSERT INTO SALESMANVALUES(4000, 'SMITH','DELHI','30 %');
 INSERT INTO SALESMANVALUES(5000, 'HARSHA','HYDRABAD','15 %');
 INSERT INTO CUSTOMER1VALUES(10, 'PREETHI','BANGALORE', 100, 1000);
 INSERT INTO CUSTOMER1VALUES(11, 'VIVEK','MANGALORE', 300, 1000);
 INSERT INTO CUSTOMER1VALUES(12, 'BHASKAR','CHENNAI', 400, 2000);
 INSERT INTO CUSTOMER1VALUES(13, 'CHETHAN','BANGALORE', 200, 2000);
 INSERT INTO CUSTOMER1VALUES(14, 'MAMATHA','BANGALORE', 400, 3000);
INSERT INTO ORDERS VALUES(50, 5000, '04-MAY-17', 10, 1000);
 INSERT INTO ORDERS VALUES(51, 450, '20-JAN-17', 10, 2000);
 INSERT INTO ORDERS VALUES(52, 1000, '24-FEB-17', 13, 2000);
 INSERT INTO ORDERS VALUES(53, 3500, '13-APR-17', 14, 3000);
 INSERT INTO ORDERS VALUES(54, 550, '09-MAR-17', 12, 2000);
 1. Count the customers with grades above Bangalore’s average.
 SELECT GRADE, COUNT (DISTINCT CUSTOMER_ID)
 FROMCUSTOMER1
 GROUPBYGRADE
 HAVING GRADE>(SELECT AVG(GRADE)
 FROMCUSTOMER1
 WHERECITY='BANGALORE');
 2. Find the name and numbers of all salesmen who had more than one customer.
 SELECT SALESMAN_ID, NAME
 FROMSALESMANA
 WHERE1<(SELECTCOUNT(*)
 FROMCUSTOMER1
 WHERESALESMAN_ID=A.SALESMAN_ID);
 3. List all salesmen and indicate those who have and don’t have customers in their cities (Use
 UNION
 operation.)
 SELECT SALESMAN.SALESMAN_ID, NAME, CUST_NAME, COMMISSION
 FROMSALESMAN,CUSTOMER1
 WHERESALESMAN.CITY = CUSTOMER1.CITY
 UNION
 SELECT SALESMAN_ID, NAME, 'NO MATCH', COMMISSION
 FROMSALESMAN
 WHERENOTCITY=ANY
 (SELECT CITY
 FROMCUSTOMER1)
 ORDER BY2DESC;
 4. Create a view that finds the salesman who has the customer with the highest order of a day.
 CREATE VIEW ELITSALESMAN AS
 SELECT B.ORD_DATE, A.SALESMAN_ID, A.NAME
 FROMSALESMANA,ORDERSB
 WHEREA.SALESMAN_ID = B.SALESMAN_ID
 ANDB.PURCHASE_AMT=(SELECT MAX (PURCHASE_AMT)
 FROMORDERSC
 WHEREC.ORD_DATE =B.ORD_DATE);
 5. Demonstrate the DELETE operation by removing salesman with id 1000. All his orders must also
 be
 deleted.
 Use ONDELETECASCADEattheendof foreign key definitions while creating child table orders and
 then execute the following:
Use ONDELETESETNULLattheendof foreign key definitions while creating child table
 customers and then executes the following:
 DELETE FROMSALESMAN
 WHERESALESMAN_ID=1000
