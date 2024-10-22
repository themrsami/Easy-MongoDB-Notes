# MongoDB CRUD Operations

## Table of Contents

1. [Overview](#overview)
2. [Key Concepts](#key-concepts)
3. [Create Operations](#create-operations)
   - [insertOne()](#insertone)
   - [insertMany()](#insertmany)
4. [Read Operations](#read-operations)
   - [find()](#find)
   - [findOne()](#findone)
   - [Projection](#projection)
   - [Sorting and Limiting](#sorting-and-limiting)
5. [Update Operations](#update-operations)
   - [updateOne()](#updateone)
   - [updateMany()](#updatemany)
   - [replaceOne()](#replaceone)
   - [Update Operators](#update-operators)
6. [Delete Operations](#delete-operations)
   - [deleteOne()](#deleteone)
   - [deleteMany()](#deletemany)
   - [Handling Deletion Results](#handling-deletion-results)
7. [Conclusion](#conclusion)
8. [Further Reading](#further-reading)

## Overview

CRUD operations stand for Create, Read, Update, and Delete. These are the fundamental operations for managing data in MongoDB. This document provides a detailed look at each operation, including syntax, examples, and best practices.

## Key Concepts

- **Database**: A container for collections; similar to a namespace for grouping related collections.
- **Collection**: A group of related documents, analogous to a table in relational databases.
- **Document**: The basic unit of data, stored in BSON format, which allows for rich data structures.

## Create Operations

To add new documents to a collection, you can use the following commands:

### insertOne()

Inserts a single document into a collection. It returns an acknowledgment with the inserted document's ID.

**Example**:
```javascript
const result = await db.users.insertOne({
  name: "Alice",
  age: 25,
  email: "alice@example.com"
});
console.log(`New user created with the following id: ${result.insertedId}`);
```

**Important Note**: If the document contains a field `_id`, MongoDB will use that value; otherwise, it will generate a new ObjectId.

### insertMany()

Inserts multiple documents into a collection. This is more efficient than inserting documents one by one.

**Example**:
```javascript
const result = await db.users.insertMany([
  { name: "Bob", age: 30, email: "bob@example.com" },
  { name: "Charlie", age: 35, email: "charlie@example.com" }
]);
console.log(`${result.insertedCount} new users created.`);
```

**Error Handling**: If one document fails to insert due to a duplicate key or other issues, the remaining documents will still be inserted.

## Read Operations

To retrieve documents from a collection, you can use the following commands:

### find()

Retrieves documents from a collection based on query parameters. You can filter results using query criteria and also apply sorting and limiting.

**Example**:
```javascript
const users = await db.users.find({ age: { $gte: 30 } }).toArray();
console.log(users);
```

### findOne()

Retrieves a single document that matches the query criteria. Returns the first match found.

**Example**:
```javascript
const user = await db.users.findOne({ name: "Alice" });
console.log(user);
```

### Projection

Limits the fields returned in the query. This is useful for retrieving only necessary data and optimizing performance.

**Example**:
```javascript
const users = await db.users.find({}, { projection: { name: 1, email: 1 } }).toArray();
console.log(users);
```

### Sorting and Limiting

You can sort the results and limit the number of documents returned.

**Example**:
```javascript
const users = await db.users.find()
  .sort({ age: -1 }) // Sort by age descending
  .limit(5)         // Limit to 5 results
  .toArray();
console.log(users);
```

## Update Operations

To modify existing documents in a collection, you can use the following commands:

### updateOne()

Updates a single document that matches the query criteria. You can use update operators to specify the changes.

**Example**:
```javascript
const result = await db.users.updateOne(
  { name: "Alice" },
  { $set: { age: 26 } }
);
console.log(`${result.modifiedCount} document(s) updated.`);
```

### updateMany()

Updates all documents that match the query criteria.

**Example**:
```javascript
const result = await db.users.updateMany(
  { age: { $gte: 30 } },
  { $set: { status: "senior" } }
);
console.log(`${result.modifiedCount} documents updated.`);
```

### replaceOne()

Replaces an entire document with a new document, rather than modifying specific fields.

**Example**:
```javascript
const result = await db.users.replaceOne(
  { name: "Bob" },
  { name: "Bob", age: 31, email: "bob.new@example.com" }
);
console.log(`${result.modifiedCount} document replaced.`);
```

### Update Operators

MongoDB provides a variety of update operators that allow you to modify documents efficiently.

- **$set**: Updates the value of a field.
- **$unset**: Removes a field from a document.
- **$inc**: Increments the value of a field by a specified amount.
- **$push**: Adds an item to an array.
- **$pull**: Removes an item from an array.

**Example**:
```javascript
await db.users.updateOne(
  { name: "Charlie" },
  { $inc: { age: 1 }, $push: { hobbies: "reading" } }
);
```

## Delete Operations

To remove documents from a collection, you can use the following commands:

### deleteOne()

Deletes a single document that matches the query criteria.

**Example**:
```javascript
const result = await db.users.deleteOne({ name: "Alice" });
console.log(`${result.deletedCount} document deleted.`);
```

### deleteMany()

Deletes all documents that match the query criteria.

**Example**:
```javascript
const result = await db.users.deleteMany({ age: { $lt: 30 } });
console.log(`${result.deletedCount} documents deleted.`);
```

### Handling Deletion Results

After a deletion operation, you can check how many documents were deleted by examining the `deletedCount` property in the result.

## Conclusion

Understanding CRUD operations in MongoDB is essential for managing and manipulating data effectively. This document provides the foundational knowledge needed to perform basic database operations and serves as a guide for more complex queries and data manipulations.

## Further Reading

- [MongoDB CRUD Operations Documentation](https://docs.mongodb.com/manual/crud/)
- [MongoDB Query and Projection](https://docs.mongodb.com/manual/tutorial/query-documents/)
- [MongoDB Update Operators](https://docs.mongodb.com/manual/reference/operator/update/)
- [MongoDB Indexing](https://docs.mongodb.com/manual/indexes/)