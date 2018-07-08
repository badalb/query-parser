A Simple SQL like query parser
------------------------------
This implementation is a SQL like query parser implemented using JAVA.
The solution has primarily three different parts - 
	a. Data Store
	b. Query Parser
	c. Query Executor

a. Data Store 


Data can be stored in CSV or other format. Currently it supports only CSV format. Data is stored in a directory mentioned in config.properties
file. Data needs to be in two files i. tablename.meta (meta data of the table) ii. tablename.data (data in CSV format). Metadata is required to data with table and column definations.

com.helpshift.databse.storage
com.helpshift.databse.storage.catalog
com.helpshift.databse.storage.catalog.csv
com.helpshift.databse.storage.csv
com.helpshift.databse.storage.result
com.helpshift.databse.storage.util

packages are dealing with data storage.



b. Query Parser

A rule based query parser with subset of supported query statement and conditions are implemented. The query parser segregates tablename, select column/s or all column, where clause and send it to executor to apply those rules. A JavaCC rule based are far more elegant though

com.helpshift.parser
com.helpshift.parser.exception
com.helpshift.parser.impl
com.helpshift.parser.statement
com.helpshift.parser.statement.filter
com.helpshift.parser.statement.select

packages are dealing with Query parsing.

c. Query Executor

Query executors takes input from query parser and apply rules to filter data in memory. First it applies column selectors then row eleminators.

com.helpshift.databse.storage.executor
com.helpshift.databse.storage.result

packages are primarily dealing with query execution.



How to Run the Program
------------------------------
com.helpshift.telnet.server.TCPServer is the main program. It will open a socket to accept request from terminal (telnet running on port
1234)

Currently it supports the following query 
1. select * from table;
2. select column/(column, column, ..) from table;
3. select column/(column, column, ..) from table where column(=,<,>) some value [both long or integer];
4. select column/(column, column, ..) from table where column(=,<,>) some value [both long or integer] {AND|OR} column(=,<,>) some value [both long or integer];


Sample Output
-------------
badalb$ telnet localhost 1234
Trying ::1...
Connected to localhost.
Escape character is '^]'.
select * from actors;
[id, name, address, age, city][1, Amitabh B, Mumbai, 70, Mumbai][2, Akshay K, Mumbai, 50, Mumbai][3, Rajanikant, Chennai, 65, Chennai][4, Siddharth,  Delhi, 30, Mumbai][5, Varun D, 30, Delhi, Mumbai]

badalb$ telnet localhost 1234
Trying ::1...
Connected to localhost.
Escape character is '^]'.
select * from actors where age > 40;
[id, name, address, age, city][1, Amitabh B, Mumbai, 70, Mumbai][2, Akshay K, Mumbai, 50, Mumbai][3, Rajanikant, Chennai, 65, Chennai]

	
	
Improvements
------------------------------
//TODO: specifies the areas where we can improve further. Primarily Executor and Parser is implemented using rules and inline expressions.

For quick implementation Expression and Evaluators are implemented inline. Ideally for every Expression and Evaluators there should be one implementation class. Like Arithmetic Expressions, Logical Expressions AND/OR/IN clauses and their evaluations.

Object oriented principal has been violated at below mention classes which reduced flexibility in terms of open for extension. 
  
  1. SelectStatementExecutor
  2. SelectStatementParser
  
Every hard coded inline implementation should be a class implementation.

A JavaCC rule based implementation can be far more roboust in terms of supporting different conditions and predicates.

