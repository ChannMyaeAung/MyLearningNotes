# MongoDB



## Databases 

```bash
test> show dbs  # list all the available databases
admin    40.00 KiB
config  108.00 KiB
local    40.00 KiB

test> use admin #switch the current database to admin
switched to db admin

admin> use school #switch the current databae to school
switched to db school

# school database is not showing up because it's empty
school> show dbs
admin    40.00 KiB
config  108.00 KiB
local    40.00 KiB

# creates a collection named `students` in the school databse
school> db.createCollection("students")
{ ok: 1 } #indicates that the op was successful

# drops the database
school> db.dropDatabase()
{ ok: 1, dropped: 'school' }

school> show dbs
admin    40.00 KiB
config  108.00 KiB
local    40.00 KiB
school>
```



## Insert

```bash
school> use school
already on db school

# inserting data
school> db.students.insertOne({name: "Spongebob", age: 30, gpa: 3.2})
{
  acknowledged: true,
  insertedId: ObjectId('671b8cb7d0fa16e59086b01d')
}

school> db.students.find()
[
  {
    _id: ObjectId('671b8cb7d0fa16e59086b01d'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2
  }
]

# inserting many data
school> db.students.insertMany([{name: "Patrick", age: 38, gpa: 1.5}, {name: "Sandy", age: 27, gpa: 4.0}, {name: "Gary", age: 18, gpa: 2.5}])
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('671b8d5cd0fa16e59086b01e'),
    '1': ObjectId('671b8d5cd0fa16e59086b01f'),
    '2': ObjectId('671b8d5cd0fa16e59086b020')
  }
}

# display all the data
school> db.students.find()
[
  {
    _id: ObjectId('671b8cb7d0fa16e59086b01d'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2
  },
  {
    _id: ObjectId('671b8d5cd0fa16e59086b01e'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5
  },
  {
    _id: ObjectId('671b8d5cd0fa16e59086b01f'),
    name: 'Sandy',
    age: 27,
    gpa: 4
  },
  {
    _id: ObjectId('671b8d5cd0fa16e59086b020'),
    name: 'Gary',
    age: 18,
    gpa: 2.5
  }
]
school>
```



## Data Types

```bash
school> db.students.insertOne({name:"Larry Lobster", age: 32, gpa: 2.8, fullTime: false, registerDate: new Date(), graduationDate: null, courses: ["Biology", "Chemistry", "Calculus"], address: {street: "123 Fake St.", city: "Bikini Bottom", zip: 12345}})
{
  acknowledged: true,
  insertedId: ObjectId('671b8fbcd0fa16e59086b021')
}
```

MongoDB supports a variety of data types. Here is a list of the main data types we can use in MongoDB:

1. **String**: Used to store text data. Example: `"Larry Lobster"`
2. **Integer**: Used to store numerical values. Example: `32`
3. **Double**: Used to store floating-point values. Example: `2.8`
4. **Boolean**: Used to store `true` or `false` values. Example: `false`
5. **Date**: Used to store date and time values. Example: `new Date()`
6. **Null**: Used to store a null value. Example: `null`
7. **Array**: Used to store arrays or lists of values. Example: `["Biology", "Chemistry", "Calculus"]`
8. **Embedded Document**: Used to store nested documents. Example: `{street: "123 Fake St.", city: "Bikini Bottom", zip: 12345}`
9. **ObjectId**: Used to store unique identifiers for documents. Example: `ObjectId('671b8fbcd0fa16e59086b021')`
10. **Binary Data**: Used to store binary data.
11. **Regular Expression**: Used to store regular expressions.
12. **JavaScript**: Used to store JavaScript code.
13. **JavaScript (with scope)**: Used to store JavaScript code with a scope.
14. **32-bit Integer**: Used to store 32-bit signed integers.
15. **64-bit Integer**: Used to store 64-bit signed integers.
16. **Decimal128**: Used to store high-precision decimal values.
17. **Min Key**: Used to compare a value against the lowest possible BSON element.
18. **Max Key**: Used to compare a value against the highest possible BSON element.

These data types allow MongoDB to store a wide variety of data in a flexible and efficient manner.

## Sorting & Limiting

### Sorting by Name

```bash
# 1 for alphabetical order
# -1 for reverse alphabetical order
school> db.students.find().sort({name:1})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4
  },
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2
  }
]

# reverse alphabetical order
school> db.students.find().sort({name:-1})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5
  }
]
```

### Sorting by GPA

```bash
# 1 for ascending order
# -1 for descending order
school> db.students.find().sort({gpa: 1})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  },
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4
  }
]

# descending order
school> db.students.find().sort({gpa:-1})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4
  },
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5
  }
]
```

