---
layout: post
title: How to correctly calculate the Request Units in Azure Cosmos DB
description: There is only one recommendation, don't calculate or estimate the Request Units in Azure Cosmos DB - Measure them!
tags: azure-cosmos-db
published: true
---

The cost of all Azure Cosmos DB database operations is normalized and expressed in terms of Request Units (RUs).
RU/s is a rate-based currency, which abstracts the system resources such as CPU, IOPS, and memory that are required.

Azure Cosmos DB requires that specific RU/s are provisioned.  
These ensure that sufficient system resources are available for your Azure Cosmos database all the time to meet or exceed the Azure Cosmos DB SLA. It is possible to provision throughput at two distinct granularities: the whole database or individual containers.

Multiple of my customers have asked in the last weeks why the [official documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/request-units) around RUs has changed and there are no longer samples or calculations in it.

I would personally just summarize it like this:
> **Reality is the only thing that’s real!**

<!--more-->

## Don't Calculate or Estimate - Measure!

Inside the [Request units in Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/request-units) you can finde the main considerations for the things which have impact on the consumed RUs. Let's summarize:

Item size
  : Larger size > More RUs consumed

Item indexing
  : Indexing more items > More RUs consumed

Item property count
  : Indexing more properties > More RUs consumed

Indexed properties
  : Deeper property hierarchies > More RUs consumed

Data consistency
  : Strong and bounded staleness consume ~2X more RUs at reads

Query patterns
  : Number of query results, number of predicates, nature of the predicates, number of UDFs, size of source data, size of result set and projections affect the RUs consumed

Script usage
  : As with queries, stored procedures, and triggers consume RUs based on the complexity of the operations being performed

> "As you develop your application, inspect the request charge header to better understand how much request unit capacity each operation consumes."

Thus the only recommendation and guideline I can give:  
Don't calculate or estimate - measure!

## The Azure Cosmos DB Request Units Tester

And for making it easier to measure different document types, document layouts, queries with different consistency levels and indexing policies I have implemented a little sample application to automate it.

