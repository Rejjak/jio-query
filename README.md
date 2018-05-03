## Install
```js
$ npm install jio-query
```
## Introduction
This is a node.js package that's depend on mysql driver. It is written in JavaScript, does not
require compiling, and is 100% free.

If you want to use this package, you have create database connection first.Once database connection is successfull,
you get an object of the connection.

Now in this package we need this database connection object.

Here is an example on how to use it:

```js
var mysql = require('mysql');
var jq = require('jio-query');

var con = mysql.createConnection({host:'localhost',user:'root',password:'',database:'db_name'});
con.connect(function(err){
  if(err) throw err;
});

var model = jq.loadMethods(con);  // here 'con' is the object of the database connection

var json_str = {
	table		: 'table_name',
	where 		: {phone:5555}
};

model.selectData(json_str,function(data){
	//your code here
});
```

## Others Query Methods

To submit the query, please use the following methods.

Insert Query:
```js
var json_str = {
	table		: 'table_name',
	body 		: {name:'Rejjak Ali',email:'rejjakali94@gmail.com'},
	where 		: {id:32}
};
model.addData(json_str,function(data){
	//your code here
});
```
Delete Query:
```js
var json_str = {
	table		: 'table_name',
	where 		: {id:32}
};
model.removeData(json_str,function(data){
	//your code here
});
```
Update Query:
```js
var json_str = {
	table		: 'table_name',
	body 		: {name:'Rejjak Ali',email:'rejjakali94@gmail.com'},
	where 		: {id:32}
};

model.updateData(json_str,function(data){
	//your code here
});
```
## Custom Query
If you need different requirement that is not implemented on this package, then you can make your own custom query, and pass this query to 'executeQuery' function.Please see the below example.

```js
var sqlQuery = 'select * from node_customer';
model.executeQuery(sqlQuery,function(result){
		//your code here
});
```

## Descriptio for the passing parameters key

When establishing a json string, you have to set the following options:

* `table`: pass the table name here
* `body` : there are two type of data passing with 'body' key. If you want to retrieve the data from database based on perticular cloumns name, then you have to pass the cloumns name as an array format.like..

```js
body:['name','email','etc..']
```

On other hand, if you want to 'update' or 'insert' data to the perticular columns with perticular values, then go to the below format,like..

```js
body : {name: 'test',email:'test@test.com',age:54}
```

## For simple where clause

```js
where: {id:45,'age!=':21}

// where id='45' and age!='21'
```

if you use this type of format, by default this will take for 'AND' operator. If you want 'OR' operator, then simply call like this,

```js
where_or : {id:45,age:21}

// where id='45' or age='21'
```


IF you want 'IN' keyword with where clause, then pass the value as an array format with perticular key like this..

```js
where	: {id:[28,30,31],age:[45,52]}

//where id in('28','30','31') and age in('45','52')
```

or you can also use like this

```js
where_in    : {id:[28,30,31],age:[45,52]}
//where id in('28','30','31') and age in('45','52') 

where_in_or : {id:[28,30,31],age:[45,52]}
//where id in('28','30','31') or age in('45','52')
```
## Another format of where clause

* For AND operator

```js
where:{
  and: {
    name : 'test',
    email : 'test11@test.com'
  }
}	
//name = 'test' and email = 'test11@test.com'

where:{
  and:{
    'start_date <=' : 'test',
    'end_date =>' 	: 'test',
  }
}
//where start_date<='test' and end_date=>'test'

```

* For OR operator

```js
where:{
  or: {
    name : 'test',
    email : 'test11@test.com'
  }
}
//name = 'test' or email = 'test11@test.com'


where:{
  or:{
    'start_date <=' : 'test',
    'end_date =>' 	: 'test',
  }
}
//where start_date<='test' or end_date=>'test'

```

* For NOT operator

