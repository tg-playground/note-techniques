
# JQuery Note

menu

- Introduction
- Get Dom
- Modify Dom
- for & each
- Event
- Ajax
- String Array JSON Object


### Introduction
```
var $varName //Defining JQuery object variable
$("")        //get JQuery object constant
$.ajax();    //JQuery official library
$().index()  //JQuery object function

```


### Get Dom 

JQuery Selector gets JQuery Object

Selector

```
$("#id")  
$("a")  
$(".ClassName")  
$("*") //select all  
$("selector1,selector2,...,selectorN ") // select by multiple condition union  
```

Hierarchical Selector  

```
$("table td") // select all descendant  
$("form > input") // select all descendant  
$("label + input") // select all input after lavel
$("#prev ~ div") // select all sublings
```

Filter

```
:first
$("tr:first")

:last
$("tr:last")

:not()
$("input:not(:checked)") 
$("#form1 input:not('#id')")

:even
$("tr:even")

:odd
$("tr:odd")

:eq()
$("tr:eq(1)")

:gt
$("tr:gt(1)") // greater than

:lt
$("tr:lt(3)") // less than

:header // select all header elements, such as <H1>,<H2>

:animated

```

Filter by Content

```
:contains
$("div:contains(hello)") // select all div contains text 'hello'

:empty
$("td:empty") // select all td text is empty

:has()
$("div:has(p)") // select all div has <p> element

:parent()
$("div :parent(p)") // select all div contained by <p> element
```

Filter by isVisible

```
:hidden
$("input:hidden")

:visible
$("input:visible")

```

Filter by Attribute

```
[attr]
$("div[id]") // all <div> have id attr 

[attr=value]
$("div[id=div1]")

[attr!=value]
$("div[id!=div1]")

[attr^=value]
$("div[id^=test]")] // all div, 'id' attr value begin with 'test'

[attr$=value]
$("div[id$=test]") // all div, 'id' attr value end with 'test'

[attr*=value] 
$("div[id*test]") // all div, 'id' attr value contains 'test'

```

Filter by child

```
:nth-child
$("ul li:nth-child(2)") // match ul child li second

:first-child
$("ul li:first-child")

:last-child
$("ul li:last-child")

:only-child
$("ul li:only-child") // match ul only contain one li
```

Filter by Method

```
first()
last()
eq("") // index begin with 0 

filter("p") // get all <p> elements in jquery dom set

not("p, .name") // get all not <p> and class=name elements in jquery dom set

$("li").has("span") //get all li has child element 'span'


parent() // one parent
parents() // all parents
parents("") // specify parents
parentsUntil("") //? 

children(“”) 
children()
find("");
find()和 children() 方法类似，不过后者只沿着 DOM 树向下遍历单一层级。

siblings() 
sibings("") // get other dom of same level, don't contain self
next()
nextAll() 
nextUntil() 
prev()
prevAll()
prevUntil()

```

Filter by Form 

```
element name

:button		//Selects all button elements and elements of type button.
:input  //Selects all input, textarea, select and button elements.


input type

:checkbox
:file  //type is file.
:image	//type is image.
:text 
:password
:radio
:reset
:submit

input status

:checked 	//Matches all elements that are checked or selected.
:selected
$("option:selected[value=1]")
:focus  	//Selects element if it is currently focused.
:disabled
:enabled	//Selects all elements that are enabled.

```


### Modify Dom

```
insert

$("").insertafter("html")
$("").before("html")
$("").after("html")
$("").append("html")
$("").prependTo()

get

$("").clone()

$("").html()
$("").text()
$("").val()
$("option1").prop("selected")  // check option1 is selected, return true or false.
$("").attr("attrName")

$("td").index($("#target_td"))  // target element in set index number.

delete

$("").remove()
$("").empty()
$("").removeAttr("attrName")


update

$("").hide()
$("").show()
$("").toggle() // cycle from hide and show

$("").css("key","value")
$("").css({"key1":"value1","key2":"value2"})

$("").html("html")
$("").text("text")
$("").val("text") // set 'value' attr value
$("").prop("selected",true)   //To change DOM properties such as the checked, selected
$("").prop("selected", function(i, val){ }); // traversal every element, if dom is selected val=true else val=false.
$("").attr("attrName","attrValue") // set attrValue
$("").attr({"attrName1":"attrValue1","attrName2":"attrValue2"})

$("").repalceWith("html")
$("html").repalceAll("p")

```

### for & each

each

```
$("option").each(function(){
    if($(this).attr("value")==search){
        $(this).attr("selected", "selected");
    } 
});
// hint: each() can't use break or continue
```

for

```
$trSet = $("tr");
for (var i = 0; i < $trSet.length; i++){
    $trSet.eq(i).find("label").text(i + 1);
}
```

### Event
### Ajax

```
$.ajax({
	url: "",
	type: "",
	async: false,	
	traditional:true,
	dataType: "",
	data:"",
	success: function(){
	},
	error: function(){
	}
});
```

### String Array JSON Object

String

```
String.fromCharCode(65) // number '65' to char 'A'

        
```

JSON

```
JSON.stringify("string")



```

