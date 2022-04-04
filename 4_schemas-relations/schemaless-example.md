# Operations

Users can make the following operations:

- create post
- edit post
- delete post
- fetch posts
- fetch post
- comment

# Kind of data

user

- id
- name
- age
- email
- posts (reference)
- comments (reference)

post

- id
- title
- text
- tags
- comments (reference)

comment

- id
- text

# Relations

- user < 1 : n > post
  - one user can have many posts,
  - one post can only have one user
- user < 1 : n > comment
- post < 1 : n > comment
  - one post can have many comments, BUT
  - one comment always belongs to one post

# Approaches

1. Two collections
   - user collection
   - post collection which
     - includes the comments and
     - references the user based on the id
   - pro: better then nesting everything like the second approach
2. One "post" collection with nested user and nested comments
   - pros: nesting comments is a good idea
   - cons: nesting user is not good,
     - we get a lot of duplicated date because
     - users can great multiple posts

# HowTo

1. Create blog database `use blog`
2. Create users `db.users.insertMany([{name: "Max Schwarzmeuller", age: 29, email: "max@test.com"}, {name: "Manual Lorenz", age: 30, email: "manu@test.com"}])`
3. Identify user ids `db.users.find()`
4. Create posts with reference to the creator (user) and the nested comments `db.posts.insertOne({title: "My first post", test: "This is my first post. I hope you like it.", tags: ["new", "tech"], creator: ObjectId("6237395059b36fb055c61e13")})`
5. Access the post object `db.posts.find()`

```javascript
{ _id: ObjectId("623739e859b36fb055c61e15"),
  title: 'My first post',
  test: 'This is my first post. I hope you like it.',
  tags: [ 'new', 'tech' ],
  creator: ObjectId("6237395059b36fb055c61e13"),
  comments:
   [ { text: 'I like this post',
       author: ObjectId("6237395059b36fb055c61e12") } ] }
```
