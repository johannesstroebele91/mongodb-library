# Basics

Multiple collections

- can be related
- which can be done using:

# Nested / embedded documents

- Pro: only one query needed to fetch all the user data
- Contra: might lead to lots of duplicated code, which is harder to handle if it changes

```json
{
  "userName": "Max",
  "age": 29,
  "address": {
    "street": "Second Street",
    "city": "New York"
  }
}
```

# References (like SQL)

- Pro: easier to handle if the data in one of the collection changes
- Contra: multiple queries needed to fetch all user data

# User

```json
{
  "userName": "Max",
  "age": 29,
  "address": "id1"
}
```

# Address

```json
{
  "_id": "id1",
  "street": "Second Street",
  "city": "New York"
}
```
