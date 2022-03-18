# Basics

MongoDB does NOT enforce Schemas

- so documents don't have to use
- the same schema inside of one collection

BUT it enables to use a schema

# Example

One collection can have totally different documents:

```JSON
{
{ _id: ObjectId("623456df679fb2f726d1cca2"),
  name: 'Darkness',
  price: 12.99 }
{ _id: ObjectId("6234570c679fb2f726d1cca3"),
  title: 'Light',
  seller: { name: 'Max', age: 29 } }
}
```
