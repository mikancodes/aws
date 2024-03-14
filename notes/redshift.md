# AMAZON REDSHIFT ☁️
 - CLOUD ORIENTED DATA WAREHOUSEING 
 - ITS A PAAS SERVICE(ONLY AVAILABLE IN AWS)
 - ITS BASED ON A PARALLEL COMPUTING MODEL(MPP)
 
REDSHIFT CLUSTER IS COMBINATION OF LEADER NODE AND COMPUTE NODE AND NODE SLICES
 - LEADER NODE-CONTROLED AND MANAGED BY AMAZON
 - COMPUTE NODE-ALL YOUR JOB WILL HAPPED IN DISTRIBUTED MANNER
 - NODE SLICE- NOTHING BUT PROCESS
 
* DENSE STORAGE NODES-LOTS OF STORAGE AND COMPUTE POERT LESS
* DENCE COMPUTE NODES-MORE COMPUTE POWER AND LESS STORAGE POWER
 
IN SINGLE NODE CLUSTER THERE IS NO LEADER NODE
 
----------------------------------------------------------
KEY FEATURE OF AMAZON REDSHIFT--
 - MASSIVELY PARALLEL PROCESSING
 - COLUMNER DATA STORAGE
 - DATA COMPRESSION
 - QUERY OPTIMIZER
 - RESULT CATCHING
 - COMPILED CODE




------------------------------------------------------------



* aws role : kind of permission we give to one service to another service 
* aws use : give permission for external access to the service



--------------------------------------------------------------------


# AWS Redshift WhitePaper

## 1. Redshift — Overview
AWS Redshift is a data warehouse service available on cloud. Redshift is petabyte scale, powerful and fully managed relational data warehousing service. Redshift can connect with client applications with JDBC, ODBC connections. It's based on PostgreSQL standard.

Amazon Redshift provides efficient storage and faster query performance through it's unique properties like massively parallel processing, columnar data storage, and various data compression encoding schemes.

An Amazon Redshift data warehouse is a collection of computing resources called nodes, which are organized into a group called a cluster. Each cluster runs an Amazon Redshift engine and contains one or more databases.

## 2. Characteristics of Redshift
Following are characteristics of AWS Redshift:

a. Massively Parallel Processing
MPP enables fast execution of complex queries on large amount of data. In Redshift, multiple compute nodes work in parallel and handle all query processing to get final aggregated result. Each node executes the same compiled query segment on portion of data allocated. Redshift distributes the rows of a table to the compute nodes so that the data can be processed in parallel. Internal communication helps to perform parallel processing.

b. Columnar Data Storage
Columnar data storage is one of the best property of Amazon Redshift. This keeps it ahead with other databases in term of performance and optimized way of storing data into Redshift. Columnar storage for database tables reduces the overall disk I/O requirement. This is an important factor in optimizing analytic query performance. This reduces the amount of data we need to load from disk. Loading less data into memory enables Redshift to perform more in-memory processing when executing queries.

c. Data Compression
Compression is column level operation which reduces size of data when it stored. Redshift compress the data which reduces the requirement of disk I/O which eventually result into gain query performance. At the time of query execution, compressed data is read into memory, then uncompressed. Loading less data into memory for execution enable Redshift to allocate more memory for analyzing the data. As Redshift uses columnar storage to store similar data sequentially, thus Redshift is able to adaptive compression encoding specifically tied to columnar data types. Best way to apply compression is to allow Redshift to apply optimal compression encoding.

d. Query Optimizer
Redshift query optimizer implements significant enhancement for processing complex analytic queries on large dataset by taking advantage of MPP and columnar data storage properties. This turns into generate optimized query execution plan to perform fast query execution.  

e. Result Caching
Redshift buffers the result of recent queries into memory of leader node. This avoids to perform parsing (syntax check, Symantec check etc.) and generating the execution plan. This helps to reduce the execution time of the query and improve the query and database performance. When a query is received for execution, Redshift checks if valid match is found in buffer, then that buffered result will be returned to user. It's enabled by default. Caching can be changed by setting enable_result_cache_for_session parameter to off.

