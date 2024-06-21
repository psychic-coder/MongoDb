1)mongodb stores data in the form of the documents in the json format
which makes it easy to fetch and use it

2)mongodb is schemaless

3)we write the date in mognodb in the form of json but its stored in the form of bson,

<!-- -------------------------------------------------------------------------------- -->

Commands

wherever we have used these "<>" we'll replace the entire including the arrows with the name

cmd(it shows the databases already present in the terminal)-->"show dbs"
cmd(it is used to create a database)--->"use <database-name>" // we can also use this command to go another database
cmd("it is used to delete a database)-->"db.dropDatabase()"
cmd(to see the collections in the database)-->"show collections"
cmd(its used to create a collection)-->"db.createCollection("<collection-name>")"
cmd(its used to delete a collection)-->"db.<collection-name>.drop()"
cmd(its used to insert document single docs in mongodb)-->"db.<collection-name>.insertOne({field1:value1,field2:value2,...})"
cmd(its used to insert document multiple docs in mongodb)-->"db.<collection-name>.insertMany([
{field1:value1 ,field2:value2,...},
{field1:value1 ,field2:value2,...},
...
])"

<!-- -------------------------------------------------------------------------------- -->

cmd("its used to read)-->"db.<collection-name>.find()"

"ordered-inserts"-->in this the mongodb stops on the first error while we are inserting multiple datas in a collection , basically our mongodb will stop after we face the error , and will stop uploading the data present after the error data

cmd("to make the insertion process unordered in the database")-->"db.<collection-name>.insertMany([doc1,doc2,...],{ordered:false})"

"Field names and collection names in databases are case-sensitive"

cmd("it is used to search the databases on the basis of a single parameter and gives a single persons data on the basis of the parameter")-->"db.collection_name.findOne({key:value})"

cmd("it is used to get all the data on the basis of a single parameter")-->"db.collection_name.find({key:value})"

cmd("to import json in mongodb , if the json objects are not inside an array ")-->"mongoimport jsonfile.json -d database_name -c collection_name"

cmd("to import json in mongodb , if the json objects are present inside a single array,products.json is the name of the file ")-->"mongoimport products.json -d database_name -c collection_name --jsonArray"

example--->" mongoimport E:\mongo\mongo_json\products.json -d shop -c products"

<!-- -------------------------------------------------------------------------------- -->

comparison operators
cmd("how to use the operators")-->"db.collection_name.find({"fieldname":{$operator:value}})"
operators:-->$eq,$ne,$gt,$gte,$lt,$lte,$in,$nin
$eq: Equal,
$ne: Not Equal,
$gt: Greater Than,
$gte: Greater Than or Equal,
$lt: Less Than,
$lte: Less Than or Equal,

"$in: In,
$nin: Not In
Selects documents where the value of the field is in the specified array.
db.find({ field: { $in: [ value1, value2, ... ] } })"
example for -->db.collection_name.find({price:{$in:[234,344,23]}})
the above gives output the results where the price is equal to the value mentioned in the array

<!-- -------------------------------------------------------------------------------- -->

cursors -->these are used to retrieve large result sets from queries,mongodb retrieves query results in batches using cursors
cursor methods are:- count(),limit(),skip(),sort()
count-> gives the total number of data we received
limit->it is used to get the number of data we want to receive
skip->it is used to skip the number of datas we don't want to receive from the starting
sort->in sort's example we're sorting the received data on the basis of the field price , the value (1) means ascending and the value (-1) means descending

db.collection_name.find({"fieldname":{$operator:value}}).count()
db.collection_name.find({"fieldname":{$operator:value}}).limit(5)
db.collection_name.find({"fieldname":{$operator:value}}).limit(5).skip(2)
db.collection_name.find({"price":{$gte:1250}}).limit(5).sort({"price":1})

CAUTION---
Be cautious when using limit() and skip() on large collections as it may hamper the performance ,
Consider using indexing to optimize query performance

<!-- -------------------------------------------------------------------------------- -->

Logical Operators:--
$and,$or,$not,$nor

$and->performs a logical And operation on an array of expressions where all expressions must be true for document to match
ex->db.products.find({$and:[{"price":{$gt:100}},{"name":"diamond ring"}]})
we can also write the above as below,as when we provide multiple fields within a single query document , MongoDb treats them as implicit AND operation
db.products.find({"price":{$gt:0}},{"name":"diamond ring"})

$or-->db.products.find({$or:[{"price":{$gt:100}},{"name":"diamond ring"}]})
$nor-->db.products.find({$nor:[{"price":{$gt:100}},{"name":"diamond ring"}]})
$not-->performs a logical NOT operation on the specified expressions inverting the result 
db.products.find({"price":{$not:{$eq:100}}})

{$and:[{condition1},{condition2},....]}
{$or:[{condition1},{condition2},....]}
{$nor:[{condition1},{condition2},....]}
{field:{$not:{operator:value}}}

<!-- -------------------------------------------------------------------------------- -->

Arithmetic Expression Operators
$abs: Returns the absolute value of a number.
$add: Adds numbers together.
$ceil: Rounds a number up to the nearest integer.
$divide: Divides one number by another.
$exp: Raises e to the specified exponent.
$floor: Rounds a number down to the nearest integer.
$ln: Computes the natural logarithm.
$log: Computes the logarithm of a number.
$log10: Computes the base-10 logarithm.
$mod: Computes the remainder of a division operation.
$multiply: Multiplies numbers together.
$pow: Raises a number to a specified exponent.
$round: Rounds a number to a specified place.
$sqrt: Computes the square root.
$subtract: Subtracts one number from another.
$trunc: Truncates a number to a specified place.

<!-- -------------------------------------------------------------------------------- -->

Complex Expressions
$expr operator 
{$expr:{operator:[field:value]}}
we use dollar before the price so the field name as in this case is price
db.products.find({$expr:{$gt:["$price",1340]}})

in the below query we're returning the fields where the multiplication of quantity and price is greater then the targetprice
db.sales.find({$expr:{$gt:[{$multiply:["$quantity","$price"]},"$targetPrice"]}})

<!-- -------------------------------------------------------------------------------- -->

Elements Operator
$exists,$type,$size
{field:{$exists:<boolean>}}
{field:{$type:"<bson-data-type>"}}
{field:{$size:<array-length>}}

we're searching for the datas where price field exists and the price should be greater then 1250
db.products.find({"price":{$exists:true},"price":{$gt:1250}}).count()

$type:the $type operator filter documents on the basis of a BSON data type of a field.
the <BSONType> value is the following:-
1:Double
2:String
3:Object
4:Array
5:Binary data
6:Undefined (deprecated)
7:ObjectId
8:Boolean
9:Date
10:Null
11:Regular expression
12:JavaScript
13:Symbol (deprecated)
14:JavaScript (with scope)
15:32-bit integer
16:Timestamp
17:64-bit integer
18:Decimal128
19:Min key
20:Max key

//we're searching for the price field where the type is a number
ex--> db.products.find({price:{$type:"number"}}).count()

//we can even use the number for search which is infront of the particular type for the searching purpose
//as in this case we wrote 8 which was ahead of the type boolean
ex--> db.products.find({"isFeatured":{$type:8}}).count()

$size:this operator matches the docs where the size of an array field matches a specified value

in the below example--> in the comments collection we're checking each data where the comments array field has a size of 2
db.comments.find({"comments":{$size:2}})

<!-- -------------------------------------------------------------------------------- -->

Projection
To include specific fields ,use projection with a value of 1 for the fields you want
To exclude fields , use Projection with a value of 0, for the fields you want to exclude .
You cannot include and exclude fields simultaneously in the same query projection

ex-->db.collection.find({},{field1:1,field2:1})

//in the below code we're fetching all the comments arrays fields from the data where the comments array has a size of 2
db.comments.find({"comments":{$size:2}},{comments:1})

<!-- another example we're we excluded the _id -->

db.comments.find({"comments":{$size:2}},{comments:1,\_id:0})

<!-- -------------------------------------------------------------------------------- -->

Embedded Docs

query documents inside embedded docs using dot notation

db.collection.find({"parent.child":value})
ex-

the user inside comments was an object inside an array
db.comments.find({"comments.user":"Lily"})

the views field inside the metadata is an object
db.comments.find({"metadata.views":{$gt:1200}})

<!-- -------------------------------------------------------------------------------- -->

$all
it is an operator selects the documents where the value of a field of a is an array that contains all the specified elements
{<field>:{$all:[<value1>,<value2>,...]}}
ex-
//both binod and alice have to be present in the user field of comments array then only this is true
db.comments.find({"comments.user":{$all:["alice","binod"]}})

$elemMatch
It is used to match documents that contain an array field with at least one element that matches all the specified query criteria.
{<field>:{$elemMatch:{<query1>,<query2>,...}}}
ex--->both the user field and the text field have should be true for each element of the comment array
db.comments.find({"comments":{$elemMatch:{"user":"Vinod","text":"hello"}}})

<!-- -------------------------------------------------------------------------------- -->

Update operations in MongoDb

UpdateOne and UpdateMany()
db.collectionName.updateOne({filter},{$set:{existingField:"new value",newField:"new value"}})
ex->
//adding a new field
db.collectionName.updateOne({"_id":32134},{$set:{existingField:"newValue",newField:"new value"}})

//adding a new field
db.collectionName.updateMany({filter},{$set:{existingField:"new value",newField:"new value"}})

Removing and Renaming the fields :---
//deleting a field
db.collectionName.updateOne({filter},{$unset:{existingField:"new value"}})
//renaming a field
db.collectionName.updateOne({filter},{$rename:{existingField:"new value"}})

Updating arrays and embedded documents:---

db.collectionName.updateOne({filter},{$push:{"arrayField":"newElement"}})
ex->db.collectionName.updateOne({"_id":5},{$push:{"comments":{"user":"aron","text":"subscribe"}}})

db.collectionName.updateOne({filter},{$pop:{"arrayField":"newElement"}})
ex-->db.collectionName.updateOne({"_id":5},{$pop:{"comments":1}})

db.collectionName.updateOne({filter},{$set:{"arrayField.$.text":"newElement"}})
ex-->db.collectionName.updateOne({\_id:7,"comments.user":"Alice"},{$set:{"comments.$.text":"Awesome Thapa"}})

<!-- -------------------------------------------------------------------------------- -->

Delete Operations in MongoDb

db.collectionName.deleteOne({filter});
ex:-->db.sales.deleteOne({\_id:1})

db.collectionName.deleteMany({field:"value"});
ex:-->db.sales.deleteMany({"price":55})

<!-- -------------------------------------------------------------------------------- -->

Indexes
-->specialized data structure that optimize data retrieval speed in mongoDB
-->enable MongoDB to locate data daster during queries

explain()
its used to understand the query execution in detail
it returns all the details we require to excute a query
ex:-
its used to measure the time taken to execute a query
db.products.find({"name":"air fryer"}).explain("executionStats")

Managing Indexes:------
//for setting the index
db.products.createIndex({field:1})
ex:---
db.products.createIndex({field:1})
db.products.createIndex({"name":1})
(1) for storing the indexes in ascending order
(-1) for storing the indexes in descending order

//to get the index
db.collection.getIndexes()
\_id is a default index

//for dropping the index
db.collection.dropIndex({field:1})
db.collection.dropIndex({"name":1})

<!-- -------------------------------------------------------------------------------- -->

Aggregation in MongoDB
aggregation:-- process of transforming and combining multiple documents into computed results
pipeline stages:-- aggregation consists of multiple stages , each performing a specific operation to the input data

$match -->its similar to the query as the first argument in .find() . It filters documents based on specified conditions .
ex-->
db.products.aggregate([{$match:{company:"Samsung"}}])
db.products.aggregate([{$match:{"price":{$gt:50}}}])

<!-- -------------------------------------------------------------------------------- -->

$group
it groups docs by the specified fields and performs aggregate operations on grouped data

db.products.aggregate([
{
$group:{
_id:{comp:"$company"},<!--groupkey -->
<field1>:{<accumulator>:<expression1>},
...
}
}
])

//when dealing with $group we neet to pass $sign for our existing field not the one we are going to create, in the below example company is the existing field,totalProducts is the new field created and it will give the summation value
db.products.aggregate([
    {
        $group:{_id:{comp:"$company"},totalProducts:{$sum:1}}
}
])

$sum: Calculates the sum of numeric values. It can also count documents if used with a value of 1.
{ $sum: <expression> }
$avg: Computes the average of numeric values.

$avg: Computes the average of numeric values.
{ $avg: <expression> }
$min: Finds the minimum value.

$min: Finds the minimum value.
{ $min: <expression> }
$max: Finds the maximum value.

$max: Finds the maximum value.
{ $max: <expression> }
$first: Returns the first value in a group.

$first: Returns the first value in a group.
{ $first: <expression> }
$last: Returns the last value in a group.

$last: Returns the last value in a group.
{ $last: <expression> }
$push: Adds values to an array.

$push: Adds values to an array.
{ $push: <expression> }
$addToSet: Adds unique values to an array (no duplicates).

$addToSet: Adds unique values to an array (no duplicates).
{ $addToSet: <expression> }
$stdDevPop: Calculates the population standard deviation of numeric values.

$stdDevPop: Calculates the population standard deviation of numeric values.
{ $stdDevPop: <expression> }
$stdDevSamp: Calculates the sample standard deviation of numeric values.

$stdDevSamp: Calculates the sample standard deviation of numeric values.
{ $stdDevSamp: <expression> }
$mergeObjects: Combines multiple documents into a single document.

$mergeObjects: Combines multiple documents into a single document.
{ $mergeObjects: <expression> }
$accumulator: Allows for custom accumulator expressions.

$accumulator: Allows for custom accumulator expressions.
{ $accumulator: { init: <function>, accumulate: <function>, accumulateArgs: <array>, merge: <function>, finalize: <function>, lang: <string> } }

ex1-->
the below code will sum the price fields value of all the docs where we have the company present
db.products.aggregate([
{
$group:{
_id:"$company",
totalProducts:{$sum:"$price"},
}
}
])

ex2-->
price should be greater then 900, we have to group data on the basis of the same company, and we have to sum the prices field value

db.products.aggregate([
{
$match:{price:{$gt:900}}
},
{
$group:{
_id:"$company",
totalProducts:{$sum:"$price"},
}
}
])
ex3-->quantity should be equal to 5 , we have to group data on the basis of the same quantity, and we have to sum the prices field value

db.products.aggregate([
{
$match:{quantity:5}
},
{
$group:{
_id:"$quantity",
priceTotal:{$sum:"$price"},
priceAvg:{$avg:"$price"}
}
}
])

<!-- -------------------------------------------------------------------------------- -->

$sort method in aggregation

//in sort we can sort the data we receive after the group method
//in the below code we didn't used the dollar before totalPrice as it was the new field we created
db.products.aggregate([
{$match:{price:{$gt:1200}}},
{
$group:{
_id:"$category",
totalPrice:{$sum:"$price"},
},
},
{
 $sort:{"totalPrice":1}
}
])

<!-- -------------------------------------------------------------------------------- -->

$project

this stage reshapes the docs , includes and excludes fields and performs operations on fields
{$project:{<field1>:<expression1>,...}}

ex:---->
the above code projects the name field and calculates a discountedPrice field by subtracting 5 from the price

the name:1, means we're returning the name field also
db.products.aggregate([

    {$project:{name:1,discountedPrice:{$subtract:["$price",5]}}}

])
ex:---->
in the below code we're also matching for prices greater then 1200

db.products.aggregate([
{$match:{price:{$gt:1200}}},
{$project:{name:1,discountedPrice:{$subtract:["$price",5]}}}
])

<!-- -------------------------------------------------------------------------------- -->

$push
this stage adds elements to an array field within docs

{$push:<expression>}

ex:-->
db.products.aggregate([
{
$match:{price:{$gt:1200}}
},
{
$group:{
_id:"$price",
products:{$push:"$name"},
}
}
])

<!-- -------------------------------------------------------------------------------- -->

$unwind
this stage deconstructs an array field and produces multiple fields
{$unwind:<array>}

db.products.aggregate([
{$unwind:"$colors"},
{
$match:{price:{$gt:1200}}
},
{
$group:{
_id:"$price",
products:{$push:"$name"},
}
}
])
<!-- -------------------------------------------------------------------------------- -->
$addToSet

this stage adds elements to an array field while preventing duplicates

db.products.aggregate([
{$unwind:"$colors"},
{
$match:{price:{$gt:1200}}
},
{
$group:{
_id:"$price",
products:{$addToSet:"$name"},
}
}
])
<!-- -------------------------------------------------------------------------------- -->
$size
it calculates the length of an array field

wherever we use the $operator we cannot use the $size operator over there

db.products.aggregate([
{$unwind:"$colors"},
{
$match:{price:{$gt:1200}}
},
{
$group:{
_id:"$price",
products:{$addToSet:"$name"},
}
},
{
    $project:{
        _id:1,//we are writing 1 as we want to return these datas
        colors:1,
        colorLength:{$size:"$colors"},
    }
}
])
<!-- -------------------------------------------------------------------------------- -->
$limit,$skip
limit gives only one data
skip is used for skipping the data

db.products.aggregate([
{$unwind:"$colors"},
{
$match:{price:{$gt:1200}}
},
{
$group:{
_id:"$price",
products:{$addToSet:"$name"},
}
},
{
    $project:{
        _id:1,//we are writing 1 as we want to return these datas
        colors:1,
        colorLength:{$size:"$colors"},//inplace of allColors we can use only those fields which we're getting back from the previous stage
    }
},{
    $limit:1,
}
},{
    $skip:1,
}
])

<!-- -------------------------------------------------------------------------------- -->

$filter
it filters elements of an array on based on the specified conditions

{
    $project:{
        <field>:{
            $filter:{
            input:"$<array>",
            as:"<variable>",
            cond:<expression>
            }
        }
    }
}

ex::->
//values is the name of the array we're accessing
//and val is the new name of the values array
// and we're also returning the name field 
// the double dollar sign val is used to represent the new field we created
// and the val is stored in the newValues
db.col.aggregate([{
    $project:{
       name:1,
       newValues:{
         $filter:{
           input:"$values",
            as:"val",
            cond:{$gt:["$$val",30]}
         }   
       } 
    }
}])
<!-- -------------------------------------------------------------------------------- -->