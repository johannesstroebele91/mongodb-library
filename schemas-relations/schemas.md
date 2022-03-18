# Basics

MongoDB does NOT enforce Schemas

- so documents don"t have to use
- the same schema inside of one collection

BUT it enables to use a schema

# Types

## Very different

One collection can have totally different documents:

```JSON
{
    { "_id": "623456df679fb2f726d1cca2",
    "name": "Darkness",
    "price": 12.99
    }
    { "_id": "6234570c679fb2f726d1cca3",
    "title": "Light",
    "seller": { "name": "Max", "age": 29 }
    }
}
```

## Extra data

One collection can have documents where most fields are the same:

```JSON
{
    { "_id": "623456df679fb2f726d1cca2",
    "name": "Darkness",
    "price": 12.99
    },
    { "_id": "6234570c679fb2f726d1cca3",
    "name": "Light",
        "price": 10.99,
    "seller": { "name": "Max", "age": 29 }
    }
}
```

## Full equality

One collection can have documents where all fields are the same:

Important:

- Fields that are not needed for each document
- can be filled with the value `null`

```JSON
{
    { "_id": "623456df679fb2f726d1cca2",
    "name": "Darkness",
    "price": 12.99 }
    { "_id": "6234570c679fb2f726d1cca3",
    "name": "Light",
    "price": null
    }
}
```
