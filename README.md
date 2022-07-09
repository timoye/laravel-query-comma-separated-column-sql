# Laravel Query Comma Separated Column Sql Eloquent using find_in_set

## Introduction

It is not uncommon to store comma separated values in a mysql column in a Laravel Project

```
colours : "red,blue,green,yellow,black,white"

users : "77,4,5,688,5454,342,32,332"

tags : "mysql,laravel,css,html"
```

## Querying the Column

MySQL Function [FIND_IN_SET()](https://www.w3schools.com/sql/func_mysql_find_in_set.asp) can be used to query
```php
$search;
ModelName::whereRaw("FIND_IN_SET($search,colours)");

ModelName::whereRaw("FIND_IN_SET($search,users)");

ModelName::whereRaw("FIND_IN_SET($search,tags)");
```

## Querying the Column - With Protection against SQL Injection
```php
$search;
ModelName::whereRaw("FIND_IN_SET(?,colours)",[$search]);

ModelName::whereRaw("FIND_IN_SET(?,users)",[$search]);

ModelName::whereRaw("FIND_IN_SET(?,tags)",[$search]);
```


## As Scopes in the Model
```php
    class ModelName extends Model{
    
    
        public function scopeContainsTag($query,$tag){
            return $query->whereRaw("FIND_IN_SET(?,tags)",[$tag]);
        } 
    }
```

You can call the scope while querying the model from a controller

```php
    public function index(Request $request){
        ModelName::containsTag($request->tag_name)->get();
    }
```

## Note
There are other ways of storing data in a column other than using comma separated,
Read Laravel Documentation [Array & JSON Casting](https://laravel.com/docs/9.x/eloquent-mutators#array-and-json-casting) 