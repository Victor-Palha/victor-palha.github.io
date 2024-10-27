---
layout:
title: "What is GraphQL - The basics core concepts"
date: 2024-10-27 13:23:20 -0000
categories: [Documentation]
tags: [guide, backend, graphql]
---

# What is GraphQL?
Well guys, let's start with the basics! **GraphQL** is a query language for your API and a runtime for executing those queries by using a _Type System_ you define for your data. It was developed by Facebook in 2012 and open-sourced in 2015. GraphQL provides a more efficient, powerful, and flexible alternative to RESTful APIs.

![GraphQL logo](https://everyday.codes/wp-content/uploads/2019/12/og_image.png)

## What is a Query Language?
A query language is a way to define the structure of the data you want to retrieve from a database or API. In the case of GraphQL, the query language allows you to specify the fields and relationships that you want to fetch from a server.

Quoting the [official GraphQL documentation](https://graphql.org/learn/):
> GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.

## Introduction to GraphQL basics concepts
Like any other technology, GraphQL has its own set of concepts and terminology that you need to understand to work effectively with it. Now, let's dive into some of the key concepts of GraphQL:

### Queries & Mutations
Queries and mutations are the two main operations you can perform in GraphQL, of course, there are other operations like subscriptions, but let's focus on the basics for now!

#### Queries
Queries are used to fetch data from a server. They are similar to GET requests in RESTful APIs. A query in GraphQL looks like this:

```graphql
query UserInformation{
  user(id: 1) {
    name
    email
  }
}
```

In this case, we are fetching the `name` and `email` fields of the user with `id` equal to `1`.
The response will be something like this:

```json
{
  "data": {
    "user": {
      "name": "John Doe",
      "email": "john@doe.com"
    }
  }
}
```

Notice that the response structure matches the query structure, which is one of the key features of GraphQL.
Now, let's dive deeper into the concept of **Queries** by checking each part of the query above:

- _query_: This is the keyword that indicates that we are performing a query operation.
- _UserInformation_: This is the name of the query operation. You can name your queries to make them more descriptive.
- _user(id: 1)_: This is the field we are fetching from the server. In this case, we are fetching the user with `id` equal to `1`.
- _name_ and _email_: These are the fields we want to retrieve from the user object.

We can also pass arguments to queries to filter or sort the data we want to retrieve. In this case, we are passing the `id` argument to filter the user we want to fetch.

Notice that we are only fetching the fields we need, which is one of the key benefits of GraphQL. Clients can request only the data they need, reducing the amount of data transferred over the network for example:

```graphql
query UserInformation{
  user(id: 1) {
    name
  }
}
```

In this case, we are only fetching the `name` field of the user with `id` equal to `1`.

#### Mutations
Mutations are used to modify data on the server. They are similar to POST, PUT, PATCH, and DELETE requests in RESTful APIs. A mutation in GraphQL looks like this:

```graphql
mutation CreateUser($name, $email) {
    createUser(name: $name, email: $email) {
        id
        name
        email
    }
}
```

In this case, we are creating a new user with the `name` and `email` fields. The response will be something like this:

```json
{
  "data": {
    "createUser": {
      "id": 2,
      "name": "Jane Doe",
      "email": "jane@doe.com"
    }
  }
}
```

Notice that the **response structure matches the mutation structure**, which is another key feature of GraphQL, everything is predictable and easy to understand!

But maybe you are asking yourself: 
> "From where comes the `$name` and `$email` variables?" 

Well, GraphQL allows you to pass variables to your queries and mutations to make them more dynamic and reusable. In this case, we are passing the `name` and `email` variables to the `createUser` mutation.

Now! Let's dive deeper into the concept of **Mutations** by checking each part of the mutation above:

- _mutation_: This is the keyword that indicates that we are performing a mutation operation.
- _CreateUser_: This is the name of the mutation operation. You can name your mutations to make them more descriptive.
- _createUser(name: $name, email: $email)_: This is the field we are modifying on the server. In this case, we are creating a new user with the `name` and `email` fields.
    - _name_ and _email_: These are the fields we are passing to the `createUser` mutation.
- _id_, _name_, and _email_: These are the fields we want to retrieve from the user object after creating it. Notice that we are fetching the `id` field, which is generated by the server after creating the user.

> [!IMPORTANT] 
> What i'm showing here is a example of how we can use queries and mutations in GraphQL, but we didn't start to create a GraphQL server yet! So, let's move on to the next section to learn how to create a GraphQL server and start using these concepts in practice!

> [!NOTE]
> For more information about queries and mutations in GraphQL, check the [official documentation](https://graphql.org/learn/queries/).

### Types & Fields
Types and fields are the building blocks of a GraphQL schema. A type represents an object in your data model, and fields represent the properties of that object. Let's take a look at an example:

```graphql
type User {
  id: ID!
  name: String!
  email: String!
}
```

In this case, we are defining a `User` type with three fields: `id`, `name`, and `email`. Each field has a type associated with it, such as `ID` and `String`, and an exclamation mark `!` indicating that the field is required so, the client must provide a value for it and it can't be null.

Now, let's dive deeper into the concept of **Types & Fields** by checking each part of the type definition above:

- _type_: This is the keyword that indicates that we are defining a new type.
- _User_: This is the name of the type. You can name your types to make them more descriptive.
- _id_, _name_, and _email_: These are the fields of the `User` type. Each field has a name and a type associated with it.
    - _ID_ and _String_: These are the types of the fields. `ID` is a scalar type that represents a unique identifier, and `String` is a scalar type that represents a sequence of characters.
    - _!_: This is the exclamation mark that indicates that the field is required. If a field is marked as required, the client must provide a value for it, and it can't be null.

#### Arguments
Arguments are used to pass data to fields, queries, and mutations in GraphQL. They allow you to customize the behavior of your operations by providing additional information. Let's take a look at an example:

```graphql
type Movie {
    id: ID!
    name: String!
    year: Int!
    director: String!
    genre: String!
    rating(public: TypeOfRating = PUBLIC): Float!
}
```

In this case, we are defining a `Movie` type with five fields: `id`, `name`, `year`, `director`, `genre`, and `rating`. The `rating` field has an argument `public` of type `TypeOfRating` with a default value of `PUBLIC`.

### Schema & Resolvers
The schema and resolvers are the core components of a GraphQL server. The schema defines the structure of your data model, and the resolvers are responsible for fetching the data from your data sources. Let's take a look at an example:

```graphql
type Query {
    user(id: ID!): User
    users: [User]
}

type Mutation {
    createUser(name: String!, email: String!): User
    updateUser(id: ID!, name: String, email: String): User
    deleteUser(id: ID!): User
}

schema {
    query: Query
    mutation: Mutation
}
```

In this case, we are defining a schema with two root types: `Query` and `Mutation`. The `Query` type defines the queries that clients can perform, such as fetching a single user by `id` or fetching all users. The `Mutation` type defines the mutations that clients can perform, such as creating, updating, and deleting users.

Now, let's dive deeper into the concept of **Schema & Resolvers** by checking each part of the schema definition above:

- _type Query_: This is the root type for queries. It defines the queries that clients can perform.
    - _user(id: ID!)_: This is a query field that fetches a single user by `id`.
    - _users_: This is a query field that fetches all users.

- _type Mutation_: This is the root type for mutations. It defines the mutations that clients can perform.

- _schema_: This is the keyword that indicates that we are defining the schema for our GraphQL server.
    - _query: Query_: This defines the root query type for the schema.
    - _mutation: Mutation_: This defines the root mutation type for the schema.

> [!TIP]
> For more information about types, fields, arguments, schema, and resolvers in GraphQL, check the [official documentation](https://graphql.org/learn/schema/).

It's **SUPER IMPORTANT** to you understand that **EVERY** GraphQL server has a schema that defines the structure of the data that clients can query and mutate. The schema is the contract between the server and the client, and it ensures that both parties understand how to interact with each other.

### Scalar Types
Scalar types are the basic building blocks of a GraphQL schema. They represent primitive data types such as `Int`, `Float`, `String`, `Boolean`, and `ID`. Let's take a look at an example:

```graphql
type User {
    id: ID!
    name: String!
    age: Int!
    height: Float!
    isStudent: Boolean!
}
```

#### Custom Scalar Types
In addition to the built-in scalar types, you can define custom scalar types in GraphQL. Custom scalar types allow you to represent complex data types such as `Date`, `Time`, `URL`, and `Email`. Let's take a look at an example:

```graphql
scalar Date

type Event {
    id: ID!
    name: String!
    date: Date!
}
```

In this case, we are defining a custom scalar type `Date` and using it in the `Event` type to represent the date of the event.

#### Enum Types
Enum types are used to represent a fixed set of values in GraphQL. They allow you to define a list of possible values for a field. Let's take a look at an example:

```graphql
enum Role {
    ADMIN
    USER
    GUEST
}

type User {
    id: ID!
    name: String!
    role: Role!
}
```

Now, we have a `Role` enum type with three possible values: `ADMIN`, `USER`, and `GUEST`. The `role` field of the `User` type can only have one of these values.

### Input Types
Input types are used to pass complex data structures to queries and mutations in GraphQL. They allow you to define a set of fields that can be used as arguments in operations. Let's take a look at an example:

```graphql
input UserInput {
    name: String!
    email: String!
}

type Mutation {
    createUser(input: UserInput!): User
}
```

In this case, we are defining an input type `UserInput` with two fields: `name` and `email`. The `createUser` mutation takes an argument `input` of type `UserInput` to create a new user

### Conclusion
GraphQL is a powerful query language and runtime for APIs that provides a more efficient, powerful, and flexible alternative to RESTful APIs. By understanding the key concepts of GraphQL, such as queries, mutations, types, fields, schema, and resolvers, you can build robust and scalable APIs that meet the needs of your clients. Remember that GraphQL is all about defining a schema that represents your data model and using resolvers to fetch the data from your data sources. By mastering these concepts, you can take full advantage of GraphQL and build modern, data-driven applications that provide a seamless user experience.