### Limiting

```bash
# sorted by objectId by default
school> db.students.find().limit(1)
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2
  }
]
```

### Merging Sort & Limit

```bash
# highest gpa
school> db.students.find().sort({gpa:-1}).limit(1)
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4
  }
]

# lowest gpa
school> db.students.find().sort({gpa:1}).limit(1)
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5
  }
]
```



## Find

### Syntax

```
.find({query}, {projection})
```

```bash
school> db.students.find({name: "Spongebob"})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2
  }
]
# no returns due to typo
school> db.students.find({gap: 4.0})

school> db.students.find({gpa: 4.0})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4
  }
]
school> db.students.find({fullTime: false})
[
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  }
]

# no returns because there is none in our database that meets the specified condition
school> db.students.find({gpa: 4.0, fullTime: true})
```

#### projections (second argument of `find`)

```bash
# return every document but only give their names
# mongodb will give their id by default
school> db.students.find({}, {name: true}) # true or 1 can be used
[
  { _id: ObjectId('671b8de6af033f2f2946f4e9'), name: 'Spongebob' },
  { _id: ObjectId('671b8e75af033f2f2946f4eb'), name: 'Patrick' },
  { _id: ObjectId('671b8e75af033f2f2946f4ec'), name: 'Sandy' },
  { _id: ObjectId('671b8e75af033f2f2946f4ed'), name: 'Gary' },
  { _id: ObjectId('671b8fbcd0fa16e59086b021'), name: 'Larry Lobster' }
]

# we can set the id to be false if don't want it to be displayed
school> db.students.find({}, {_id: false, name: true})
[
  { name: 'Spongebob' },
  { name: 'Patrick' },
  { name: 'Sandy' },
  { name: 'Gary' },
  { name: 'Larry Lobster' }
]
```



## Update

#### Syntax

```bash
school> db.students.updateOne(filter, update)

# filter - selection criteria for the update
```

#### updateOne()

```bash
school> db.students.updateOne({name: "Spongebob"}, {$set: {fullTime: true}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
school> db.students.find({name: "Spongebob"})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: true
  }
]

# However, sometimes we can have people with the same name. 
# It is better to update with the _id.

school> db.students.updateOne({_id:ObjectId('671b8de6af033f2f2946f4e9')}, {$set:{fullTime: false}})   
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
school> db.students.find({_id: ObjectId('671b8de6af033f2f2946f4e9')})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  }
]

```



```bash
# unset operator to remove a field
# empty string to remove a field
school> db.students.updateOne({_id: ObjectId('671b8de6af033f2f2946f4e9')}, {$unset: {fullTime: ""}})  
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

school> db.students.find({_id: ObjectId('671b8de6af033f2f2946f4e9')})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2
  }
]
```



#### updateMany()

```bash
school> db.students.updateMany({}, {$set: {fullTime: false}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 5,
  modifiedCount: 4,
  upsertedCount: 0
}

school> db.students.find()
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  }
]
```

```bash
# remove fullTime fields from Gary and Sandy
school> db.students.updateOne({name: "Gary"}, {$unset: {fullTime:""}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
school> db.students.updateOne({name: "Sandy"}, {$unset: {fullTime:""}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

school> db.students.find()
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  }
]
```

```bash
# if a document doesn't have fullTime field, we'll give them one
school> db.students.updateMany({fullTime: {$exists: false}}, {$set:{fullTime: true}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 2,
  modifiedCount: 2,
  upsertedCount: 0
}

# now Sandy and Gary are full time students now
school> db.students.find()
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: true
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  }
]
```



## Delete

#### deleteOne()

```bash
school> db.students.deleteOne({name: "Larry Lobster"})
{ acknowledged: true, deletedCount: 1 }
school> db.students.find()
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: true
  }
]
```

#### deleteMany()

```bash
# deleting students who aren't full time students
school> db.students.deleteMany({fullTime:false})
{ acknowledged: true, deletedCount: 2 }
school> db.students.find()
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: true
  }
]

# deleting students who doesn't have registerDate
school> db.students.deleteMany({registerDate: {$exists: false}})
{ acknowledged: true, deletedCount: 2 }
school> db.students.find()

school>
# now our students db is empty
```



## Comparison operators

#### Keynotes

- Comparison Query Operators - Comparison operators return data based on value comparisons.

#### What if we want to find students other than "Spongebob"

```bash
# We can use not equal comparison operator
# precede our value with $ne:
school> db.students.find({name:{$ne: "Spongebob"}})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: true
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  }
]

# gives us four records
```

#### Less than , Less than or equal to

