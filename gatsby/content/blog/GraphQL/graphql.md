---
title: Getting started with GraphQL
date: "2020-01-20T22:12:03.284Z"
description: "Some basic inforamtion to get started with GraphQL"
---

**What is GraphQL?**
A layer between the front end client and backend data base.
If there are multuple data servers (eg: mongo, db2, sql), the client have to talk only to GraphQL
GraphQL clients are in control of the data they need. The will request fr the data that they need (APIS return all the data where as graphQL
return only necessary data, no overfetching)
eg query from the client requesting for particular fields:
{
    emplyee(id: 42) {
        firstName
        lastName
        email
    }
}
sample resposne (a JSON object)
{
    "employee": {
        "firstName": "Joe",
        "lastName: "Mathew",
        "email":"jmp@gmail.com"
    }
}
As we can see, the answer matches the request query. Every field in the query, becomes a key in the answer object.

GraphQL has to parts to it:
* Its a query language
* It a run time as well

Query language - Its all about the communication or the query language. Query (read operation) and Murtation (Write operation)
Run time - This is about validation of the querries, the type system, Introspection and Execution

Clients can perform query / mutation operation using a medium like HTTP.

A **GraphQL document** can containe one more more operations.

GraphQL run time - Reads and translates GraphQL docuemntats to other data services and vice versa while handling the response.
Also, GraphQL runt ime defines a graph based schema to declare teh GraphQL API service capabilties to all clients. 

**GraphQL schema**
Every field we define in a schema has a type. (scalar or non scalar)
Every field has a resolver function to ap the field to its read logic.

Client sends a GraphQL request
The GraphQL server read the input rom an interface
Find the resolver function for each fields
The response might be a field having another resolver fucntion
If so, process that resolver as well
Consolidate the resposne and send to client

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


GraphQL fields are modelled afetr functions. They accept argumets and return something in response.
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

directives
Aliases
Fragments
Mutations








