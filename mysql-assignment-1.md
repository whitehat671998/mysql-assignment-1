CREATE TABLE :
create table SalesPeople (Snum int 
NOT NULL,Sname varchar(255) NOT NULL UNIQUE,City varchar(35) NOT NULL,Comm int NOT NULL,PRIMARY KEY (Snum) ); 

INSERT QUERY :
mysql> insert into SalesPeople values (1001,'Peel','London',12);
Query OK, 1 row affected (0.01 sec)

mysql> insert into SalesPeople values (1002,'serres','sanjose',13);
Query OK, 1 row affected (0.01 sec)

mysql> insert into SalesPeople values (1004,'Motika','London',11);
Query OK, 1 row affected (0.01 sec)

mysql> insert into SalesPeople values (1007,'Rifkin','Barceiona',15);
Query OK, 1 row affected (0.01 sec)

mysql> insert into SalesPeople values (1003,'Axeicod','Newyork',10);
Query OK, 1 row affected (0.01 sec)

mysql> select * from SalesPeople;
+------+---------+-----------+------+
| Snum | Sname   | City      | Comm |
+------+---------+-----------+------+
| 1001 | Peel    | London    |   12 |
| 1002 | serres  | sanjose   |   13 |
| 1003 | Axeicod | Newyork   |   10 |
| 1004 | Motika  | London    |   11 |
| 1007 | Rifkin  | Barceiona |   15 |
+------+---------+-----------+------+
5 rows in set (0.00 sec)

mysql> create table Customers (Cnum int,
    -> Cname varchar(255),
    -> City varchar(35),
    -> Snum int,
    -> PRIMARY KEY (Cnum),
    -> FOREIGN KEY (Snum) REFERENCES SalesPeople(Snum)
    -> );
Query OK, 0 rows affected (0.12 sec)

mysql> show columns from Customers;
+-------+--------------+------+-----+---------+-------+
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| Cnum  | int          | NO   | PRI | NULL    |       |
| Cname | varchar(255) | YES  |     | NULL    |       |
| City  | varchar(35)  | YES  |     | NULL    |       |
| Snum  | int          | YES  | MUL | NULL    |       |
+-------+--------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
mysql> insert into Customers values (2001,'Hoffman','London',1001);
Query OK, 1 row affected (0.01 sec)

mysql> select * from customers;
+------+----------+---------+------+
| Cnum | Cname    | City    | Snum |
+------+----------+---------+------+
| 2001 | Hoffman  | London  | 1001 |
| 2002 | Gioanni  | Rome    | 1003 |
| 2003 | Liu      | Sanjose | 1002 |
| 2004 | Grass    | Brelin  | 1002 |
| 2006 | Ciemens  | London  | 1001 |
| 2007 | Pereira  | Rome    | 1004 |
| 2008 | Cisneros | Sanjose | 1007 |
+------+----------+---------+------+
7 rows in set (0.00 sec)

mysql> create table Orders (Onum int,
    -> Amt float,
    -> Odate date,
    -> Cnum int,
    -> Snum int,
    -> PRIMARY KEY(Onum),
    -> FOREIGN KEY(Cnum) REFERENCES Customers(Cnum),
    -> FOREIGN KEY(Snum) REFERENCES SalesPeople(Snum)
    -> );^C


mysql> insert into Orders values (3001,18.69,'1990-10-03',2008,1007);


1. Count the number of Salesperson whose name begin with ‘a’/’A’.

mysql> select * from SalesPeople where Sname like 'a%';
+------+---------+---------+------+
| Snum | Sname   | City    | Comm |
+------+---------+---------+------+
| 1003 | Axeicod | Newyork |   10 |
+------+---------+---------+------+
1 row in set (0.02 sec)


2.
mysql> select SalesPeople.*,sum(Orders.Amt) as Amount from SalesPeople INNER JOIN Orders ON SalesPeople.Snum = Orders.Snum group by Orders.Snum having Amount > 2000;
+------+--------+---------+------+--------------------+
| Snum | Sname  | City    | Comm | Amount             |
+------+--------+---------+------+--------------------+
| 1001 | Peel   | London  |   12 | 14932.069885253906 |
| 1002 | serres | sanjose |   13 |  6546.150146484375 |
+------+--------+---------+------+--------------------+
2 rows in set (0.10 sec)



3.  Count the number of Salesperson belonging to Newyork.

mysql> select count(*) from SalesPeople where City='Newyork';
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.01 sec)



4. Display the number of Salespeople belonging to London and belonging to Paris

mysql> select City,count(*) as No_of_SalesPeople from SalesPeople group by City having City in ('London','Paris');
+--------+-------------------+
| City   | No_of_SalesPeople |
+--------+-------------------+
| London |                 2 |
+--------+-------------------+
1 row in set (0.00 sec)

5.
mysql> select Snum,Odate,count(*) as count_by_snum_odate from Orders group by Odate,Snum order by Snum;
+------+------------+---------------------+
| Snum | Odate      | count_by_snum_odate |
+------+------------+---------------------+
| 1001 | 1990-10-03 |                   1 |
| 1001 | 1990-10-05 |                   1 |
| 1001 | 1990-10-06 |                   1 |
| 1002 | 1990-10-03 |                   1 |
| 1002 | 1990-10-04 |                   1 |
| 1002 | 1990-10-06 |                   1 |
| 1003 | 1990-10-04 |                   1 |
| 1004 | 1990-10-03 |                   1 |
| 1007 | 1990-10-03 |                   2 |
+------+------------+---------------------+
9 rows in set (0.00 sec)

