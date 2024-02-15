# dataweave
dataweaveLearnings
Problem1 --

Input

{
    "name": "Indrajeet Kumar"
}


script1:- 

%dw 2.0
output application/json
var tokens = sizeOf(payload.name splitBy " ")
fun getInitials(name: String): 

String = 
    if (tokens == 3) 
        (upper (name[0] ++ name[(indexOf(name, " ") + 1)] ++ name[(lastIndexOf(name, " ") + 1)]))
    
    else if (tokens == 2) 
        (upper (name[0] ++ name[1] ++ name[(lastIndexOf(name, " ") + 1)]))
    
    else
        (upper (name[0] ++ name[1] ++ name[2]) )

---
{
    "Intials": getInitials(payload.name),
   
}


Output:-
{
  "Intials": "INS"
}



Script 2:- 

%dw 2.0
import substringBy from dw::core::Strings

output application/json
var tokens =  upper(trim(payload.name))  substringBy ($ == "~" or $ == "=" or $ == "_" or $==" ")
var delims = sizeOf(tokens)

---

if(delims == 3) tokens[0][0] ++ tokens[1][0] ++ tokens[2][0]
else if(delims == 2) tokens[0][0] ++ tokens[0][1] ++ tokens[1][0]
else  upper(tokens[0][0 to 2])




Script 3 :- 

%dw 2.0
output application/json
import substringBy from dw::core::Strings
import first from dw::core::Strings

var tokens = upper(payload.name)  substringBy ($ == "~" or $ == "=" or $ == "_" or $==" ")
fun getInitials(name: String): 

String = 
    if(sizeOf(tokens) == 3) tokens[0][0] ++ tokens[1][0] ++ tokens[2][0]
    
    else if(sizeOf(tokens) == 2) tokens[0] first 2 ++ tokens[1][0]
    
   else  tokens[0] first 3

---
{
    "Intials": getInitials(payload.name),
   
}



==============================================================================================================
Problem 2 

Input: " hello world "
Output: " world hello "

Input: " I love coding "
Output: " coding love I "



%dw 2.0
output application/json
var list = payload.name splitBy  (" ") 
---
    // Using orderBy with criteria function.
    list orderBy ((item, index) -> -index) joinBy  (" ")

==============================================================================================================
Problem 3

Given a string, write a function to remove all duplicate characters and return the string with each character appearing only once. The order of characters should be maintained. For example:

Input: "programming"
Output: "progamin"

Input: "hello"
Output: "helo"


%dw 2.0
output application/json

---
    // Using orderBy with criteria function.
payload splitBy ("") distinctBy $ joinBy  ("")

=======================================================================================================

Given two strings, write a function to determine if one is a permutation of the other. That is, check if the second string is a rearrangement of the characters in the first string. For example:

Input: "listen", "silent"
Output: True

Input: "hello", "world"
Output: False


%dw 2.0
import * from dw::core::Strings
output application/json
var first = trim(payload substringBefore(",") splitBy  ("") orderBy $ joinBy (""))
var last = trim(payload substringAfter(",") splitBy  ("") orderBy $ joinBy (""))

---
    // Using orderBy with criteria function.
first == last

======================================================================================================
Email validation :-

%dw 2.0
output application/json
var regexEmail = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/
 
var message = payload
---
message matches regexEmail

======================================================================================================

Array<String> to Array<Object>

Input
["key1","value1","key2","value2","key3","value3"]

Output
[
  {
    "key1": "value1"
  },
  {
    "key2": "value2"
  },
  {
    "key3": "value3"
  }
]


sctipt: - 
%dw 2.0
output application/json
import divideBy from dw::core::Arrays
---
//payload must be an object

payload divideBy  2 map {
    ($[0]) : $[1],
}


====================================================================================================
Removing escape characters from xml

From	To
<	&lt;
>	&gt;

Input : 

<note>
<to>Tove</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>Don't forget me this weekend!</body>
</note>


Output :- 

&lt;note&gt;
  &lt;to&gt;Tove&lt;/to&gt;
  &lt;from&gt;Jani&lt;/from&gt;
  &lt;heading&gt;Reminder&lt;/heading&gt;
  &lt;body&gt;Don't forget me this weekend!&lt;/body&gt;
&lt;/note&gt;


Script : -
%dw 2.0
output text/plain
---
write(payload,"application/xml") 
replace "<?xml version='1.0' encoding='UTF-8'?>\n" with ""
replace "<" with "&lt;"
replace ">" with "&gt;"



========================================================================================================
