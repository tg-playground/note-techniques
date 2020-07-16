# JavaScript Note


menu

- Introduction
- Data Type
- function
- Statement
	- for/in loop
- JS Errors
- Object
	- Object, Array Object, JSON Object, Date, Number
 	- Object define, using element, function

------------------------------------------

### Introduction

What is JavaScript

- HTML to define the content of web pages.
- CSS to specify the layout of web pages.
- JavaScript to program the behavior of web pages.

JavaScript Function

- Change HTML Content, Attribute Values, Styles (CSS)
- Hide or show HTML Elements


Using JavaScript

1.JavaScript in `<head> or <body>`

Placing scripts at the bottom of the `<body>` element improves the display speed, because script compilation slows down the display.

2.External JavaScript
 
External JavaScript Advantages: Cached JavaScript files can speed up page loads

located in a specified folder on the current web site:

`<script src="/js/myScript1.js"></script>`

located in the same folder as the current page:

`<script src="myScript1.js"></script>`

3.External References

referenced with a full URL.

`<script src="https://www.w3schools.com/js/myScript1.js"></script>`




### Data Type

JavaScript Types are Dynamic. Same variable can be used to hold different data type.

```
var x;           // Now x is undefined
x = 5;           // Now x is a Number
x = "John";      // Now x is a String
```

Primitive Data Type

- string
- number
- boolean
- undefined

NaN
- NaN(Not-a-Number),Global Object in javascript
- typeof NaN //number
- isNaN() 

```
typeof "John"              // Returns "string" 
typeof 3.14                // Returns "number"
typeof true                // Returns "boolean"
typeof false               // Returns "boolean"
typeof x                   // Returns "undefined" (if x has no value)
```

Complex Data Type

- function
- object

```
typeof {name:'John', age:34} // Returns "object"
typeof [1,2,3,4]             // Returns "object" (not "array", see note below)
typeof null                  // Returns "object"
typeof function myFunc(){}   // Returns "function"
```

Data Type Conversion

int to char

```
var chr = String.fromCharCode(97 + n); 
char='abcdefghijklmnopqrstuvwxyz'.charAt(code);
```

number to string


```
var foo = 45;
var bar = '' + foo;
//raw speed there is an advantage for .toString()

```

String to number


```
parseInt(string,radix) 
parseFloat(string)
Number(string) // If you want the string to fail with NaN if it has other characters in it, Number() may be a better choice.

```
### Function

function(){}


### Statement

The For/In Loop
```
var person = {fname:"John", lname:"Doe", age:25}; 

var text = "";
var x;
for (x in person) {
    text += person[x];
}
```




### Object

new Object()

### Array Object

var arr = [] or var arr = [ , , ] or new Array(length)



### JSON Object

1.Basic

define: var jsonObj = {}; or var jsonObj = {"":"",...};

key data type: string. value data type: string, number, object, array, boolean, null

get element: jsonObj.key or jsonObj['key']


2.JSON Array

JSON array also is array Object, but elments of Array is Json Object

```
Example-1			

var jsonObj = {'id':1, roles:[{'roleId':1,'roleName':'teacher'}, {'roleId':2, 'roleName':'admin'}]}
alert(jsonObj.roles[0].roleName);
jsonObj.roles.push({'roleId':3, 'roleName':'student'});
alert(JSON.stringify(jsonObj)); 
//{"id":1,"roles":[{"roleId":1,"roleName":"teacher"},{"roleId":2,"roleName":"admin"},{"roleId":3,"roleName":"student"}]}

Example-2

var arr = ['zhangsan','lisi'];
var jsonArrStr = JSON.stringify(arr); //["zhangsan","lisi"]

```


3.Conversion

JSON.stringify(jsonObject)
JSON.parse(jsonStr)	


4.Joint

``` 
Example

var jsonObj = {};
jsonObj.name = 'zhangsan';
jsonObj.age = 15;
jsonObj.cities = [];
var cityName = ['nanjing','beijing'];
var cityArea = ['south','north'];
for (var i = 0; i < cityName.length; i++){
	var json1 = {};
	json1.name = cityName[i];
	json1.area = cityArea[i];
	jsonObj.cities.push(json1);
}
alert(jsonObj['cities'][0]['name']);
alert(jsonObj.cities[0].name)
alert(JSON.stringify(jsonObj));

```

5.Other


reference : https://www.w3schools.com/js

