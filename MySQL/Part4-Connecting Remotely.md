# Connecting Remotely via Linux host

Please note: This is a continuation of *MySQL Queries.md*.

### Scope: 
This is a continuation of *MySQL Queries*. I will be attempting to connect to the MySQL server to run some queries from a Linux host, and add a new schemas and tables remotely via the command line. 

### Connecting to MySQL server

When I initially tried to remote into the server, I was getting the error below:

![Pasted image 20240504154914](https://github.com/lm3nitro/Projects/assets/55665256/32d1db03-7a26-4367-bb98-b5fd2e158750)

I ran a packet capture while attempting to see if I could get more information. I was able to capture the following:

![Pasted image 20240504154823](https://github.com/lm3nitro/Projects/assets/55665256/a68c155a-2c04-4b40-aada-ac84eb5f57b6)

I also wanted to see the error in MySQL regarding the remote attempt. You can biew logs by going to *Server > Server Logs*

![Pasted image 20240504160434](https://github.com/lm3nitro/Projects/assets/55665256/e0cc0c8d-6a66-49b6-a10a-276c959bcc63)

Here I can see the error related to my remote host:

![Pasted image 20240504160547](https://github.com/lm3nitro/Projects/assets/55665256/831445a0-1ae8-4067-909a-e43bf5f4f664)

To resolve this issue and connect to MySQL remotely, I will need to create/add a user and set-up a new connection.

### New User and Connection Set-up

To get started, navigate to *Server > Users and Privileges*

![Pasted image 20240504155126](https://github.com/lm3nitro/Projects/assets/55665256/cfadcc8f-8d55-4c19-a91c-b0839c3a9f45)

In the new dialog, select *Add Account* towards the bottom, and enter the needed information regarding the new user. When complete, click *Apply*

![Pasted image 20240504153412](https://github.com/lm3nitro/Projects/assets/55665256/5f52aeab-a5b0-46f2-b940-957df22216a2)

You will then need to select the role/privileges the new user will have. To do this, navigate to *Administrative Roles*. I chose to give my user complete access. Once complete, click *Apply* to save the changes. It is always recommended to apply the principle of least privilege when possible.

![Pasted image 20240504153628](https://github.com/lm3nitro/Projects/assets/55665256/c80e7c92-b9e2-46d5-85ea-0b48247540b5)

Next, I will need to select which schema my user will have access to. Then select *OK*.

![Pasted image 20240504153938](https://github.com/lm3nitro/Projects/assets/55665256/7f5ba7a2-eea3-4998-bbdb-1d89e1b1b001)

Once I clicked OK, I can now select what the user will be able to do within the schema (add, delete, etc.). Click *Apply*

![Pasted image 20240504154139](https://github.com/lm3nitro/Projects/assets/55665256/11597da1-56b8-454d-bb96-e0473a2afed7)

Now that my user has been created, I can set up the connection. To do this, go to the main window for MySQL Workbench, and select *+* icon to add a new connection. Enter the needed information for the remote host and click *OK*

![Pasted image 20240504154437](https://github.com/lm3nitro/Projects/assets/55665256/1dffb7f1-c140-4d0e-a0fb-3ce646f201d8)


### Testing connection

My user has been created, and the connection is set up. I can now resume and attempt to connect remotely again. Here I can see that it was successful and that my user can see the databases:

![Pasted image 20240504155622](https://github.com/lm3nitro/Projects/assets/55665256/b3aa5d07-8006-4b04-92bf-cfd89e003aee)

On the MySQL server, I can also verify and see the remote connection:

![Pasted image 20240504161258](https://github.com/lm3nitro/Projects/assets/55665256/7c68edb3-6ff5-45ab-8387-915549347aab)

I can also see the successful connection via Wireshark:

![Pasted image 20240504161516](https://github.com/lm3nitro/Projects/assets/55665256/632ad97a-1ee3-41e1-a93f-e59a668d3687)

### Creating New Database

To see database options, use the following command

```
show databases;
```
![Pasted image 20240505142318](https://github.com/lm3nitro/Projects/assets/55665256/1655bb03-c800-4ec0-b041-e0e7509bb802)

I do not want to change anything in the existing databases. I will add a new database and change into it in order to use it:

```
create database lm3nitro_food;
use lm3nitro_food;
```
![Pasted image 20240505143256](https://github.com/lm3nitro/Projects/assets/55665256/a7f663bc-9d11-4320-bfe3-04b8634ded9a)

Now that I have my new database, I created a table. The table I created is food_table and the columns are (ID, Name, Color, and Type):

```
Add table to database:

 create table food_table (
    -> id int,
    -> name varchar(300),
    -> color varchar(300),
    -> type varchar(300)
    -> );
```

![Pasted image 20240505143843](https://github.com/lm3nitro/Projects/assets/55665256/88a53fb1-43fc-4c07-b7e7-4c4e549fe067)

I can verify my columns using the following command:

```
describe food_table;
```

![Pasted image 20240505144043](https://github.com/lm3nitro/Projects/assets/55665256/a42a82c5-29d3-4247-9af3-59fa08b7e6c2)

Now I needed to add data into my table:

insert into food_table values (1, "apple", "red", "fruit"),
    -> (2, "banana", "yellow", "fruit"),
    -> (3, "carrot", "orange", "vegetble"),
    -> (4, "broccoli", "green", "vegetable"),
    -> (5, "blueberry", "blue", "fruit"),
    -> (6, "spinach", "green", "fruit"),
    -> (7, "pineapple", "yellow", "fruit"),
    -> (8, "orange", "orange", "fruit"),
    -> (9, "lemon", "yellow", "fruit"),
    -> (10, "lime", "green", "fruit");


Verifying everything was added properly:

```
select * from food_table;
```

![Pasted image 20240505152407](https://github.com/lm3nitro/Projects/assets/55665256/09167fe9-503b-44a9-87f2-ec5f1a17554a)


In the table above, I see that spinach is under the category fruit. Let's delete the line in my table and re-add correctly:

```
delete from food_table where id=6;
select * from food_table;
insert into food_table values (6, "spinach", "green", "vegetable");
```

![Pasted image 20240505162253](https://github.com/lm3nitro/Projects/assets/55665256/c12d0f70-cc86-44c4-83c8-1af06f891da8)


Let's create another table:

```
 create table customer_table (
    -> id int,
    -> first_name varchar (300),
    -> last_name varchar (300),
    -> store varchar (300),
    -> age int,
    -> state varchar (300)
    -> );
```
Let's verify:

```
describe customer_table;
```

I can also verify on the MySQL Workbench:

![Pasted image 20240505165528](https://github.com/lm3nitro/Projects/assets/55665256/2518842a-b7dc-4a7f-8f12-d662ad352749)

Linux
![Pasted image 20240505162951](https://github.com/lm3nitro/Projects/assets/55665256/86e72c7e-c664-4e50-8bd8-fb37af405148)


Information added to table:

```
 insert into customer_table values (1, "sally", "jensen", "target", 30, "CA"),
    -> (2, "bob", "smith", "walmart", 45, "FL"),
    -> (3, "cristina", "lopez", "target", 25, "TX"),
    -> (4, "joe", "jackson", "kroger", 40, "UT"),
    -> (5, "clare", "williams", "target", 20, "NY");
```

![Pasted image 20240505163802](https://github.com/lm3nitro/Projects/assets/55665256/ad5dd890-caed-4003-970c-d4fd7aa1ca8c)


### Queries

Now that I have created a couple of schemas and have added tables and data, it's time to run some queries. 

#### Query #1

See all customers who went to target:

```
select * from customer_table where store= "target";

![Pasted image 20240505164114](https://github.com/lm3nitro/Projects/assets/55665256/3d4a0c47-238d-4362-abc6-60078bada0e4)
```

#### Query #2

See all customers who went to target or kroger:

```
select * from customer_table where store= "target" or store= "kroger";
```

![Pasted image 20240505164405](https://github.com/lm3nitro/Projects/assets/55665256/1ec27297-b21a-4d50-af0b-052a1ecb6c2a)


#### Query #3

See customers under the age of 30:

```
select * from customer_table where age< "30";
```

![Pasted image 20240505164703](https://github.com/lm3nitro/Projects/assets/55665256/df8da6e9-f41f-46f2-9300-75b2deb7d2da)


#### Query #4

To update information in a table I used the *update* command

```
update customer_table set age = 21 where first_name = "clare";
```

![Pasted image 20240505165221](https://github.com/lm3nitro/Projects/assets/55665256/eb899b7d-12a3-48b9-946e-ebc4cb217675)

Summary:

In conclusion, being able to modify, view, and query a database is important because direct access is not always possible, this provides flexibility and convenience. MySQL servers are crucial for data management and analysis. Because MySQL is open source, it makes it so that it can be freely used and modified. MySQL also supports user authentication as seen above, SSL for secure connections, and data encryption. 

By installing MySQL and running queries, I was able to learn how to structure and manipulate data, understand SQL syntax, and gain insights into database design. Connecting remotely allowed me to also learn about database security and network configurations. The key takeaway for me was the practical experience in managing databases, ability to write and optimize SQL queries, and expand my knowledge of security best practices.
