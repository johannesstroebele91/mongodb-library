# Basics

Indexes enable to

- retrieve data more efficiently
- if done correctly

# Example

`db.products.find({seller: "Max"})`

If there is `no index` for the searched field e.g. `seller`

- MongoDB does a **COLLSCAN** (collection scan) by
- going through the complete collection and
- each document and
- look for if `seller` equals `Max`

If there is an index

- MongoDB does a **IXSCAN** (index scan) by
- recognizing that for the key `seller` an index exists and
- going to the `seller index`
- which is easier to scan for
- because it is always an ordered list of all values and
- it can directly "jump" to the filtered documents
- e.g. `Anna, Anna, Chris, Manuel, Max, Max`

# Disadvantage of indexes

Indexes lead to a lower performance

- for `insert` OR `update` operations
- e.g. if creating an index for all fields of a collection
- because if a new document is inserted OR updated
- all indexes need to be adjusted
- because the new value needs to be added and
- the list sorted
