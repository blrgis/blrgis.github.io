# Loading data from csv file into postgresql using python

For any kind of applications, one of the major requirements is to store, modify and access data for viewing, analysing and finding insights from it. We are using PostgreSQL as it is an open source, highly stable and can be easily integrated with cloud too. We use **psycopg2** module of python for the connection with database. 

Sample data we will be using for this example:

stu_id | stu_firstname | stu_lastname | department
------------ | -------------  | ------------- | -------------
189056 | John | Dodge | CSE
189057 | Sarah | Smith | ECE
189058 | George | Dapper | EEE
189059 | Jack | Dodge | ME
189060 | Jessica | Jones | CSE
189061 | Dragon | Davich | ECE
189062 | Steve | Curio | EEE
189063 | aura | Black | IT
189064 | Wilhelm | Blake | IT

**connect** method is used for establishing connection with database. The parameters are: 

**‘user’**: the owner of database; 

**‘password’**: password to access the database;

**‘host’**: host where Postgres server is currently running; localhost if local server is used, else hostname of remote server;

**‘port’**: port number of database;

**‘database’**: database name in which data has to be stored;

1. Username is important as there are multiple users who are given different kinds of access to the database. This is done to increase security levels of data. Similarly multiple databases can be used to increase data security and division. 

```
connection = psycopg2.connect(user = "username", password = "password", host = "hostname", port = "port_number", database = "database")
```

2. Cursor objects are created to execute operations on the database. Execute method is used for executing commands. **cursor** method creates an object for handling operations on the database.

```
cur=connection.cursor()
```

3. Query for creating a table. This table has four columns stu_id, stu_firstname, stu_lastname, and department

```
create_query = """CREATE TABLE student(stu_id integer PRIMARY KEY, stu_firstname text, stu_lastname text, department text)"""   
```

4. Query for inserting values into database. 

```
insert_query = """INSERT INTO student VALUES (%s, %s, %s, %s)", (189076, 'Alice', 'Bob', 'IT')""" 
cur.execute(insert_query) 
```

5. To read a csv file use open method. ‘r’ denotes read mode. Reader method creates an object to iterate through the file. Next method is used to skip the header row. This method needs to use csv module to read file contents.

```
with open(‘student_details.csv', 'r') as filename:
  fileReader = csv.reader(filename)
  next(fileReader)
```

6. For iterating through the file contents and inserting it into database.

```
for row in fileReader:
  cur.execute("""INSERT INTO student VALUES (%s, %s, %s, %s)""", row)
```

7. More optimized insert would be to use copy_from method for loading data without looping over INSERT command. The data is loaded without execute command as well. copy_from directly loads data 

```
with open('user_accounts.csv', 'r') as f:
   next(f) 
   cur.copy_from(f, 'users', sep=',')
connection.commit()
```

8. Query for selecting all values of the table. **fetchall** method retrieves all records 

```
select_query = """select * from STUDENT"""
cur.execute(select_query)
res_allrecord = cur.fetchall()
```

9. Query for selecting values of the table based on stu_id. fetchone method retrieves the record based on given criteria

```
select_query = """select * from STUDENT where stu_id = %s"""
cur.execute(select_query, ([stu_id]))
res_record = cur.fetchone()
```

10. Query for updating value based on stu_id

```
update_query = """Update STUDENT set department = %s where stu_id = %s"""    
cur.execute(update_query, ([department, stu_id] ))
```

11.Query for deleting value based on stu_id

```
delete_query = """Delete from STUDENT where stu_id = %s"""
cur.execute(delete_query, ([stu_id,] ))
```

12. Close the cursor and connection

```
cur.close()
connection.close()
```

**Complete python code**

```
import csv
import psycopg2
try:
    connection = (user = "username", password = "password", host = "hostname", port = "port_number", database = "database")    

    cur=connection.cursor()


    create_query = """CREATE TABLE student(stu_id integer PRIMARY KEY, stu_firstname text, stu_lastname text, department text)"""    

    cur.execute(create_query)

    with open(‘student_details.csv', 'r') as filename:
        fileReader = csv.reader(filename)
        next(fileReader) # Skip the header row.
        for row in fileReader:
            cur.execute("""INSERT INTO student VALUES (%s, %s, %s, %s)""", row)
    
    stu_id = 189065
    department="ME"
    
    select_query = """select * from STUDENT"""
    cur.execute(select_query)
    res_allrecord = cur.fetchall()
    row_count = cur.rowcount
    print(row_count, " record/s fetched successfully!!")
    
    print("Before updating record ")
    select_query = """select * from STUDENT where stu_id = %s"""
    cur.execute(select_query, ([stu_id]))
    res_record = cur.fetchone()
    print(res_record)
    
    update_query = """Update STUDENT set department = %s where stu_id = %s"""
    cur.execute(update_query, ([department, stu_id]))
    connection.commit()
    row_count = cur.rowcount
    print(row_count, " record/s updated successfully ")
   
    print("After updating record ")
    select_query = """select * from STUDENT where stu_id = %s"""
    cur.execute(select_query, ([stu_id]))
    res_record = cur.fetchone()
    print(res_record)
    
    delete_query = """Delete from STUDENT where stu_id = %s"""
    cur.execute(delete_query, ([stu_id,] ))
    connection.commit()
    row_count = cur.rowcount
    print(row_count, " record/s deleted successfully!!")
   
    connection.commit()


except (Exception, psycopg2.Error) as error :
    print ("Error connecting to PostgreSQL!!", error)
finally:
    if(connection):
      cur.close()
      connection.close()
      print("Connection is closed successfully!!")
```

