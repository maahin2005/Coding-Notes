## Aggregation => MongoDB framework

### Syntex: db.collectionName.aggregate([])

#### db.collectionName.aggregate([]) => initiates pipelines aggregation on the given collection.

- Aggregation Pipeline:

  - This is a series of stages that process documents. Each stage performs an operation on the input documents and passes the results to the next stage.
  - simple and for interview: Pipeline in MongoDB is a series of stages that process and transform data. Each stage takes the output of the previous stage as its input, allowing for complex data transformations and aggregations.

  - Common stages include:

    - $match: Filters the documents to pass only those that match the specified condition.
    - $group: Groups documents by a specified expression and applies accumulators, such as sum, avg, min, max, etc., to each group.
    - $project: Reshapes each document in the stream, adding, removing, or modifying fields.
    - $sort: Sorts the documents by a specified field.
    - $limit: Limits the number of documents passed to the next stage.
    - $skip: Skips over a specified number of documents.
    - $unwind: Deconstructs an array field from the input documents to output a document for each element.

- #### Aggregation in MongoDB is a method to process data records and return combined results.
- #### It involves operations like filtering, grouping, and sorting data to produce a summary or computed result.

### In the array of aggregate, we enter operation objects separated by commas, allowing as many operations as we want.

    - Ex: `db.<collectionName>.aggregate( [ { }, { }, { } ] )`
    - The output of the first query will be the input for the second query, and the same applies for the second, third, and so on until the end of the process.
    - first query also we can called as stage 1 and so on...

## $match:

    - Similar to find() => filter the data
    - can we use find ? if no why ? bcoz we can't perfom like => db.<collectionName>.find().update()
        - inshort we are not able to combined the queries with find()

    - Ex: `db.LecturePracticeI.aggregate( [ { $match: {first_name: "Jarret" } } ] )`
    - Ex: `db.LecturePracticeI.aggregate( [ { $match: { $and: [ { age: { $gte: 18 } }, { age: { $lte: 20 } } ] } } ] )`

## $project:

    - shape and transform document according to our specification
    - select any field
    - we can rename a field
    - compute certain field

1. select any field: Get only specific field from collection:

   - `db.LecturePracticeI.aggregate( [{$project:{email:1,age:1}}] )`

     - 1 => which field do you want to see.
     - 0 => which field you don't want to see.
     - do use either 1 or 0.

   - `db.LecturePracticeI.aggregate( [ { $project:{ salaryPerMonth: { $divide: [ "$salary", 12 ] }, name: { $concat: [ "$first_name", " ", "$last_name" ] } } } ] )`

   - `db.LecturePracticeI.aggregate( [ { $project: { salaryPerMonth: { $divide: [ "$salary", 12 ]   } } } ] )`

   - `db.LecturePracticeI.aggregate( [ { $match: { $and: [ { age: { $gte: 18 } }, { age: { $lte: 30 } } ] } }, { $project: { Name: { $concat: [ "$first_name", " " , "$last_name" ] }, age:1 } }, { $sort: { age: -1 } } ] )`

### The Aggregate operation in MongoDB does not change the real backend data. It processes and transforms data to produce computed results without altering the original documents in the database.