```bash
# age less than 20
# lt = less than
school> db.students.find({age:{$lt: 20}})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: true
  }
]

# less than or equal to (27 is inclusive in this case)
# lte = less than or equal to
school> db.students.find({age:{$lte: 27}})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: true
  }
]
```



#### Greater than, greater than or equal to

```bash
# gt = greater than
school> db.students.find({age:{$gt: 27}})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  }
]

# gte = greater than or equal to
school> db.students.find({age:{$gte: 27}})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  }
]
school>
```



```bash
# students that have a good gpa
school> db.students.find({gpa: {$gte: 3, $lte: 4 }})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  }
]
```



#### `in` , `nin` operator

```bash

# one of the values in `$in` array found in `name`.
school> db.students.find({name: {$in:["Spongebob", "Patrick", "Sandy"]}})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  }
]

# nin = not in
school> db.students.find({name: {$nin:["Spongebob", "Patrick", "Sandy"]}})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: true
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  }
]
```



## Logical operators

### Keynote

- **Logical Query Operators** - Logical operators return data based on expressions that evaluate to true or false.

#### $and

```bash
# fullTime AND age <= 22
school> db.students.find({$and: [{fullTime: true}, {age: {$lte: 22}}]})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: true
  }
]
```



#### $not

```bash
# we intentionally set Gary's age to null.
# traditionally, find method will ignore the null.
# what if somebody doesn't have info about their age?
school> db.students.find({age: {$lt: 30}})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  }
]

# when we use not operator, it will give me Gary's age even though it is null.
school> db.students.find({age: {$not:{$gte: 30}}})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: null,
    gpa: 2.5,
    fullTime: true
  }
]
```



#### $nor

```bash
# both conditions need to be false
# in this case, fullTime: true needs to be false, and age less than or equal to 22 needs to be false as well.

school> db.students.find({$nor: [{fullTime: true}, {age: {$lte: 22}}]})
[
  {
    _id: ObjectId('671b8de6af033f2f2946f4e9'),
    name: 'Spongebob',
    age: 30,
    gpa: 3.2,
    fullTime: false
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4eb'),
    name: 'Patrick',
    age: 38,
    gpa: 1.5,
    fullTime: false
  },
  {
    _id: ObjectId('671b8fbcd0fa16e59086b021'),
    name: 'Larry Lobster',
    age: 32,
    gpa: 2.8,
    fullTime: false,
    registerDate: ISODate('2024-10-25T12:31:56.573Z'),
    graduationDate: null,
    courses: [ 'Biology', 'Chemistry', 'Calculus' ],
    address: { street: '123 Fake St.', city: 'Bikini Bottom', zip: 12345 }
  }
]
```



#### $or

```bash
school> db.students.find({$or: [{fullTime: true}, {age: {$lte: 22}}]})
[
  {
    _id: ObjectId('671b8e75af033f2f2946f4ec'),
    name: 'Sandy',
    age: 27,
    gpa: 4,
    fullTime: true
  },
  {
    _id: ObjectId('671b8e75af033f2f2946f4ed'),
    name: 'Gary',
    age: 18,
    gpa: 2.5,
    fullTime: true
  }
]
```



## Indexes

### Keynote

- Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a **collection scan**, i.e. scan every document in a collection, to select those documents that match the query statement.
- If appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

```bash
# give me the execution stats of this query
school> db.students.find({name: "Larry Lobster"}).explain("executionStats")
{
  explainVersion: '1',
  queryPlanner: {
    namespace: 'school.students',
    parsedQuery: { name: { '$eq': 'Larry Lobster' } },
    indexFilterSet: false,
    queryHash: '544F3E5C',
    planCacheKey: 'B9363AF4',
    optimizationTimeMillis: 0,
    maxIndexedOrSolutionsReached: false,
    maxIndexedAndSolutionsReached: false,
    maxScansToExplodeReached: false,
    prunedSimilarIndexes: false,
    winningPlan: {
      isCached: false,
      stage: 'COLLSCAN',
      filter: { name: { '$eq': 'Larry Lobster' } },
      direction: 'forward'
    },
    rejectedPlans: []
  },
  executionStats: {
    executionSuccess: true,
    nReturned: 1,
    executionTimeMillis: 0,
    totalKeysExamined: 0,
    totalDocsExamined: 5, # examine 5 docs
    executionStages: {
      isCached: false,
      stage: 'COLLSCAN',
      filter: { name: { '$eq': 'Larry Lobster' } },
      nReturned: 1,
      executionTimeMillisEstimate: 0,
      works: 6,
      advanced: 1,
      needTime: 4,
      needYield: 0,
      saveState: 0,
      restoreState: 0,
      isEOF: 1,
      direction: 'forward',
      docsExamined: 5
    }
  },
  command: {
    find: 'students',
    filter: { name: 'Larry Lobster' },
    '$db': 'school'
  },
  serverInfo: {
    host: 'LAPTOP-DPIHL1TU',
    port: 27017,
    version: '8.0.1',
    gitVersion: 'fcbe67d668fff5a370e2d87b9b1f74bc11bb7b94'
  },
  serverParameters: {
    internalQueryFacetBufferSizeBytes: 104857600,
    internalQueryFacetMaxOutputDocSizeBytes: 104857600,
    internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
    internalDocumentSourceGroupMaxMemoryBytes: 104857600,
    internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
    internalQueryProhibitBlockingMergeOnMongoS: 0,
    internalQueryMaxAddToSetBytes: 104857600,
    internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600,
    internalQueryFrameworkControl: 'trySbeRestricted',
    internalQueryPlannerIgnoreIndexWithCollationForRegex: 1
  },
  ok: 1
}
```

