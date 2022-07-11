# Azure Cosmos DB SQL API

Azure Cosmos DB is a fast NoSQL database service for modern app development at any scale.

## What is a NoSQL database?

NoSQL databases designed for:

- **High volumes** of data
- Data with many **different sources and forms**
- **Dynamic data schemas** to store different types of data
- Using **high-velocity and/or real-time data**

NoSQL databases are defined by common characteristics they share rather than a specific formal definition, namely:

- Data store is non-relational
- Designed for scale-out
- Does not enforce a specific schema

NoSQL databases generally **do not enforce relational constraints or put locks on data**, making **writes fast**.
Often designed for **horizontally scale** via sharding or partitioning, which allows them to maintain high-performance regardless of size.

There are 4 commonly used data model families for NoSQL databases:

- Documents - Azure Cosmos DB SQL API
- Key-Value
- Graph
- Column-Family

Azure Cosmos DB SQL API uses document data model.

## Why use a NoSQL database with the document data model?

- The document data model breaks data down into individual document entities.
- A document can be any structured data type, but JSON is commonly used as the data format.
- The Azure Cosmos DB SQL API supports JSON natively.

A document is an atomic entity and can have its own data form, regardless of what is stored in other documents in the same database. This flexibility allows/enables:
- Building apps rapidly, as there us ni need for a pre-defined schema
- Storing different types of data together where models can evolve over the lifetime of an app

## What is a JSON document?

JSON (JavaScript Object Notation) - is a lightweight data format which was built to be highly compatible with the literal notation of an object in the JavaScript language. Many frameworks, browsers, and databases support JavaScript natively making JSON a popular format for transmitting and storing data.

JSON document example:

```JSON
{
  "device": {
    "type": "mobile"
  },
  "sentTime": "2019-11-12T13:08:42",
  "spoolRefs": [
    "6a86682c-be5a-4a4a-bacd-96c4d1c7ece6",
    "79e78fe2-93aa-4688-89db-a7278b034aa6"
  ]
}
```

JSON is a relatively readable data format that clearly exposes its content. JSON is also relatively easy to parse and use in JavaScript applications.

## What is Azure Cosmos DB SQL API?