```js
where:{
  and:{
    not: {
      name : 'test',
      email : 'test11@test.com'
    }
  }
}
//not name = 'test' and not email = 'test11@test.com'

where:{
  or:{
    not: {
      name : 'test',
      email : 'test11@test.com'
    }
  }
}
//not name = 'test' or not email = 'test11@test.com'
```
* Nested clause
```js

where:{
  and:{
    in:{
      id:[28,30,31],
      address:['test','test2','test3']
    },
    and : {
      aname : 'test1',
      bname : 'test2'
    },
    or : {
      cname : 'test3',
      dname : 'test4'
    },
    not : {
      'ename' : 'test5',
      'gname' : 'test6'
    },
  }
}

//where id in('28','30','31') and address in('test','test2','test3') and (aname='test1' and bname='test2') and (cname='test3' or dname='test4) and (not ename='test5' and not gname='test6')
```
* LIKE keyword

```js
where:{
  and:{
    like : {
      'ename' : 'test5',
      'gname' : 'test6'
    }
  }
}
//where ename like '%test5%' and gname like '%test6%'

```

* `order`: This is for 'order by' keyword and for this package pass the parameter as an array format like this,

```js
order : ['address']
```

* `group`: This is for 'group by' keyword and for this package pass the parameter as an array format like this,

```js
group : ['name','email']
```

* `limit` : This is for 'LIMIT' clause, the LIMIT clause accepts one or two arguments. The values of both arguments must be zero or positive integers, so you have pass the values as an array format like this,

```js
limit		: [0,10]
```

* `having` : For having clause, you have to pass the data as a json format like this,

```js
having		: {id:30} or 
having_or	: {id:30} 
```

* `like` : this is for simple LIKE keyword with OR operator,for AND operator use 'like_and'

```js
like		: {name:'Rejjak Ali',email:'rejjakali94@gmail.com',address:'test'}
```

* `query` : This key is return the mysql query string, if you want see what exact query will be execute by your passed parameters. By default query is 'false'. So, for get the result it's not mendatory but if you want see the query string then you need to pass 'true'. If you pass 'true' your query should not be execute and you will get a mysql query string.

```js
var json_str = {
	table		: 'table_name',
	body 		: {name:'Rejjak Ali',email:'rejjakali94@gmail.com'},
	where 		: {id:32},
	query 		: true
};
model.updateData(json_str,function(data){
	console.log(data);
});

//Output: update table_name set name='Rejjak Ali',email='rejjakali94@gmail.com' where id='32'
```

## Summary of all parameter and condition of the passing object
* Please go through the below object, you have to used same like this structure based on your requirement.Its not required to pass unnecessary parameters, please pass the parameter based on your requirement. 

```js
var nested_clause = {
  and/or : {
        id:30,
        in:{
          roll:[28,30,31],
          address:['test','test2','test3']
        },
        and : {
          test1 : 'test1',
          test2 : 'test2'
        },
        or : {
          test3 : 'test3',
          test4 : 'test4'
        },
        not : {
          'test5' : 'test5',
          'test6' : 'test6'
        },
        like : {
          'test7' : 'test7',
          'test8' : 'test8'
        },
        'fname'    : 'test',
        'lname'    : 'test',
        'start =>' : 'test',
        'end   <=' : 'test',    
  }
};
var params = {
	table		: 'node_customer',
	body 		: {name: 'test',email:'test@test.com',age:54},
	// for select statement is used for perticular field,otherwise remove the body key from this object,consequently it will take '*'
	body 		: ['name','email','ctc..'],
	// for simple clause
	where 		: {id:32,email:'rejjakali94@gmail.com'},
	where_or 	: {id:32,email:'rejjakali94@gmail.com'},
	where_in	: {id:[28,30,31],age:[45,52]},
	where_in_or	: {id:[28,30,31],age:[45,52]},
	// for nested clause
	where       : nested_clause,
	order		: ['address'],
	group		: ['name','email'],
	limit		: [0,3],
	having		: {id:30},
	// for simple like operator, otherwise ignore this key and go to nested clause structure with 'where' key and remove 'like' key from this object, because both key will not work simultaneously.
	like		: {"name !":'test',"email":'test',"address":'test'}
	query 		: false
};
```
* Note: 'where', 'where_or', 'where_in', 'where_in_or' --- You can use only one of these key at a time 