- We have examined 5 documents to find "Larry Lobster".
- We are performing a linear search through our collections.
- Imagine having thousands of documents, linear search won't be optimal.

#### Applying Index

```bash
school> db.students.createIndex({name: 1})
name_1

school> db.students.find({name: "Larry Lobster"}).explain("executionStats")
{
  explainVersion: '1',
  queryPlanner: {
    namespace: 'school.students',
    parsedQuery: { name: { '$eq': 'Larry Lobster' } },
    indexFilterSet: false,
    queryHash: '544F3E5C',
    planCacheKey: 'EEE0759C',
    optimizationTimeMillis: 0,
    maxIndexedOrSolutionsReached: false,
    maxIndexedAndSolutionsReached: false,
    maxScansToExplodeReached: false,
    prunedSimilarIndexes: false,
    winningPlan: {
      isCached: false,
      stage: 'FETCH',
      inputStage: {
        stage: 'IXSCAN',
        keyPattern: { name: 1 },
        indexName: 'name_1',
        isMultiKey: false,
        multiKeyPaths: { name: [] },
        isUnique: false,
        isSparse: false,
        isPartial: false,
        indexVersion: 2,
        direction: 'forward',
        indexBounds: { name: [ '["Larry Lobster", "Larry Lobster"]' ] }
      }
    },
    rejectedPlans: []
  },
  executionStats: {
    executionSuccess: true,
    nReturned: 1,
    executionTimeMillis: 11,
    totalKeysExamined: 1,
    totalDocsExamined: 1, # examine 1 doc
    executionStages: {
      isCached: false,
      stage: 'FETCH',
      nReturned: 1,
      executionTimeMillisEstimate: 10,
      works: 2,
      advanced: 1,
      needTime: 0,
      needYield: 0,
      saveState: 0,
      restoreState: 0,
      isEOF: 1,
      docsExamined: 1,
      alreadyHasObj: 0,
      inputStage: {
        stage: 'IXSCAN',
        nReturned: 1,
        executionTimeMillisEstimate: 10,
        works: 2,
        advanced: 1,
        needTime: 0,
        needYield: 0,
        saveState: 0,
        restoreState: 0,
        isEOF: 1,
        keyPattern: { name: 1 },
        indexName: 'name_1',
        isMultiKey: false,
        multiKeyPaths: { name: [] },
        isUnique: false,
        isSparse: false,
        isPartial: false,
        indexVersion: 2,
        direction: 'forward',
        indexBounds: { name: [ '["Larry Lobster", "Larry Lobster"]' ] },
        keysExamined: 1,
        seeks: 1,
        dupsTested: 0,
        dupsDropped: 0
      }
    }
  },
  command: {
    find: 'students',
    filter: { name: 'Larry Lobster' },
    '$db': 'school'
  },
  serverInfo: {
    host: 'LAPTOP-DPIHL1TU',
    port: 27017,
    version: '8.0.1',
    gitVersion: 'fcbe67d668fff5a370e2d87b9b1f74bc11bb7b94'
  },
  serverParameters: {
    internalQueryFacetBufferSizeBytes: 104857600,
    internalQueryFacetMaxOutputDocSizeBytes: 104857600,
    internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
    internalDocumentSourceGroupMaxMemoryBytes: 104857600,
    internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
    internalQueryProhibitBlockingMergeOnMongoS: 0,
    internalQueryMaxAddToSetBytes: 104857600,
    internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600,
    internalQueryFrameworkControl: 'trySbeRestricted',
    internalQueryPlannerIgnoreIndexWithCollationForRegex: 1
  },
  ok: 1
}
school>
```

