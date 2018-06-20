# CQRS

Command Query Responsibility Segregation

## What is CQRS?

- an architectural pattern
- that separates reading and writing into two different models.

This means that every method should either be

- **a Command** that performs an action or **a Query** that returns data.
- **a Command** cannot return data and **a Query** cannot change the data. 

By splitting the application into dedicated read and write models, you move the responsibility into dedicated objects.

- The write model does not need to be concerned with returning data and
- the read model can be specifically written to return the correct data to satisfy the application’s requirements.


## Using two separate data stores

-  you use two different models for reading and writing.
- the actual implementation of this idea can be handled in a number of different ways.


### 방법1.

> writing and reading from the same database

- use two different models that talk to the same database.
- This is good solution for applications that require synchronous processing and immediate results.

### 방법2. 

> use two different database.

- the write model will send commands to one database,
- and then the data will asynchronously update the read database.

방법2가 더 좋다.

#### 이유1.

you can store the data in the read database as **denormalised data**.

this has the  advantage of allowing you to 

- write more efficient queries,
- store data that makes more sense as to how it should be displayed in your application.

#### 이유2.

you can **scale the two different sides** of your application separately.

If you have a very high number of reads, and a low number of writes,

- you can scale the query side of the application without worrying about write side.

원문: [what is CQRS](https://www.culttt.com/2015/01/14/command-query-responsibility-segregation-cqrs/)
