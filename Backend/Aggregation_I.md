## Aggregation => MongoDB framework

### Syntex: db.collectionName.aggregate([])

#### db.collectionName.aggregate([]) => initiates pipelines aggregation on the given collection.

- Aggregation Pipeline:

  - This is a series of stages that process documents. Each stage performs an operation on the input documents and passes the results to the next stage.
  - simple and for interview: Pipeline in MongoDB is a series of stages that process and transform data. Each stage takes the output of the previous stage as its input, allowing for complex data transformations and aggregations.

  - Common stages include:

    - $match: Filters the documents to pass only those that match the specified condition.
    - $project: Reshapes each document in the stream, adding, removing, or modifying fields.
    - $group: Groups documents by a specified expression and applies accumulators, such as sum, avg, min, max, etc., to each group.
    - $sort: Sorts the documents by a specified field.
    - $limit: Limits the number of documents passed to the next stage.
    - $skip: Skips over a specified number of documents.
    - $unwind: Deconstructs an array field from the input documents to output a document for each element.

- #### Aggregation in MongoDB is a method to process data records and return combined results.
- #### It involves operations like filtering, grouping, and sorting data to produce a summary or computed result.

### In the array of aggregate, we enter operation objects separated by commas, allowing as many operations as we want.

    - Ex: `db.collectionName.aggregate( [ { }, { }, { } ] )`
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

#### select any field: Get only specific field from collection:

- `db.LecturePracticeI.aggregate( [{$project:{email:1,age:1}}] )`

  - 1 => which field do you want to see.
  - 0 => which field you don't want to see.
  - do use either 1 or 0.

- `db.LecturePracticeI.aggregate( [ { $project:{ salaryPerMonth: { $divide: [ "$salary", 12 ] }, name: { $concat: [ "$first_name", " ", "$last_name" ] } } } ] )`

- `db.LecturePracticeI.aggregate( [ { $project: { salaryPerMonth: { $divide: [ "$salary", 12 ]   } } } ] )`

- `db.LecturePracticeI.aggregate( [ { $match: { $and: [ { age: { $gte: 18 } }, { age: { $lte: 30 } } ] } }, { $project: { Name: { $concat: [ "$first_name", " " , "$last_name" ] }, age:1 } }, { $sort: { age: -1 } } ] )`

## $group:

    - Combined and groups the documents based on specific criteria.
    - Allows performing Aggregation operations like sum, count, average, etc...
    - Useful for generating statistics or analyzing data.

    - Ex: `db.LecturePracticeI.aggregate( [{$group:{_id: "$gender"}}] )`
        - Ans: [
              { _id: 'Male' },
              { _id: 'Bigender' },
              { _id: 'Genderqueer' },
              { _id: 'Agender' },
              { _id: 'Genderfluid' },
              { _id: 'Non-binary' },
              { _id: 'Female' }
            ]
    - why this `_id` ?
        1. Grouping criteria
        2. unique Identifier
        3. output format

1.  count users in all groups which are devided by gender

    - query: `db.LecturePracticeI.aggregate([{$group:{_id:"$gender", userCount:{$count:{}}}}])`
    - ans: [
      { _id: 'Genderfluid', userCount: 2 },
      { _id: 'Genderqueer', userCount: 2 },
      { _id: 'Bigender', userCount: 6 },
      { _id: 'Male', userCount: 47 },
      { _id: 'Agender', userCount: 2 },
      { _id: 'Female', userCount: 40 },
      { _id: 'Non-binary', userCount: 1 }
      ]

2.  find the average salery of all groups

    - query: ` db.LecturePracticeI.aggregate([{$group:{_id:"$gender",avgSalary:{$avg:"$salary"}}}])`
    - ans: [
      { _id: 'Bigender', avgSalary: 28727.833333333332 },
      { _id: 'Female', avgSalary: 26498.35 },
      { _id: 'Genderfluid', avgSalary: 29632.5 },
      { _id: 'Non-binary', avgSalary: 48100 },
      { _id: 'Genderqueer', avgSalary: 21122.5 },
      { _id: 'Agender', avgSalary: 26953 },
      { _id: 'Male', avgSalary: 26904.08510638298 }
      ]

3.  find the average of age of all the users (Not according to any group or criteria)

    - query: db.LecturePracticeI.aggregate([{$group:{_id: 1, avgAge:{$avg:"$age"},totalUsers:{$sum:1}}}])
    - ans. [ { _id: 1, avgAge: 38.32, totalUsers: 100 } ]

4.  make a group of all users with their first_name and age

    - query:
      - arry form: `db.LecturePracticeI.aggregate([{$group:{_id:["$first_name","$age"]}}])`
      - obj form: `db.LecturePracticeI.aggregate([{$group:{_id:{name:"$first_name", age:"$age"}}}])`
    - ans: [
      { \_id: { name: 'Maura', age: 42 } },
      { \_id: { name: 'Isabeau', age: 28 } },
      { \_id: { name: 'Anthea', age: 23 } },
      { \_id: { name: 'Ardenia', age: 30 } },
      { \_id: { name: 'Fidelity', age: 33 } },
      { \_id: { name: 'Marcos', age: 42 } },
      { \_id: { name: 'Beverie', age: 36 } },
      { \_id: { name: 'Kerry', age: 24 } },
      { \_id: { name: 'Sylvan', age: 20 } },
      { \_id: { name: 'Dorice', age: 28 } },
      { \_id: { name: 'Sid', age: 37 } },
      { \_id: { name: 'Esme', age: 22 } },
      { \_id: { name: 'Bianca', age: 14 } },
      { \_id: { name: 'Gordy', age: 56 } },
      { \_id: { name: 'Gerard', age: 39 } },
      { \_id: { name: 'Bobbie', age: 37 } },
      { \_id: { name: 'Esra', age: 62 } },
      { \_id: { name: 'Nannette', age: 60 } },
      { \_id: { name: 'Antonia', age: 59 } },
      { \_id: { name: 'Jorge', age: 54 } }

      ]

5.  get the unique gender of those people who are above the age or equal to 50 and their avg salary

        - query: `db.LecturePracticeI.aggregate([{$match:{age:{$gte:50}}},{$group:{_id: "$gender",avgSalary:{$avg:"$salary"}}}])`
        - ans: `[

    { \_id: 'Male', avgSalary: 24602.6 },
    { \_id: 'Female', avgSalary: 25425.7 }
    ]`

### The Aggregate operation in MongoDB does not change the real backend data. It processes and transforms data to produce computed results without altering the original documents in the database.
