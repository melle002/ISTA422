# T-SQL Chapter 10
## Homework

##### 1. What is the purpose of transactions? Why do we use transactions in SQL scripts?
A transaction is a unit of work that might include multiple activities that query and modify data and that can also change the data definition.
##### 2. Briefly describe each of the ACID properties.
* Atomicity- atomic unit of work. Either all changes in the transaction take place or none do.
* Consistency- The term consistency refers to the state of the data that the relational database management system (RDBMS) gives you access to as concurrent transactions modify and query it.
* Isolation- Isolation ensures that transactions access only consistent data.
* Durability- Data changes are always written to the database’s transaction log on disk before they are written to the data portion of the database on disk.

##### 3. What do we mean when we talk about the granularity of locks?
Lock granularity specifies which resource is locked by a single lock attempt.
##### 4. What do we mean when we talk about the modes of locks?
which lock type
##### 5. In your own words, describe blocking, and give an example.
waiting to access a resource or getting put on a waiting list
##### 6. What are the properties of locks? That is, list the column name and column type of the fields in sys.dm tran locks.
* The resource type that is locked (for example, KEY for a row in an index)
* The ID of the database in which it is locked, which you can translate to the database name * by using the DB_NAME function
* The resource and resource ID
* The lock mode
* Whether the lock was granted or the session is waiting for it

##### 7. What are the properties of sessions? That is, list the column name and column type of the fields in sys.dm exec connections.
* The time they connected.
* The time of their last read and write.
* A binary value holding a handle to the most recent SQL batch run by the connection.

##### 8. What are the requests of sessions? That is, list the column name and column type of the fields in sys.dm exec requests.
This view has a row for each active request, including blocked requests. In fact, you can easily isolate blocked requests because the attribute blocking_session_id is greater than zero.
##### 9. What is an isolation level? Give an example of the operation of an isolation level.
Isolation levels determine the level of consistency you get when you interact with data
##### 10. (Not in the book.) What do we mean when we say that an object is serializable?
it can be copied
##### 11. What is an deadlock? Give an example of a deadlock?
A deadlock is a situation in which two or more sessions block each other.
* An example of a two-session deadlock is when session A blocks session B and session B blocks session A.
* An example of a deadlock involving more than two sessions is when session A blocks session B, session B blocks session C, and session C blocks session A.