f. Compiled Code
Leader node distributes optimized compiled code across all available nodes of a cluster. Compiling the query eliminates the overhead associated with an interpreter and therefore increases speed of user queries. Compiled code is buffered and shared across all sessions on the same cluster so the subsequent execution of the same query [with different parameter] will be faster. The execution engine compiles different code for JDBC, ODBC, and PSQL connection protocols. For e.g. two clients using different protocol executing same query for first time, will have first time compilation code.

## 3. Redshift Architecture 
Here are the components of Redshift Architecture

a. Cluster
A Cluster in terms of Redshift is set of one or more compute nodes. There are two type of nodes Leader Node and Compute Node described below. If a cluster has two or more compute nodes, and additional leader node is there to coordinate with all compute nodes and external communication with client applications. In case of single compute node, it act as leader as well as compute node. There can be 101 nodes/cluster and 200 reserved nodes.

b. Leader node
Leader node interacts with client applications and communicates with compute nodes to carry out operations. It parses and generates an execution plan to carry out database operations. Based on execution plan, then it compiles the code. Then compiled code is distributed to all provisioned compute nodes and its data portion to each node.  

c. Compute nodes
Primary responsibility of leader node to compiles code, interaction with external applications and client applications. Leader node compile each step of execution plan and assign that to all compute nodes. Compute nodes carry out execution of the given compiled code and send back intermediate results back to leader node to aggregate the final result for each request from client application.

Each compute node has its own dedicated CPU, memory and storage which are essentially determined by node type. Based on workload, compute capacity, storage of a cluster can be enhanced by adding either by adding number of nodes, or by upgrading node type or both. 

AWS Redshift provides two node types at high level:
· Dense storage nodes (ds1 or ds2)  
· Dense compute nodes (dc1 or dc2)

Max Total tables — 900 [For large and xlarge cluster node types and 20000 for 8xlarge cluster node types]

d. Node slices
Compute node is further partitioned into slices. Each slice is allocated a portion of node's memory, disk space where it carries out the given workload to the node. Leader node manages distributing data to slices for any queries or other database operations to the slices. All slices work in parallel to complete the operation. The number of slices are determined by node size of the cluster.

e. Internal network
Internal network is for communication between leader node and compute nodes to perform various database operations. Redshift has very high-bandwidth connections, various communication protocols to provide high speed, private and secure communication across leader and compute nodes.

f. Databases
A cluster contains one or more databases. User data is stored on compute nodes. Redshift is a RDBMS, so it is compatible with other RDBMS applications. Although it provides same functionality as typical RDBMS including OLTP functions like DMLs, however it's optimized for high-performance analysis and reporting on large datasets. Limit of user defined databases 60/cluster and total schemas 9900/cluster

g. Connections
Redshift interacts with client applications using JDBC and ODBC drivers for Postgre SQL.

h. Client applications
AWS Redshift provides flexibility to connect with various client tools like ETL, business intelligence reporting, and analytics tools for e.g. Matillion for Redshift. It is based on industry standard PostgreSQL most existing SQL client applications are compatible and work with little or without any changes.

