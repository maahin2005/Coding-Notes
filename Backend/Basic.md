## BASIC Terminal Command:

### 1. For Showing Dbs:

    - show dbs

### 2. For create any db or use any existing db:

    - use <db name/>
        - (if db is already there so it take that, otherwise create it)
        - ex. use library

### 3. How to check collections of any database?

    - show collections

### 4. How to create a collections?

    - db.createCollection("<Name-of-collection>")

### 5. How to create or Insert one docs inside collection?

    - db.<collection-name>.insertOne({<Enter data in Object format>})
    - ex.
        - db.fictional.insertOne({name:"game of thrones",author:"superman",price:100})
        - db.Loki.insertOne({name:"Loki s2", hero:"Loki", villain:"who he remains", year:"2022"})

### 6. How to check docs of collections?

    - db.<collection-name>.find()
    - Ex.
        - db.Loki.find()

### 7. How to insert many docs?

    - db.<collection-name>.insertMany([<multiple data in objects form>])
    - Ex.
        - db.thor.insertMany([{name:"thor: Love and thunder", year:2019, hero:"Thor"},{name:"Thor ragnarokor",year:2021,hero:"Thor"}])

### 8. filtering:

    - inside find's paranthesis enter whatever you want to find and do filter
    - ex. => db.fictional.find({name:"harry potter"})

### 9. updation:

    - must use $set
    - update one:
        -  db.fictional.updateOne({name:"game of thrones"}, {$set:{author:"Super Man"}})
    - update many:
        - db.fictional.updateMany({<Indetifier>}, {$set:{Enter new data}})
    - success output:
        - {

            acknowledged: true,
            insertedId: null,
            matchedCount: 1,
            modifiedCount: 1,
            upsertedCount: 0
         }

### 10. deletion:

    - db.fictional.deleteOne({<any identifier>})
        - db.thor.deleteOne({name:'Thor ragnarokor'})
        - success output: { acknowledged: true, deletedCount: 1 }

    - db.fictional.deleteMany({<any identifier>})
