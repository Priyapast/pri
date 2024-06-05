 CREATE TABLE student(
     snum int primary key,
     sname char(20),
     major char(20),
     level char(20),
     age int);

 CREATE TABLE class (
     cname char(20) primary key,
     meets_at time,
     room char(20),
     fid int);

 CREATE TABLE enrolled (
     snum int,
     cname char(20),
     primary key(snum,cname));

 CREATE TABLE faculty(
     fid int primary key,
     fname char(20),
    deptid int);

INSERT INTO student (snum, sname, major, level, age) VALUES
(1001, 'aditya', 'ise', 'jr', 21),
(1002, 'ben', 'ise', 'jr', 22),
(1003, 'connor', 'ise', 'sr', 23),
(1004, 'davey', 'cse', 'jr', 24),
(1005, 'evelyn', 'cse', 'jr', 25),
(1006, 'franklin', 'cse', 'sr', 26),
(1007, 'geralt', 'history', 'jr', 27),
(1008, 'hank', 'history', 'sr', 28);

INSERT INTO class (cname, meets_at, room, fid) VALUES
('ancient_history', '12:00:00', 'r124', 2004),
('c', '12:00:00', 'r123', 2002),
('java', '10:00:00', 'r122', 2001),
('js', '13:00:00', 'r125', 2002),
('javascript', '09:00:00', 'r128', 2003),
('python', '09:00:00', 'r121', 2001);

INSERT INTO enrolled (snum, cname) VALUES
(1001, 'javascript'),
(1001, 'python'),
(1003, 'java'),
(1004, 'c'),
(1006, 'java'),
(1007, 'ancient_history'),
(1007, 'c');


INSERT INTO faculty (fid, fname, deptid) VALUES
(2001, 'ivan', 3001),
(2002, 'jason', 3002),
(2003, 'kirby', 3003),
(2004, 'leon', 3001);




q1) 1. Find the names of all juniors (Level = JR) who are enrolled in a class taught
by I. Teacher.

 SELECT DISTINCT s.sname FROM enrolled e JOIN class c ON e.cname = c.cname JOIN student s ON e.snum = s.snum WHERE (e.cname != (SELECT cname FROM class GROUP BY cname HAVING count(*) != 1) AND s.level = 'jr' );


2. Find the age of the oldest student who is either a History major or is
enrolled in a course taught by I. Teacher.

  SELECT age FROM enrolled e JOIN class c ON e.cname = c.cname RIGHT JOIN student s ON e.snum = s.snum WHERE(e.cname != (SELECT cname FROM class GROUP BY cname HAVING count(*) != 1) OR s.major = 'history') ORDER BY s.age DESC LIMIT 1;


3. Find the names of all classes that either meet in room R128 or have five or
more students enrolled.

 SELECT cname FROM class WHERE room = 'r128'
     UNION
     SELECT cname FROM enrolled GROUP BY cname HAVING count(*) > 5;


4. Find the names of all students who are enrolled in two classes that meet at
the same time

 SELECT sname FROM enrolled e JOIN student s ON e.snum = s.snum JOIN class c ON e.cname = c.cname GROUP BY sname HAVING count(*) != count(distinct meets_at);


5. Find the names of faculty members who teach in every room in which
some class is taught.
	SELECT fid,count(DISTINCT room) FROM class GROUP by fid HAVING count(DISTINCT room ) > (SELECT COUNT(DISTINCT room) FROM class);



6. Find the names of faculty members for whom the combined enrollment of
the courses that they teach is less than five.

 SELECT f.fname,count(*) FROM class c JOIN enrolled e ON c.cname = e.cname JOIN faculty f ON c.fid = f.fid GROUP BY c.fid HAVING count(*) < 5 ;

7. Print the Level and the average age of students for that Level, for each
Level.

 SELECT level, avg(age) FROM student GROUP BY level;

8. Print the Level and the average age of students for that Level, for all Levels
except JR.
 SELECT level, avg(age) FROM student GROUP BY level HAVING level != 'jr' ;


9. Find the names of students who are enrolled in the maximum number of
classes.
 SELECT s.sname FROM student s JOIN enrolled e ON s.snum = e.snum GROUP BY e.snum HAVING count(*) = (SELECT count(*) FROM enrolled GROUP BY snum ORDER BY count(*) DESC LIMIT 1);


10. Find the names of students who are not enrolled in any class.
 SELECT s.sname FROM student s LEFT JOIN enrolled e ON s.snum = e.snum WHERE e.snum IS NULL;