You can find the [Azure Cosmos DB Request Units Tester](https://github.com/RicardoNiepel/AzureCosmosDB-RequestUnitsTester) sources and more documentation on GitHub - any feedback is welcome!

It is build as an .NET Core console application and can be simply run with `dotnet run`.

### What happens underneath

The console application tests all combinations of

- Consistency Level
- the defined Indexing Policies
- the defined Documents & Queries

For each document and the possible operation the following is tracked:

- original document size
- document size inside CosmosDB (with metadata, without formatting)
- request units charge for each operation

For each query the following is tracked:

- count of items returned
- total size of all items returned
- request units charge

After that it generates a 'results.csv' with all the results.

## Same Examples

The [Azure Cosmos DB Request Units Tester](https://github.com/RicardoNiepel/AzureCosmosDB-RequestUnitsTester) comes already with some samples to show how it can be used and how important it is, to get real numbers with real scenarios.

In addition to the three samples below, you can easily see that consistency and indexing can have a large impact.

### Metadata and General Overhead

The first sample shows, how big the overhead could be.

Let us compare the metadata overhead:

- [document.simple1](https://github.com/RicardoNiepel/AzureCosmosDB-RequestUnitsTester/blob/master/scenarios/document.simple1.json) has a original size of *0.038 KB*, but if you save this into Cosmos DB, the size with metadata will be *0.243 KB*
- [document.simple2](https://github.com/RicardoNiepel/AzureCosmosDB-RequestUnitsTester/blob/master/scenarios/document.simple2.json) has a original size of *0.798 KB*, but if you save this into Cosmos DB, the size with metadata will be *1.003 KB*

Let us compare the RUs needed for different operations and see the general overhead:

| Document  | Create RUs | Read RUs | Consistency | Indexing
| --------- | ---------- | -------- | ----------- | --------
| *both*    | 4.95       | 1        | Eventual / ConsistentPrefix / Session | Lazy
| *both*    | 4.95       | 2        | BoundedStaleness / Strong | Lazy
| simple1   | 5.71       | 1        | Eventual / ConsistentPrefix / Session | Consistent
| simple1   | 5.71       | 2        | BoundedStaleness / Strong | Consistent
| simple2   | 5.9        | 1        | Eventual / ConsistentPrefix / Session | Consistent
| simple2   | 5.9        | 2        | BoundedStaleness / Strong | Consistent

As you can see especially for very tiny documents, the metadata overhead is substantial.  
In addition there is a "basic charge" which in particular comes into eyes for small documents.

### Read vs. Query

The second sample shows, how a simple difference can make a big difference at the end.

I'm talking about the difference between "reading" a single document with the id and doing the same using a query.  
Thus I'm talking about

```
GET
https://{databaseaccount}.documents.azure.com/dbs/{db-id}/colls/{coll-id}/docs/{doc-id}
```

vs.

```sql
SELECT * FROM Coll c WHERE c.id = 'c123'
```

Let us compare it:

| Document  | Query    | Read RUs | Query RUs | Consistency | Indexing
| --------- | -------- |-------- | --------- | ----------- | --------
| *both*    |          | 1       |           | Eventual / ConsistentPrefix / Session | *both*
| *both*    |          | 2       |           | BoundedStaleness / Strong | *both*
|           | simple 1 |         | 2.82      | Eventual / ConsistentPrefix / Session | *both*
|           | simple 2 |         | 2.84      | Eventual / ConsistentPrefix / Session | *both*
|           | simple 1 |         | 5.64      | BoundedStaleness / Strong | *both*
|           | simple 2 |         | 5.68      | BoundedStaleness / Strong | *both*

Please ensure to do not query if you just want to read an item per id.

### Document Layout

Clemens Vasters has a nice post regarding [Data Encodings and Layout](http://vasters.com/blog/data-encodings-and-layout/) in the context of options for messaging.

But especially the chapter [Data Layout Convention for Map/Array/Value Encodings](http://vasters.com/blog/data-encodings-and-layout/#data-layout-convention-for-maparrayvalue-encodings) could also be applied to document design.

To show the impact little layout changes can have, take a look at the following:

Original layout [document.complex1.json](https://github.com/RicardoNiepel/AzureCosmosDB-RequestUnitsTester/blob/master/scenarios/document.complex1.json):

```json
"name": {
    "first": "Acevedo",
    "last": "Kramer"
},
"contact": {
    "email": "acevedo.kramer@rameon.io",
    "phone": "+1 (832) 568-2233",
    "address": "328 Macon Street, Kula, Massachusetts, 332"
},
"location": {
    "latitude": "77.548518",
    "longitude": "-113.83542"
},
"friends": [
    {
        "id": 0,
        "fullname": "Cecilia Nielsen"
    },
    {
        "id": 1,
        "fullname": "Bauer Benson"
    }
]
```

Flatten layout [document.complex2.json](https://github.com/RicardoNiepel/AzureCosmosDB-RequestUnitsTester/blob/master/scenarios/document.complex2.json):

```json
"first": "Acevedo",
"last": "Kramer",
"email": "acevedo.kramer@rameon.io",
"phone": "+1 (832) 568-2233",
"address": "328 Macon Street, Kula, Massachusetts, 332",
"latitude": "77.548518",
"longitude": "-113.83542",
"friends": [
    {
        "id": 0,
        "fullname": "Cecilia Nielsen"
    },
    {
        "id": 1,
        "fullname": "Bauer Benson"
    }
]
```

Optimized layout [document.complex3.json](https://github.com/RicardoNiepel/AzureCosmosDB-RequestUnitsTester/blob/master/scenarios/document.complex3.json):

```json
"first": "Acevedo",
"last": "Kramer",
"email": "acevedo.kramer@rameon.io",
"phone": "+1 (832) 568-2233",
"address": "328 Macon Street, Kula, Massachusetts, 332",
"latitude": "77.548518",
"longitude": "-113.83542",
"friends_header": ["id", "fullname"],
"friends_values": [
    [0, "Cecilia Nielsen"],
    [1, "Bauer Benson"]
]
```

Let us compare it:

| Document             | Create RUs | Document Size | Indexing
| -------------------- | ---------- | ------------- | ---------
| origin: complex1     | 49.71      | 1.984 KB      | Consistent
| flatten: complex2    | 48.38      | 1.95 KB       | Consistent
| optimized: complex3  | 45.14      | 1.672 KB      | Consistent

As you can see it could be very helpful to consider different document layout options.

As a sample below are the two different Cosmos DB SQL queries for the first and third layout.

```sql
-- Query for original layout (document.complex1.json)
SELECT c.id
FROM c
JOIN f in c.friends
WHERE f.fullname = "Bauer Benson"
```

vs.

```sql
-- Query for optimized layout (document.complex3.json)
SELECT c.id
FROM c
JOIN f in c.friends_values
WHERE f[1] = "Bauer Benson"
```

**Both** queries are consuming 6.82 RUs for 1 item.

## Feedback

Please give me your feedback and share your insights.

- [@RicardoNiepel](https://twitter.com/RicardoNiepel)
