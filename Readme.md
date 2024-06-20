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

cmd("to import json in mongodb , if the json objects are present inside a single array,products.json is the name of the file  ")-->"mongoimport products.json -d database_name -c collection_name --jsonArray"


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