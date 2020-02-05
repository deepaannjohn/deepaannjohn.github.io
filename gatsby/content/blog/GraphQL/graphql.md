---
title: Getting started with GraphQL
date: "2020-01-20T22:12:03.284Z"
description: "Some basic inforamtion to get started with GraphQL"
---

**What is GraphQL?**

GraphQL is a query language / API standard  that was invented and open-sourced by Facebook.

GraphQL is a syntax that describes how to ask for data, and is generally used to load data from a server to a client. 

It lets the client specify exactly what data it needs. GraphQL clients are in control of the data they need. They will request for the data that they need (APIs return all the data where as graphQL
return only necessary data, no overfetching)

GraphQL can be called as a more efficient alternative to REST. Increased mobile usage, low power devices and slow networks were the initial reasons why Facebook developed GraphQL. GraphQL minimies the amountof data that needs to be transferred over the network and thus improves the application operating under these conditions.

eg: query from the client requesting for particular fields:

```
{
  
    employee(id: 42) {

        firstName

        lastName

        email

    }

}

```
sample resposne (a JSON object)

```
{

    "employee": {

        "firstName": "ABC",

        "lastName: "XYZ",

        "email":"xyz@gmail.com"

    }

}


```
As we can see, the answer matches the request query. Every field in the query, becomes a key in the answer object.

GraphQL has two parts to it:

* It's a query language
* It has run time as well

Query language 

It's all about the communication or the query language. Query (read operation) and Mutation (Write operation)

GraphQL query language is very similar to JSON.

**GraphQL Editor**

There are many GraphQL demo sites available eg: GraphQLHub.com
There are many projects under GraphQLHub.com. Any of the projects under this 
if clicked will open in an (in browser) editor called GraphiQL (an editor from facebook).


**GraphQL Language syntax**

_query operation_
example:
query QueryName {
  graphQLHub
  github {
    user(username: "deepaannjohn") {
      login
      id
      company
      avatar_url
      repos {
        name
      }
    }
  }
}

tip: use ctrl+space to get prompts inside GraphiQL editor
type String! => this field is mandatory

**GraphQL schema**
Every field we define in a schema has a type. (scalar or non scalar)
Every field has a resolver function to read the logic.

* Client sends a GraphQL request
* The GraphQL server read the input from an interface
* Find the resolver function for each fields
* The response might be a field having another resolver function
* If so, process that resolver as well
* Consolidate the resposne and send to client


**GraphQL Query language**

**Fields**

Scalar fields

They are simple fields (primitive / basic)
examples:
GraphQLInt
GraphQLFloat
GraphQLString
GraphQLBoolean
GraphQLID - special type of id  which appears in all GraphQL responses

Complex fields

They usually have a custom type (eg: GutHubUser having name, companyname etc)

Another example:
GraphQLList


GraphQL fields are modelled after functions. They accept argumets and return something in response.
On the server, we write JS functions to determine the value returned by every field.
These functions are called resolver functions.

In the above github query example, th use field accepted username and used
resolver function to determine the value to  be returned.

Variables

usage: to make query reusable

example:

query ($reponame: String!){
  github {
    repo (name : $reponame, ownerUsername: "facebook")  {
      commits {
        message
        date
      }
    }
  }
}

Directives

Directives are used to alter the graphQL run time and they are used along with variables to achieve this.
examples of Built in directives are

a. @skip
b. @include

Directive also support arguments, just like fields.

eg: @skip, @include accepts a variable if (which is a boolean)

Directives can be used with fields and fragments

eg:
query test($includerepos: Boolean!) {
  github {
    user (username: "deepaannjohn") {
      login,
      repos @include(if: $includerepos) {
        name
      }
    }
  }
}

Aliases

eg:

A UI componenet is expecting user.githubid as 'id' where as the JSON response object from 
GraphQL has 'id' in the response.

We can rename as below:
request:
query test {
  github {
    user (username: "deepaannjohn") {
      login,
      githubid : id
    }
  }
}
response:
{
  "data": {
    "github": {
      "user": {
        "login": "deepaannjohn",
        "githubid": 1234
      }
    }
  }
}

So UI component can use it as response.data.github.githubid.
Again, this gives clients more control.

another use of alias:
to retrieve the same field more than once
query test {
  github {
    user1: user(username: "deepaannjohn") {
      login
      githubid: id
    }
    user2: user(username: "mathewjpallan") {
      login
      id
    }
  }
}

reponse:
{
  "data": {
    "github": {
      "user1": {
        "login": "deepaannjohn",
        "githubid": 1234
      },
      "user2": {
        "login": "mathewjpallan",
        "id": 5678
      }
    }
  }
}


Fragments

Fragments make GraphQL composable

usage: to eliminate repeatition
eg:
query test {
  github {
    user1: user(username: "deepaannjohn") {
      ...UserInfoFragment
    }
    user2: user(username: "mathewjpallan") {
      ...UserInfoFragment
    }
  }
}

fragment UserInfoFragment on GithubUser {
  login,
  id
}

... => spread operator

Mutations

Writing into GraphQL


eg:

mutation m1($inputval: SetValueForKeyInput!) {
  keyValue_setValue(input: $inputval) {
    item {
      id
      value
    }
    clientMutationId
  }
}


{
  "inputval": {
    "id": "1",
    "value": "3635",
    "clientMutationId": "3"
    
  }
}

**GraphQL Runtime**

This is about validation of the queries, the type system, Introspection and Execution

Clients can perform query / mutation operation using a medium like HTTP.

Reads and translates GraphQL documents to other data services and vice versa while handling the response.
Also, GraphQL run time defines a graph based schema to declare teh GraphQL API service capabilties to all clients. 

What we saw above was how to access GraphQL APIs.
Now the next part is to build the GraphQL APIs / server.

eg:
a hello-world graphql application:
request:
{
  hello
}
expected response:
{
  "data":{
    "hello":"world"
  }
}

We need to define the schema first.

To do that we can use npm graphql pacakge.

graphql npm package provides two important capabilities: building a type schema, and serving queries against that type schema.

graphql-helloworld (without http server) is avaialble here:

https://github.com/deepaannjohn/graphql-helloworld

graphql-http-endpoint (with http server endpoint) is available here:

https://github.com/deepaannjohn/graphql-http-endpoint











