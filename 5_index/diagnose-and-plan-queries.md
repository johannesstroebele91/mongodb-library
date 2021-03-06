# Basics

Get information about an executed operation

- e.g. which search method was used `db.someCollection.explain().find()`
- the function works for `explain()` works
  - for `find, update, delete`
  - but NOT for `insert`
- with the parameters:
  - `explain("queryPlanner")` show
    - summary of executed query
    - winning plan ((e.g. IXSCAN vs COLLSCAN))
  - `explain("executionStats")` show
    - detailed summary of executed query
    - winning plan
    - possibly rejected plans
  - `explain("allPlansExecution")` show
    - detailed summary of executed query
    - winning plan
    - winning plan decision process

# How to find out if a query is efficient

1. Milliseconds process time: which plan was faster e.g. IXSCAN often beats COLLSCAN
2. Number of **keys examined in the index**
   - should be as close as possible
   - TO the **number of documents examined**
   - OR the number should be `0`
3. Number of **documents examined**
   - should be as close as possible
   - TO the **number of documents returned**
   - OR the number should be `0`

# How MongoDB selects a winning plan

MongoDB let's

- `plans` compete against each other
- such as multiple IXSCANs or COLLSCAN
- by measuring how they perform
- for a number of the documents

MongoDB cashes the winning plan

- so it does not waste performance
- which gets reevaluated when
  - write treshold is reached (currently 1.000)
  - index is rebuilt
  - other indexes are added or deleted
  - MongoDB server is restarted
