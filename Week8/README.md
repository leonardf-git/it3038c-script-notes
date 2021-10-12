# Python Data Storage Types 

 

Let’s talk briefly to clear up some of the python data types that might be a bit confusing to those of you who are new to Python.  

### Lists 

Lists are the same as arrays. They’re just called lists in Python, even though they are arrays. However, python lists are far more flexible than arrays because you can have multiple data types in a list, including sublists.  

Let’s create a list now. 

Open a python console 

Create a list called food 

 
```
>>> food = [“fruit”, “vegetables”, “bread”, “beer”] 
>>> food[0] 
“fruit” 
```
 

Notice how the first entry in a list is [0], because lists are “zero based”, meaning they start at 0.  

 

### Sublists 

Lists can also have Sublists, like so 

 
```python
>>> food = [["apple","banana"],["tomato","corn"], "beer"] 

>>> food[0] 

['apple', 'banana'] 

>>> food[0][0] 

'apple' 

>>> food[0][1] 

'banana' 

```

As you can see, we can have a list full of lists, and to call a specific list, we pass one index (food[0]). Two get one item within one list, we pass two indexes (food[0][0]) 

 

What if we go out of range? 
```python
>>> food[1][2]

Traceback (most recent call last): 

  File "<stdin>", line 1, in <module> 

IndexError: list index out of range 
```
 

We can add or modify lists.  
```python

>>> food[0][0] = “strawberry” 

>>> food 

[['strawberry', 'banana'], ['tomato', 'corn'], 'beer'] 

>>> food.append("blueberry") 

>>> food 

[['strawberry', 'banana'], ['tomato', 'corn'], 'beer', 'blueberry'] 
```
 

 

What if we apply an index to our “beer” entry 
```python
>>> food[2] 

'beer' 

>>> food[2][0] 

'b' 

>>> food[2][3] 

'r' 
```
 

As you can see, it treats the ‘string’ variable as an index. This also works for negative indexes, starting at the end. Let’s create a new string to test this.  

 
```python
>>> food[2][-4] 
‘b’ 

>>> word = "Hello World" 
>>> word[0] 
'H' 

>>> word[1] 
'e' 

>>> word[-3] 
'r' 

>>>word[0:4] 
‘Hell’ 

>>> word[0:4] + word[-6:-1] 
'Hell Worl' 
```
 

 

### Tuples 

 

Tuples are immutable lists, meaning unlike a list, a tuple cannot be changed once created.  

And just so we’re aware: “Objects whose value can change are said to be mutable; objects whose value is unchangeable once they are created are called immutable.” 

 

Tuples can be referenced the same as lists, giving them a few advantages:  

Tuples are faster, protects from accidental changes, can be used as dictionary keys (more on that soon).  

 

Let’s build a tuple.  

 
```python
>>> t = ("this", "is", "a", "tuple") 

>>> t[0] 

>>> t[0] 

'this' 

>>> t[3] 

'tuple' 
```
 

But we can’t append or modify a tuple 

 
```python
>>> t[0] 

'this' 

>>> t[0]="what" 

Traceback (most recent call last): 

  File "<stdin>", line 1, in <module> 

TypeError: 'tuple' object does not support item assignment 

>>> t 

('this', 'is', 'a', 'tuple') 

 ```

 

### Dictionaries 

Next to lists and tuples, dictionaries are essential to python programming. They’re like lists, but they have no order because they are accessed by keys.  

Let’s create an example.  

 
```python
>>> drink = {"soda" : "coke", "beer" : "dunkel", "water" : "water"} 

>>> drink 

{'water': 'water', 'beer': 'dunkel', 'soda': 'coke'} 

>>> drink['beer'] 

'dunkel' 

>>> drink['beer'] = "ale" 

>>> drink['beer'] 

'ale' 
```
 

Key data types must be immutable, so no lists or dictionaries. Tuples are ok though.  
```python
>>> drink['beer']=("dunkle","IPA","Ale") 

>>> drink['beer'] 

('dunkle', 'IPA', 'Ale') 

>>> drink['beer'][0] 

'dunkle' 

>>> drink['beer'][2] 

'Ale' 

>>> drink['beer'][1] 

'IPA' 

>>> drink['water'][1] 

'a' 

>>> drink['water'][0] 

'w' 

>>> drink['water'] 

'water' 

>>> drink['soda'] 

'coke'  

>>> di = { "countdown":(5,4,3,2,1), "countup":(1,2,3,4,5), (1,2,3):"abc" } 

>>> di['countdown'] 

(5, 4, 3, 2, 1) 

>>> di[(1,2,3)] 

'abc' 
```
 