Redshift Internal Workflow Diagram
![Redshift Internal Workflow Diagram](https://i.imgur.com/vEH0ohX.png)

## 4. Query Planning and Execution workflow
This section explains order of execution of a query:

a. Leader node receives the queries from client application. First step of query execution is to parse the query which includes syntax and semantic check of the query.  

b. Once query is parsed, parser generates a query tree which is a logical representation of the received query. This query tree is input for query optimizer.

c. Query optimizer evaluates and rewrite the query to maximize the efficiency of the query.

d. Optimizer generates an optimized query execution plan. This plan has join types, join orders, aggregation options and data distribution requirements.

e. Execution engine translates the query plan into steps, segments and stream
Step — Each step is an operation required for query execution. Steps can be combined to allow compute nodes to perform query, join etc.  
Segment — A combination of several steps that can be done by a single process, also the smallest compilation unit executable by a compute node slice.
Stream — Collection of segments to be parceled out over the available compute node slices. Execution engine generates the compiled code on steps, segments and streams. Compiled code executes faster than interpreted code and require less capacity

## 5. Other Features
Redshift has list of features which help us in our day to day activities. These features can help us gaining productivity, managing workload and security in our Redshift cluster. Few of them can be managed through Redshift parameters groups or from AWS console or carried out manually as and when required.

a. Workload Management
Redshift provides a feature of workload management this includes manage priorities of the queries to avoid bottleneck in the database. When a query is sent for execution, WLM assigns the query to a queue according to the user's user group or by matching a query group that is defined in the queue configuration with a query group label that the user sets at runtime.

Query queues are created at runtime according to service classes, which define the configuration parameters for various types of queues, including internal system queues and user accessible queues. 

As workload is managed by queue, there is a default queue as well as a super user queue associated with a cluster. Any query which is not routed to a queue will be executed with default queue while super user queue is reserved for operations performed by super user.

b. Backup and Recovery
Backup of the data in Redshift database can be taken by Automatic or Manual backup/Snapshot. Backups can be used to restore the data to a cluster. While data is restored from a snapshot, a new cluster is created with snapshot data. 

We can take or schedule backup from AWS management console. Redshift periodically takes automatic backup incrementally and saves it to Amazon S3. Also, Redshift allows to set snapshot retention period while taking manual backup. Automated snapshots cannot be deleted manually. Snapshots can be replicated to other regions as well. There can be 20 snapshots per cluster.

c. Auditing and Logging
We can use the database audit and logging feature to track information about number of activities performed against Redshift cluster like authentication attempts, connections, disconnections, changes to database user definitions, and queries executed. This information is useful for security and troubleshooting purposes in Amazon Redshift. Auditing and Logging can be enabled from AWS management console by selecting the cluster for which audit is required.

d. Events and Notifications
Amazon Redshift tracks all events takes place in Redshift cluster and retains information about them for a period of several weeks in AWS account. For each event, Amazon Redshift reports information such as the date the event occurred, a description, the event source (for example, a cluster, a parameter group, or a snapshot), and the source ID. Redshift event notifications can be setup to monitor activities in the cluster by specifying the filters.

When an event occurs that matches the filter criteria, Amazon Redshift uses Amazon Simple Notification Service to actively inform the user that the event has occurred.

e. Scalability and Elasicity
AWS Redshift gives flexibility to add number of compute nodes, change node types as and when needed. It's simple and quickly scales with few clicks or simple API call. Scaling does not require any downtime, we can continue to run database operations during scaling happens. Provisioning and release of resources can be done on-demand in Redshift.  

f. Fault Tolrance and High availability
Amazon Redshift enhance the reliability of data warehouse cluster by continuous monitoring of the health of the cluster. Redshift is available in multiple Area Zones in various geographical regions. With this feature, selecting nearest area zone latency on database operations can be achieved. In order to achieve high availability or fault tolerance we can replicate Redshift cluster in multiple AZs.

g. Vacuum
Redshift Vacuum resorts rows and reclaim space in either a specified table or all tables in current database. Timely performing VACUUM on database objects helps to update statistics of that object. These statistics are used by query optimizer to generate query execution plan. There are various parameters we can use with Vacuum:
· FULL — This is default option. This sorts the table and reclaim the disk space occupied by rows which are deleted or updated. This option does not perform any re-indexing
· SORT ONLY — Performs only sort operation, does not reclaim space by Delete/Update  
· DELETE ONLY — Performed automatically by the cluster in background. Reclaim disk space
· REINDEX — Lengthy and time taking operation. It makes additional round to analyze new records/keys. Sort and Merge may take longer than usual to complete

h. Security
Security is one of the major concern on the cloud for the clients. AWS is responsible for protecting the infrastructure that runs AWS services in the AWS Cloud. Once encryption is enabled on the cluster all data blocks and system metadata is also encrypted. This covers snapshots as well

Cluster Security — AWS secures cluster with VPC, SSL connections. Security of the cloud is done by AWS and its effectiveness is regularly verified by third-party auditors as part of AWS compliance programs.

Data Security — Data security is categorized at various levels like data at rest, and in-transit data. In-transit data can be protected by using SSL or by client-side encryption. While data at rest can be done by server side encryption

Security of the data can be controlled by user accounts, schema in AWS Redshift database. We can limit the user access by granting the permission only for the required resources.

Encryption can be enabled at the launch of the cluster, or we can modify unencrypted cluster to use AWS Key Management Service (AWS KMS) encryption.

i. Cost
Cost is the major factor while choosing any infrastructure, service. AWS Redshift have various options for variety of business needs. It simple works on pay as you go model. AWS Redshift is the only cloud data warehouse that provide On-Demand pricing with no up-front costs. Moreover, Reserved Instance pricing can help us to save up to 75% by opting to long-term plan (1 year or 3 years). Cost of the Redshift is derived from various factors like type of node, number of nodes.

Nodes Pricing (On-Demand)
On-Demand pricing model is to pay for the nodes and types of nodes by hour with no upfront costs associated with the cluster. We can change the number of nodes and type of nodes at any point of time based on workload. Best suited for Non-Production like environments. Following table illustrate the On-Demand Pricing:-

Node Pricing — Reserved Instance
Reserved nodes/Instance are best fit for Production like systems workload. Reserved nodes helps to gain massive cost saving as compared to on-demand pricing of Redshift Nodes. Reserved instances are available in 1 year or 3 year plan.

Following factors derive the Node Type and Number of Nodes
· Volume of data we import into our Redshift cluster
· Complexity and frequency of the queries on Redshift cluster  
· Number and Need of downstream systems which uses the query result from Redshift cluster

## 6. Operations on AWS Redshift
There are numerous operation we can perform on database like query, create, modify or remove database objects, records, loading and unloading data from and to Simple storage service etc. Details are as follows:

a. Query
Redshift allows to use SELECT statement to extract data from tables. It allows to extract specific columns, restrict rows based on given conditions using WHERE clause. Data can be sorted in ascending and descending order. Redshift allows extracting data using joins, subqueries, call in-built and user defined functions etc.

b. DML
Redshift allows to perform transactions using INSERT, UPDATE, DELETE commands. DML's require commit to be saved permanently in database or Rollback to revert the changes. A set of DML's are known as Transactions. Transaction is completed if any COMMIT, ROLLBACK, or any DDL is performed.

c. Loading and Unloading Data
Load and Unload operations in Redshift is done by COPY and UNLOAD command. COPY command copies data from files in S3 while UNLOAD dumps the data into S3 buckets in various formats. COPY command can be used to load data into Redshift from data files or multiple data stream simultaneously. Redshift recommends to use COPY command in in case of bulk inserts rather than INSERT.

Amazon Redshift splits the result of a select statement across a set of one or more files per node slice to simplify parallel loading of data. While unloading the data into S3, files can be generated serially or in parallel. Also, there is a provision to specify the file size. UNLOAD encrypts the data files using Amazon S3 server side encryption (SSE-S3). Max size of a row can be 4 MB in Copy command.

d. DDL
Specifically known as data definition language. CREATE, ALTER, DROP are names of few can be used to create, modify and delete Database, SCHEMA, USER, and database objects like Tables, views, stored procedure, user defined functions etc. Truncate can used to delete tables data and faster than delete. Truncate releases the space immediately.

e. Grant, Revoke
Access can be shared and restricted for different set of user groups using Grant and Revoke statements. Access can be granted individually or in the form of roles.

f. Functions
Functions are data set objects with predefined code to perform a specific operation. They stored in database as precompiled code and can be used in select statement, DML, and in any expression. Functions provide reusability, avoid redundant code. There are two types of functions.

i. User defined functions
Redshift allows to create a custom user-defined scalar function (UDF) using either a SQL SELECT clause or a Python program. Function must return a value. The user defined functions are stored in the database. User defined functions can be used by user who has sufficient privileges to run. For Python UDFs, in addition to using the standard Python functionality, AWS provide a feature to import own custom Python modules. Functions can be created by CREATE FUNCTION command.

ii. In-Built Functions
a. Character functions
b. Number and Math functions
c. JSON functions  
d. Date Type formatting functions
e. Aggregate/Group Functions  

g. Stored Procedures
Stored procedures can be created in Red
