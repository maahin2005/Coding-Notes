## Operators Command:

## Comparison Operators:

### <= : lte (less then and equal to)

### >= : gte (greater then and equal to)

### < : lt (less then)

### > : gt (greater then)

### == : eq (equal to)

### != : ne (not equal to)

- Example:
  - db.fictional.find({price:{$gte:100}})

## IMP NOTE: Don't forget to use `$` sign before using operators

    - ex. $lt, $gt, $eq

## Logical Operators:

### && : and

### || : or

- Example:
  - db.LecturePracticeI.find({$and:[{gender:"Female"}, {age:{$lte:25}}]})

### skip and limit:

    - db.LecturePracticeI.find().skip(<any int value>).limit(<any int value>)
    - useful for pagination

### sort:

    - db.LecturePracticeI.find().sort({key:value})
    - key: (on which basis want to sort/filter/identifier)
    - value: (either 1 or -1)
    - db.LecturePracticeI.find().sort({age:1})
    - db.LecturePracticeI.find().sort({age:-1})
    - ascending order: 1
    - descending order: -1
