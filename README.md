# GraphQL Notes

  - GraphQL is a query language for API's. It provides a server side runtime to execute queries.
  
  - Queries in GraphQL are defined with types with strong Schema definition.
  
  - GraphQL is not tied to any specific darabase or storage engine. It is backed by existing code and data
    provides an extra abstraction to fetch data more effectively and efficiently.
    
  - Every GraphQL service is created by defining type of data it needs to be returned/fetched as fields.
    Then providing a function for each field type.
    
## How to Query GraphQL Server

### Fields

  - Fileds are integral part of GraphQL query. We will define what needs to be fetched in query filed as simple field
    or it can be an Object.
    
  - In case of an Object is required we have to define what fields in that Object needs to be fetched.
  
  ```
  Req Query
  
  filim{
    name
    actors{
     name
    }
  }
  
  Response
  
  {
    "data": {
      "film":{
         name: "Titanic"
         "actors": [
           {"name":"Actor-1"},
           {"name":"Actor-2"},
           {"name":"Actor-3"}
         ]
      }
    }
  }
  
  ```
### Arguments

  - In GraphQL we can pass arguments to every fileds we are trying to fetch.
  
  - In normal REST services we can only pass single set of arguments as Query parameters in URL. Which limits our ability to do granular data fetching.
    Due to this nature of REST we may need to do more roundtrips/multiple API calls to fetch different scenario data.
    
 - In GraphQL by setting arguments in filed level we can able to fetch the data in Single call for different predicates.
 
 - Its supports Enumaration type as well. GraphQL comes with default set of types along with we can defined curstion types as well.
 
```

Request

{
  human(id: "100"){
    name
    height(unit: FOOT)
  }
}

Response

{
 "data":{
    "human": {
      "name": "George",
       "height" : 4.9123
    }
 }
}

```
### Aliases

  - In GraphQL we can't directly query for the same field with different arguments. To achieve this we can use aliases.
  
  - Aliases help us to rename the filed with any name which makes same field can be queried with different arguments.
 
```
Request
{
indiaCapital: capitol(country: INDIA){
  name
}
usCapital: capitol(country: US){
  name
}

Response

{
 "data":{
   "indiaCapital" : {
     "name" : "New Delhi"
   },
   "usCapitol" :{
     "name" : "Washingdon DC"
   }
 }
}

```
### Fragments

  - Fragments are reusable units of fileds. Which help us to avoid defining same fileds in differernt elements.
  
  - Fragments are kind of template, where it can be defined on one place/once or with one variable reference and can be 
    used under different fields.

```
Request

Request
{
indiaCapital: capitol(country: INDIA){
  ...compareData
  
}
usCapital: capitol(country: US){
  ...compareData
}
}

fragment compareData on City {
  name
  population
  hospitals{
    name
  }
}

Response

{
"data" :{
  "indiaCapital" : {
     "name" : "New Delhi",
      "population" : 100000,
       "hospitals" : [
        {"name" : "NDHospital-1"},
        {"name" : "NDHospital-2"},
        {"name" : "NDHospital-3"}
       ]
  },
   "usCapital" : {
     "name" : "Washindon DC",
      "population" : 10000,
       "hospitals" : [
        {"name" : "WDCHospital-1"},
        {"name" : "WDCHospital-2"},
        {"name" : "WDCHospital-3"}
       ]
  }
}
}

```
### GraphQL Query Syntax

  - GraphQL query will have 2 main components 1.Operation Type , 2. Operation Name.
  
  - Operation type has 3 possible values. 
    
     - query
     - mutuation
     - subscription
     
  - OperationName is a meaningful name of their operation of the query.

  **It is always good practice to use OperationName and OperationType while defining GraphQL queries**
  
  In below example **query** keyword is OperationType indicate its a fetch operation.
  
  **UserNameAndPrivilageRoles** is OperationName which is logical name of the query.
  
  
```
Request

query UserNameAndPrivilageRoles{
   user{
      name
      privilages {
        role
      }
   }
}

Response

{
  "data": {
    "user" : {
      "name" : "Mark",
       "privilages" : [
         {"role" : "Developer"},
         {"role" : "Admin"},
       ]
    }
  }
}
```

### Variables

   - Variables will help us to pass dynamic arguments to GraphQL query.
   
   - This will help the client application code to modify/manipulate GraphQL query directly.

   - We can able to set default values to any variable we are defining as part of the query.

