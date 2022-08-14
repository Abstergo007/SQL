
# Sql Queries

- Write a SQL query to fetch all the duplicate records from a table.

  * Tables Structure:
\
drop table users;\
create table users\
(\
user_id int primary key,\
user_name varchar(30) not null,\
email varchar(50));\
\
insert into users values\
(1, 'Sumit', 'sumit@gmail.com'),\
(2, 'Reshma', 'reshma@gmail.com'),\
(3, 'Farhana', 'farhana@gmail.com'),\
(4, 'Robin', 'robin@gmail.com'),\
(5, 'Robin', 'robin@gmail.com');\
\
select * from users;\
\
  * Solution 1:
\
Replace ctid with rowid for Oracle, MySQL and Microsoft SQLServer\
select *\
from users u\
where u.ctid not in (\
select min(ctid) as ctid
from users\
group by user_name\
order by ctid);\


  * Solution 2: Using window function.\
\
select user_id, user_name, email\
from (\
select *,\
row_number() over (partition by user_name order by user_id) as rn\
from users u\
order by user_id) x\
where x.rn <> 1;\




* Write a SQL query to fetch the second last record from a employee table.

  * Tables Structure:\
\
drop table employee;\
create table employee\
( emp_ID int primary key\
, emp_NAME varchar(50) not null\
, DEPT_NAME varchar(50)\
, SALARY int);\
\
insert into employee values(101, 'Mohan', 'Admin', 4000);\
insert into employee values(102, 'Rajkumar', 'HR', 3000);\
insert into employee values(103, 'Akbar', 'IT', 4000);\
insert into employee values(104, 'Dorvin', 'Finance', 6500);\
insert into employee values(105, 'Rohit', 'HR', 3000);\
insert into employee values(106, 'Rajesh',  'Finance', 5000);\
insert into employee values(107, 'Preet', 'HR', 7000);\
insert into employee values(108, 'Maryam', 'Admin', 4000);\
insert into employee values(109, 'Sanjay', 'IT', 6500);\
insert into employee values(110, 'Vasudha', 'IT', 7000);\
insert into employee values(111, 'Melinda', 'IT', 8000);\
insert into employee values(112, 'Komal', 'IT', 10000);\
insert into employee values(113, 'Gautham', 'Admin', 2000);\
insert into employee values(114, 'Manisha', 'HR', 3000);\
insert into employee values(115, 'Chandni', 'IT', 4500);\
insert into employee values(116, 'Satya', 'Finance', 6500);\
insert into employee values(117, 'Adarsh', 'HR', 3500);\
insert into employee values(118, 'Tejaswi', 'Finance', 5500);\
insert into employee values(119, 'Cory', 'HR', 8000);\
insert into employee values(120, 'Monica', 'Admin', 5000);\
insert into employee values(121, 'Rosalin', 'IT', 6000);\
insert into employee values(122, 'Ibrahim', 'IT', 8000);\
insert into employee values(123, 'Vikram', 'IT', 8000);\
insert into employee values(124, 'Dheeraj', 'IT', 11000);\
\
select * from employee;\
\
  * Solution:\
select emp_id, emp_name, dept_name, salary\
from (\
select *,\
row_number() over (order by emp_id desc) as rn\
from employee e) x\
where x.rn = 2;\

* \Write a SQL query to display only the details of employees who either earn the highest salary
or the lowest salary in each department from the employee table.

  * Tables Structure:
\
drop table employee;\
create table employee\
( emp_ID int primary key\
, emp_NAME varchar(50) not null\
, DEPT_NAME varchar(50)\
, SALARY int);\
\
insert into employee values(101, 'Mohan', 'Admin', 4000);\\
insert into employee values(102, 'Rajkumar', 'HR', 3000);\\
insert into employee values(103, 'Akbar', 'IT', 4000);\\
insert into employee values(104, 'Dorvin', 'Finance', 6500);\\
insert into employee values(105, 'Rohit', 'HR', 3000);\\
insert into employee values(106, 'Rajesh',  'Finance', 5000);\\
insert into employee values(107, 'Preet', 'HR', 7000);\\
insert into employee values(108, 'Maryam', 'Admin', 4000);\\
insert into employee values(109, 'Sanjay', 'IT', 6500);\\
insert into employee values(110, 'Vasudha', 'IT', 7000);\\
insert into employee values(111, 'Melinda', 'IT', 8000);\\
insert into employee values(112, 'Komal', 'IT', 10000);\\
insert into employee values(113, 'Gautham', 'Admin', 2000);\\
insert into employee values(114, 'Manisha', 'HR', 3000);\\
insert into employee values(115, 'Chandni', 'IT', 4500);\\
insert into employee values(116, 'Satya', 'Finance', 6500);\\
insert into employee values(117, 'Adarsh', 'HR', 3500);\\
insert into employee values(118, 'Tejaswi', 'Finance', 5500);\\
insert into employee values(119, 'Cory', 'HR', 8000);\
insert into employee values(120, 'Monica', 'Admin', 5000);\
insert into employee values(121, 'Rosalin', 'IT', 6000);\
insert into employee values(122, 'Ibrahim', 'IT', 8000);\
insert into employee values(123, 'Vikram', 'IT', 8000);\
insert into employee values(124, 'Dheeraj', 'IT', 11000);\
\
select * from employee;\
\
  * Solution:
select x.*\
from employee e\
join (select *,\
max(salary) over (partition by dept_name) as max_salary,\
min(salary) over (partition by dept_name) as min_salary\
from employee) x\
on e.emp_id = x.emp_id\
and (e.salary = x.max_salary or e.salary = x.min_salary)\
order by x.dept_name, x.salary;\

