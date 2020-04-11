## What is GraphQL?

GraphQL is a query language for your APIs. It's also a runtime for fulfilling queries with your data. The GraphQL service is transport agnostic but is typically served over HTTP.

Whenever a query is executed againt a GraphQL server, it is validated against a type system. Every GraphQL service defines types in a GraphQL schema. You can think of a type system as a blueprint for your API's data, backed by a list of objects that you define.

GraphQL is often referred to as a declarative data-fetching language. By that, we mean that developers will list their data requirements as what data they need without focusing on how they are going to get it.

### The GraphQL Specification

GraphQL is a specification (spec) for client-server communication. What is a spec? A spec describes the capabilities and characteristics of a language.

### Design principles of GraphQL

Even though GraphQL is not controlling about how you build your API, it does offer some guidelines for how to think about a service:

- Hierarchical
  A graphQL query is hierachical. Fields are nested within other fields and qeuery is shaped like the data that it returns.

- Product centric
  GraphQL is driven by the data needs of the client and the language and runtime that support the client.

- Strong typing
  A graphQL server is backed by the graphQL type system. Each data point has a specific type against which it will be validated.

- Client-specified queries
  A GraphQL server provides the capabilities that the clients are allowed to consume.

- Introspective
  The GraphQL language is able to query the GraphQL server's type system.

### History of Data Transport

#### Remote Procedure Call

Request some data from the client, get a response from the server.

#### Simple Object Access Protocol(Soap)

Soap used XML to encode a message and HTTP as a transport. SOAP also used a type system introduced the concept of resource-oriented calls for data.SOAP offered fairly predictable results but caused frustration because SOAP implementations were faily complicated.

#### REST

REST is a resource-oriented architecture in which users would progress through web resources by performing operations such as GET, PUT, POST, and DELETE. The network of resources can be thought of as virtual state machine, and the actions are state changes within the machine.

##### REST drawbacks

- Overfetching
  We are getting a lot of data back that we don't need. The client requires 3 data points, but we are getting back an object with 16 keys and sending information over the network that is useless.

- Underfetching
  GraphQL is able to solve this by defining a nested query and then request the data all in one fetch. We receive only the data that we need in one request and the shape of the query matches the shape of the returned data.

- Merging REST Endpoints

There are GraphQL clients emerged to speed the workflow for developer teams and improve the efficiency and performance of applications, such as Relay(facebook).

## The GraphQL query language

SQL is structured query language, GraphQL takes the ideas that were originally developed to query databases, and applies them to the internet. Even though they are both query languages, GraphQL and SQL are completely different. They are intended for completely different environments. You send SQL queries to a database. You send GraphQL queries to an API. SQL data is stored in the data tables. GraphQL data can be stored anywhere: a database, multiple databases, GraphQL is a query language for the internet.

### public GraphQL APIs

- SWAPI (the Star Wars API)
  This is a Facebook project that is a wrapper around the SWAPI REST API.
- GitHub API
  One of the largest public GraphQL APIs, the GitHub GraphQLAPI allows you to send queries and mutations to view and change yourlive data on GitHub. You’ll need to sign in with your GitHub account tointeract with the data.

## Designing a Schema

GraphQL is going to change your design process. Instead of looking atyour APIs as a collection of REST endpoints, you are going to begin looking at your APIs as collections of types. Before breaking ground onyour new API, you need to think about, talk about, and formally define the data types that your API will expose. This collection of types is called a schema.

Schema First is a design methodology that will get all of your teams on the same page about the data types that make up your application. The backend team will have a clear understanding about the data that it needs to store and deliver. The front end team will have the definitions that it needs to begin building user interfaces. Everyone will have aclear vocabulary that they can use to communicate about the system they are building.

### Type

The core unit is the type. A type represents a custom object and these objects describe your application’score features.
The exclamation point specifies that thefield is non-nullabl

## Creating a GraphQL API

A schema defines the query operations that clients are allowed to make and also how different types are related. A schema describes the data requirements but doesn’t perform the work of getting that data. That work is handled by resolvers.
