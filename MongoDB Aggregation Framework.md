# MongoDB Aggregation Framework

## Table of Contents

1. [Overview](#overview)
2. [Key Concepts](#key-concepts)
3. [Aggregation Pipeline](#aggregation-pipeline)
4. [Stages of the Aggregation Pipeline](#stages-of-the-aggregation-pipeline)
   - [match](#match)
   - [group](#group)
   - [project](#project)
   - [sort](#sort)
   - [limit](#limit)
   - [skip](#skip)
   - [lookup](#lookup)
5. [Common Operators](#common-operators)
6. [Examples](#examples)
   - [Example 1: Basic Aggregation](#example-1-basic-aggregation)
   - [Example 2: Grouping and Summarizing](#example-2-grouping-and-summarizing)
   - [Example 3: Joining Collections](#example-3-joining-collections)
7. [Conclusion](#conclusion)
8. [Further Reading](#further-reading)

## Overview

The MongoDB Aggregation Framework provides powerful capabilities for processing and analyzing data within collections. It allows you to perform complex queries, transformations, and calculations on your data.

## Key Concepts

- **Aggregation**: The process of performing operations on data to return computed results.
- **Aggregation Pipeline**: A framework that consists of a series of stages that process documents and transform them into aggregated results.

## Aggregation Pipeline

The aggregation pipeline consists of multiple stages, where each stage transforms the data as it passes through. The output of one stage becomes the input to the next stage.

## Stages of the Aggregation Pipeline

### match

- **Purpose**: Filters documents to pass only those that match the specified criteria.
- **Usage**: Similar to the `find()` method.

**Example**:
```javascript
{ $match: { status: "active" } }
```

### group

- **Purpose**: Groups documents by a specified identifier and allows for aggregation operations like sum, average, count, etc.
- **Usage**: Essential for summarizing data.

**Example**:
```javascript
{ 
  $group: { 
    _id: "$category", 
    totalSales: { $sum: "$sales" }
  }
}
```

### project

- **Purpose**: Reshapes each document in the stream by adding new fields or removing existing ones.
- **Usage**: Useful for controlling which fields are included in the output.

**Example**:
```javascript
{ 
  $project: { 
    name: 1, 
    totalSales: { $multiply: ["$price", "$quantity"] }
  }
}
```

### sort

- **Purpose**: Sorts the documents based on specified fields.
- **Usage**: Useful for ordering results.

**Example**:
```javascript
{ $sort: { totalSales: -1 } } // Sort by totalSales in descending order
```

### limit

- **Purpose**: Restricts the number of documents passed to the next stage.
- **Usage**: Useful for pagination or when only a subset of results is needed.

**Example**:
```javascript
{ $limit: 10 }
```

### skip

- **Purpose**: Skips a specified number of documents before passing the remaining documents to the next stage.
- **Usage**: Commonly used with `limit` for pagination.

**Example**:
```javascript
{ $skip: 5 }
```

### lookup

- **Purpose**: Joins documents from another collection into the pipeline.
- **Usage**: Enables relational-like queries in MongoDB.

**Example**:
```javascript
{ 
  $lookup: {
    from: "products",
    localField: "productId",
    foreignField: "_id",
    as: "productDetails"
  }
}
```

## Common Operators

- **$sum**: Calculates the sum of numeric values.
- **$avg**: Calculates the average of numeric values.
- **$count**: Counts the number of documents.
- **$max**: Finds the maximum value.
- **$min**: Finds the minimum value.
- **$push**: Creates an array of values.
- **$addToSet**: Creates an array of unique values.

## Examples

### Example 1: Basic Aggregation

To count the number of active users in a collection:

```javascript
db.users.aggregate([
  { $match: { status: "active" } },
  { $count: "activeUserCount" }
]);
```

### Example 2: Grouping and Summarizing

To get total sales by category:

```javascript
db.sales.aggregate([
  { $group: { _id: "$category", totalSales: { $sum: "$amount" } } },
  { $sort: { totalSales: -1 } }
]);
```

### Example 3: Joining Collections

To join sales data with product details:

```javascript
db.sales.aggregate([
  { 
    $lookup: {
      from: "products",
      localField: "productId",
      foreignField: "_id",
      as: "productDetails"
    }
  },
  { $unwind: "$productDetails" },
  { 
    $project: {
      productName: "$productDetails.name",
      amount: 1
    }
  }
]);
```

## Conclusion

The Aggregation Framework in MongoDB is a powerful tool for performing complex data analysis and transformations. By utilizing various stages and operators, you can extract valuable insights from your data efficiently.

## Further Reading

- [MongoDB Aggregation Documentation](https://docs.mongodb.com/manual/aggregation/)
- [Aggregation Pipeline Stages](https://docs.mongodb.com/manual/core/aggregation-pipeline/)
- [Using the Aggregation Framework](https://www.mongodb.com/docs/manual/aggregation/)