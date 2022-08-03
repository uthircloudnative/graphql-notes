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
