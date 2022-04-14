# Introduction

## About this Workshop

Oracle Text allows you to do fast, full-text searching in Oracle Database.

While superficially it is similar to an indexed version of the LIKE operator, there are many differences.

Oracle Text creates **word-based** indexes on textual content in the database. That content can range from a few words in a VARCHAR2 column to multi-chapter PDF documents stored in a BLOB column (or even externally on a file system or at a URL).

This workshop is an introduction to Oracle Text indexes, and is designed to walk you through the basics of creating a text index, performing text queries, and maintaining text indexes. Later workshops will explore the more advanced capabilities of the product.

### Workshop Scenario

We're going to create a simple table with a number column and a VARCHAR2 column.

We'll populate that table, then create a text index on it.

We'll then work through various types of query using the Oracle Text CONTAINS operator.

Finally, we'll look at how to SYNC and OPTIMIZE Oracle Text indexes.

### Prerequisites

Oracle Text is a SQL-level toolkit. This workshop assumes you have:

* Some familiarity with basic SQL concepts
* An Oracle Cloud account

You may now proceed to the next lab.

## Learn More

* [Oracle Text Homepage](https://www-sites.oracle.com/database/technologies/appdev/oracletext.html)

## Acknowledgements

* **Author** - Roger Ford, Principal Product Manager
* **Last Updated By/Date** - Roger Ford, March 2022