If you’re ever unsure of what type of data a certain value is, you can use the type() function to find out.  

```python

>>> type(drink) 

<class 'dict'> 

>>> type(drink['beer']) 

<class 'tuple'> 

>>> type(drink['beer'][0]) 

<class 'str'> 
```
 

In a future lab, you’ll see that json is returned as a dictionary. This is why when we pull our json data, the data is returned as a dictionary object. And to parse that data, we have to use the above rules to get the data we want. Let’s test this out.  

 
```python
>>> import json, urllib.request 

>>> r = urllib.request.urlopen('http://api.openweathermap.org/data/2.5/weather?zip=45218,us&appid=4d77e6ffc103503b80a32507a754582a') 

>>> data=json.load(r) 

>>> print(type(data))  

<class 'dict'>  

>>> type(data['weather']) 

<class 'list'> 

>>> type(data['weather'][0]) 

<class 'dict'> 

>>> type(data['weather'][0]['description']) 

<class 'str'> 

>>> data['weather'][0]['description'] 

'overcast clouds' 
```
 

 
# Regular Expressions 

Regular Expressions or “reg-ex” is a set of characters that act as a ‘catch’ to other text. This text can be read by a computer to interpret expected content and validate it.  

 

An example of validation might come in the form of an email address. When a user submits a form to a web page, we want to validate the values are legit, so we use regular expressions to validate the form before we send it to the web server. An email address contains the following: 

 

Multiple alphanumeric characters and some special characters before the @ symbol 

Multiple alphanumeric characters and some special characters after the @ symbol 

A “dot” (.) Or multiple “dots” (.)  

Multiple alphanumeric characters and some special characters after the @ symbol (in this example, limited to .com, .net, .org, .edu) 

 

Here is how we do that.  

https://regex101.com/ 

 

Let’s create our Regular Expression string, starting small and working our way up.  

 
```
Cheat Sheet: 

^The – Starts with “The” 

End$ - Ends with “End” 

^The End$ - Starts with “The” and ends with “End” (exact match) 

The* End - matches string followed by 0 or more “e” (The End is valid, the ‘e’ is totally optional) 

The+ End - matches string followed by 1 or more “e” (‘Theeeee End’ is valid, but not ‘Th End’) 

The? End – may or may not have an “e” (The End, Th End, but not Thee End) 

[The ]+End$ - And characters “t”, “h”, “e” or <space>, but nothing else before or after “End”. 
```
 

The square brackets indicate that you can specify that list of characters.  

 
```
[A-Z] – Any Capital Letters 

[a-z] – Any Lowercase Letters 

[0-9] – Any Numbers 

[A-z0-9] – Any letters or Numbers, no special characters 

[@%_-.] – Special characters 

\$ - Regex characters used to validate the string need to be ‘escaped’ with a “\” 
```
 

Let’s validate an email address.  

 

First, tell RegEx what is allowed in the email address name, then tell it we NEED to have an “@” symbol and “gmail.com” 

`[A-Za-z0-9_%.-]+@gmail\.com$ `

 

Validate: 

`ajbothe@gmail.com `

Any other email will not work 

 

We can get fancy and say we’ll allow any letters and numbers in place of gmail 

`[A-z0-9_%.-]+@[A-z0-9-]+\.com$`

 

You can specify that it has to be .com, .net, .org or some other TLD 

`[A-z0-9_%.-]+@[A-z0-9-]+\.(com|net|org|ninja)+$`

 
Try: 
`aj@bothe.com`

`aj@bothe.ninja`

 

Finally, we can say it has to include a top level domain with at least 2 charcters, denoted by “{2,}” 

`[A-Za-z0-9_%.-]+@[A-z0-9_%.-]+\.[A-z0-9]{2,} `

 

`dog@go.com`

 

Don’t fret: People spend years studying and using regex and often struggle with the simplest forms of it. You could spend a whole semester teaching and learning it, so if it doesn’t make sense, don’t worry. Google is your friend. The most common inputs already have regex pre-written for us, we just have to find them.  

 

Now, back to python. Let’s test RegEx in Python.  

Create a file called `regex.py`

 

Let’s test the following code: 

 
```python
import re 

data = "Hello. My email is botheaj@ucmail.uc.edu. Please contact me!" 

p = re.compile('[A-Za-z0-9_%.-]+@[A-z0-9_%.-]+\.[A-z0-9]{2,}') 

print(p.search(data).group()) 
```
 

This code will take our string called data and pull out just the email address. Re.compile takes our regex as an argument and the search function takes our string and outputs the results compared to the regex.  