- This time we only examine only one document.

```bash
# there's already an index applied to object id by default.
# name_1 is the index we created.

school> db.students.getIndexes()
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { name: 1 }, name: 'name_1' }
]
school> db.students.dropIndex("name_1")
{ nIndexesWas: 2, ok: 1 }
school> db.students.getIndexes()
[ { v: 2, key: { _id: 1 }, name: '_id_' } ]
```



## Collections



```bash

school> show collections
students
school> db.createCollection("teachers", {capped: true, size: 10000000, max:100}, {autoIndexId: false})
{ ok: 1 }
school> show collections
students
teachers
```

- `capped: true`  - Indicates that the collection is a capped collection. Capped collections have a fixed size and maintain insertion order.
  - If capped is set to true, we do need to set the minimum size.
  - Capped collections are fixed-size collections that automatically overwrite the oldest documents when the specified size or document count limit is reached.
  - They are useful for scenarios where we need to maintain a rolling log of data, such as logging or caching.
- `size: 10000000` - Specifies the maximum size of the collection in bytes (10 MB in this case). (1000 * 1024).
- `max: 100` - Specifies the maximum number of documents the collection can hold. (In this case, we don't want any more than 100 teachers).
- `{autoIndexId: false}` - Used to specify whether to automatically create an index on the `_id` field. By default, MongoDB creates an index on the `_id` field.

```bash
school> db.createCollection("courses")
{ ok: 1 }

school> show collections
courses
students
teachers

# drop the collection
school> db.courses.drop()
true
school> show collections
students
teachers
```



## Connecting to a Next.js Application

- It's advised to refer to MongoDB documentation and Mongoose documentation.

### Steps

1. Create a cluster on MongoDB. Click "Connect" on the cluster created and copy the appropriate URL to connect to our next.js application.

2. In Network Access, we should define our Network Address.

3. After that, in our next.js application, inside the `app` directory, we need to create a folder called `lib`.

4. Inside the `lib` folder, we need to create `utils.ts`.

5. Inside the `utils.ts`, we are going to have the following code. Before that we need to store the URL we copied from our database in a `.env` file and we need to include `.env` to our `.gitignore`.

   - `utils.ts`

   - ```ts
     import mongoose from "mongoose";
     
     export const connectToDB = async () => {
       const connection: { isConnected?: number } = {};
       try {
         if (connection.isConnected) {
           return;
         }
         if (!process.env.MONGO) {
           throw new Error("MONGO environment variable is not defined");
         }
         const db = await mongoose.connect(process.env.MONGO);
         connection.isConnected = db.connection.readyState;
       } catch (error) {
         if (error instanceof Error) {
           throw new Error(error.message);
         } else {
           throw new Error("An unknown error occurred");
         }
       }
     };
     
     ```

   - then we also need to create `models.ts`

     ```ts
     import mongoose from "mongoose";
     
     // Defining the schema depending on our application
     const userSchema = new mongoose.Schema(
       {
         username: {
           type: String,
           required: true,
           unique: true,
           min: 3,
           max: 20,
         },
         email: {
           type: String,
           required: true,
           unique: true,
         },
         password: {
           type: String,
           required: true,
           min: 6,
           max: 20,
         },
         img: {
           type: String,
         },
         isAdmin: {
           type: Boolean,
           default: false,
         },
         isActive: {
           type: Boolean,
           default: true,
         },
         mobile: {
           type: String,
         },
         address: {
           type: String,
         },
       },
       { timestamps: true }
     );
     const productSchema = new mongoose.Schema(
       {
         title: {
           type: String,
           required: true,
           unique: true,
         },
         desc: {
           type: String,
           required: true,
         },
         price: {
           type: Number,
           required: true,
           min: 0,
         },
         stock: {
           type: Number,
           required: true,
           min: 0,
         },
         img: {
           type: String,
         },
         color: {
           type: String,
         },
         size: {
           type: String,
         },
       },
       { timestamps: true }
     );
     
     // If there's an existing database, don't create a new one.
     // If not, create a new schema.
     export const User = mongoose.models.User || mongoose.model("User", userSchema);
     export const Product =
       mongoose.models.Product || mongoose.model("Product", productSchema);
     
     ```

   - then finally we'll need to create `data.ts`.

     ```ts
     import { User } from "./models";
     import { connectToDB } from "./utils";
     
     export const fetchUsers = async () => {
       try {
         connectToDB();
         const users = await User.find();
         return users;
       } catch (err) {
         console.log(err);
         throw new Error("Failed to fetch users!");
       }
     };
     
     ```

     