```

Request

query CarAndModelVariants($model: Model){
  car(moderl: $model){
    name
    variants{
      name
    }
  }
}

{
 "model" : "Civic"
}

Response

{
 "data" : {
   "car" : {
    "name" : Honda,
     "variants" : [
       {"name" : "LX"},
       {"name" : "DX"}
     ]
   }
 }
}

```

### Directives

   - Directives are attached to a field or fragments of a Query which controls execution of query on the server.

   - It will help us control which field should be include/excluded or skipped in the query definition.

   - GraphQL will support 2 directive out of the box.
   
      - **@include(if: Boolean)** Only include this field in the result if the argument is true.
      
      - **@skip(if: Boolean)** Skip this filed if the argument is true.

### Mutuations

   - Mutuations help us to modify server side data. With help of mutuation we can able to modify server side data
     and get the modified data. Also it will allow us to get the specific portion of the modified data after the modification.
     
   - 
     
```
Request

mutuation CreateCommentForBlog($blog: Blog!, $comment: Comment!){
  CreateComment(blog: $blog, comment: $comment){
     comment
     like
  }
}

{
  "blog" : "Cook Recipie",
  "comment" : {
      "comment" : "This blog is useful",
      "like"    : "Yes"
  }
}

Response

{
 "data" : {
   "CreateComment" :{
      "comment" : "This blog is useful",
       "like"   : "Yes"
   }
 }
}

```
   
### Schemas and Types

  - A GraphQL service is defined by set of types which describes set of possible data it can service.
    It act as a contract between client and GraphQL service.
    
  - When any client queries aganist GraphQL service request is validated aganist the schema and its executed.
  
  - GraphQL supports different types of its own. In which Object type is basic one.
  
```

type Character{
  name: String!
  appearsIn: [Episode!]!
}

 # In this example **Character** is a GraphQL object type which has 2 fileds.
 
 # **name** is one field with string data element. As String is build in sclar type. It dosen't have any nested types.
 
 # **appearsIn** is another field which holds Array of Episode objects.
 
 # ! mark in the filed indicate its a non-null field.
```
### Arguments

  - Every filed in GrapgQL object can have zero or more arguments.
  
  - All arguments are named and all are passed by name.
  
  - We can set default values to arguments.

```
type Car {
  id: ID!
  name: String!
  length(unit: LengthUnit = METER) : Float
}

```

### Scalar Types

  - GraphQL object has many fields each fields will resolve to single not nested filed which is scalar field.

  - Each GraphQL filed objects will resolved to a single Scalar field.
  
  - GraphQL will support following Scalar types.

      - Int     -> A signed 32 bit integer.
      - Floar   -> A signed double-precision floating point value.
      - String  -> A UTF-8 character sequence.
      - Boolean -> true or false
      - ID      -> It represents a unique identifier.
      
### Enumeration Types

  - GraphQL support Enum types. Its a special kind of scalar that is restricted to a particular set of
    allowed values.
    
  - When enum type is defined for a filed GraphQL validate given filed is has any one of enum defined values.
    If not throws error.
    
```
enum DaysOftheWeek {
  SUNDAY
  MONDAY
  TUESDAY
  WEDNESDAY
  THURSDAY
  FRIDAY
  SATURDAY
}
```

### List and Non-Null

  - GraphQL support List type a list of values are represented by [SOMEFIELDDATA].
  
  - With ! we can define Non-Null charactersitic of that filed.

```

type Week {
  daysOfWeek: [WeekDays]!

}

 # [WeekDays]! -> Indicate GraphQL server always expected to return a value for this filed.


```

### Interfaces

  - Interfaces are helpful to define abstract type of objects. When we want to have same fileds at different types of
    objects then we can define those fileds in interfaces.
    
 ```
 interface Person {
   id: ID!
   name: String!
   age: String!
   
 }
 
 type Student implements Person {
   id: ID!
   name: String!
   age: String!
   school:
 }
 
 type Employee implements Person {
   id: ID!
   name: String!
   age: String!
   employeer: String!
 }
 ```
 
### Input Types

  - In GraphQL we can pass entire object to GraphQL server using Input types. This is very useful 
    in case of mutuation where we have to save an object in GraphQL server.
    
 ```
 
 input Comment {
   like: String
   comment: String!
 }
 
 mutuation CreateComment($post: Post, $comment: Comment) {
 
  createComment(post: $post, comment: $comment) {
      like
      comment
   }
   
  Variables
  {
    "post": "MyPost",
     "comment" : {
       "like" : "Yes",
       "comment": "This looks good"
     }
  }
 }
 ```
