# Using XPE DB

## Introduction

In the previous tutorial, we briefly touched the builtin XPE DB, which allows one to create REST services with ease. The XPE DB engine uses an innovative architecture so developers can create very fast data services even without much tuning.

### How XPE DB works?

XPE consists of an advanced storage engine which ensures sequential writings so we can achieve maximum wrtiting speed the underlying hardware allows. All records are indexed by in-memory indexers so data retrivel is very fast. However, every indexer carries a cost and too many indexers on a DB will slow down the update speed.

Each type of indexer has a different cost.  Most indexers would carry a search cost of O(1) or O(n).  Indexers with sorting would typically carray cost of O(nlog(n))

### Design a schema

### Cross-field index

### Range search

### Security

