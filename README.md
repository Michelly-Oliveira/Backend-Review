# Backend Review

Reviewing concepts introduced on GoStack level 2

### _Reload server:_

```bash
npm install ts-node-dev -D
```

- use _ts-node-dev_ to watch for changes on the files and restart the server

- this package executes TypeScript code without converting it to JavaScript

- restarts node process when required files change, but shares TS compilation process between restarts

- increased speed comparing to node-dev and nodemon variations because there is no nedd to instantiate ts-node compilation each time the node restarts

---

### _Models_

Model is used to define the format of a data entity, how the data is organized. Template of the structure of the object.
Is basically a class, a blueprint of what an object must have.
Define what data/properties and methods must be present and their types.

A model also can be called Entity.
To use a model, you create an object from the model: instantiate the class.

---

### _Repositories_

Repository is the structure that stores the data(related to the model) and is responsible for all operations on top of that data.

Any time you need to manipulate an instance/object of a model, you must use the repository to get/use/modify its data.
All functionality is defined on the repository

---

_Model =_ is the template

_Repository =_ stores and manipulates the data

---

### _Data Transfer Object (DTO)_

When you need to transfer data between files put that data inside an object. It becomes much easier to deal with function parameters.

When used with TypeScript, the file that receives the data must have an interface that defines each variable and its type.

---

### _Service_

A service is a file that stores the rules of how a certain aspect of the app must work.
Conditions, rules of business.

They have one method/function that executes one specific task.

---

### _Route_

Receives request, call other files to manipulate the data, and return a response.

---

### _Database_

Software that stores data in a way that is easy to search for and modify. Needs to be installed on the server.

**Queries**

Make searchs on the database. Can make queries using three methods:

- use native driver, more manual way

- use a query builder, like Knex.js. Create your queries using JavaScript

- use ORM(Object Relational Mapping), TypeORM for TypeScript or Sequelize for JavaScript

**TypeORM**

Map registry of tables on database with JS objects.
Provides a layer of abstraction for queries, so that these queries can be used with other relational databses(SQL).

- After you add a database using TypeORM on your app, the concepts of Model and Repository get related with its tables, that happens because a table stores entities based on the models:

1. Model now is also used to define columns and their types on a table, and the relations between them, if they exist
1. Repository now gets data from database. TypeORM repositories have several methods included, so you can extend it to make your custom methods. But interacting with the database can sometimes take a while, so we use async/await(wait for a promise to resolve before moving on)

**Migration**

A file that contains instructions to create a table on the database.
Controls the version of the database, keep track of the data on the database.

You don't make changes directly on the tables, if you need to make some changes:

1. create another migration that makes the alterations on the table, if the code is already available for other developers
1. or if the code is only local, you can make changes to the migration itself

Using migrations, it is easy to make severals developers on a team keep their databses in sync, because you can just run the migrations and update all the tables, there is no need to manually make changes to the tables whenever you pull changes from other developers.

A migration has two methods:

1. up: what is to be done on the database
1. down: revert the changes that were made on the method up

- example: up creates a table, down deletes the table

**Entity**

Use TypeScript decorator to allow entities.

Function Entity receives a class(that is defined bellow) as parameter, and the class defines the columns of the table and their types.

When you use an Entity the constructor is created automatically. There is no need to instantiate the model anymore using the _new_ keyword.

**Database Relations**

Reference an entry/column on another table.

- OneToOne: one user has one appointment

- OneToMany: one user has many appointments (the user owns the relation)

- ManyToOne: many appointments to one user (appointments owns the relation)

- ManyToMany: many users participate on the same appointment

---

### _JWT_

JSON Web Token (Bearer token)

Is used to authenticate users on a RESTful API.

The token that is generated is in JSON and is encrypted, because it contains some data(id for example) from the user that generated it, though you should not pass sensitive information from the user on it.
Use a secret key (hashed string) to create the token, that is known only to your app (security measure to ensure the token was created by your app)

**Authentication and Authorization**

Tokens are sent on the header of the request.
Create a middleware (function that can manipulate request/response) to handle authentication, and pass it to all the routes that need the user to be logged.

When a user is created, you need to encrypt its password before storing it on the database.
Then, when the user tries to login and the email exists on the database, you need to compare the password passed with the one that is encrypted. You also need to check if the token was created using the secret key

After the token is decoded and we get the user data, we can add it to the request(create a new prop on the request object), so that all the routes that use the middleware(function to check authentication) have access to which user is logged (keep user logged on the app).

---

### _Exception Handling_

Centralize error handling, create an error class that accepts a status code and a message so we can make custom messages to show to the user.

Errors are created on routes, so we must handle errors _after_ the routes

_app.use(routes)_
_app.use(errorClass)_

- use _express-async-errors_ to handle errors on sync functions (express doesn't handle async errors very weel, so we use this package).

---

### _Docker_

Is a tool to create isolated environments/containers that contain external tools/software.

Containers are isolated from one another and bundle their own software, libraries and configuration files. They don't interact with your machine or other softwares unless you create a connection between them through ports.

**Image:** service/tool we can put inside a container

**Container:** instance of an image

**Docker Registry(Hub):** cloud storage for the images

**Dockerfile:** pattern to create your own image

- Create a container with a image:

```bash
sudo docker run --name <nameOfContainer> -e <IMAGE>_PASSWORD=<passwordOfContainer> -p <pcPort>:<portContainer> -d <imageName>
```

- List all containers
