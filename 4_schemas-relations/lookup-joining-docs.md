# Basics

The look operations allows to

- join related documents
- using a reference
- (so when documents were not nested)

# Example

1. Insert the authors `db.authors.insertMany([{name: "Max Schwarz", age: 29, address: {street: "Main"}}, {name: "Manuel Lor", age: 30, address: {street: "Tree"}}])`
2. Find out the authors id `db.authors.find()`
3. Make a reference for a book using the id of the authors `db.books.insertOne({name: "Book", authors: [ObjectId("623708d259b36fb055c61e0e"), ObjectId("623708d259b36fb055c61e0f")]})`
4. Join the related documents using the id `db.books.aggregate([{$lookup: {from: "authors", localField: "authors", foreignField: "_id", as: "creators"}}])`

```javascript
{ _id: ObjectId("623707fd59b36fb055c61e0d"),
  name: 'Book',
  authors:
   [ ObjectId("623708d259b36fb055c61e0e"),
     ObjectId("623708d259b36fb055c61e0f") ],
  creators:
   [ { _id: ObjectId("623708d259b36fb055c61e0e"),
       name: 'Max Schwarz',
       age: 29,
       address: { street: 'Main' } },
     { _id: ObjectId("623708d259b36fb055c61e0f"),
       name: 'Manuel Lor',
       age: 30,
       address: { street: 'Tree' } } ] }
```
