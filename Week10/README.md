# Working with APIs 

#### List of Links for this week: 

https://home.openweathermap.org/api_keys  

 

## REST and APIs 

While as a systems administrator, you may not be coding against web services every day, it’s important to understand how they work so you can have a better idea of how to efficiently host them or manage systems that call them.  

Let’s discuss APIs, what they are and how they work.  

 

An API is an Application Programming Interface, which is the way an application allows a developer to talk with it. For example, you can use the Twitter API to post tweets, which requires authentication, URLs, classes, methods, and so on. API’s typically reserve data in the form of JSON. Any client can make a request to an API.  

For an API to be RESTful, it must be client to server, must be stateless, must use HTTP methods (GET, POST, etc).  

 

APIs are basically middlemen to a server, so a client (laptop, desktop, phone) makes a call to the API, which then makes a call to the server.  

 

Client to server is how HTTP connections work, so this shouldn’t be a new concept for you.  

Stateless means that there is no memory of the previous request. For example, if you make a call to a RESTful service, then make another subsequent call, the server will not be able to serve that second request with any information from the first. It’s completely unaware of these subsequent requests and treats them independently.  

The last one is the HTTP method. GET and POST are the most common, so that’s what we’ll focus on.  

 

## Calling a 3rd party API 

 

Let’s try calling an API that’s currently available. There is an open one that should remain open as long as we keep our requests light, and that’s https://openweathermap.org/current 

Click the Sign-in or Sign Up button if you don’t have an account already. Go ahead and create an account.  

Once logged in, click API keys  

 

 

If you don’t have a key already filled in, you should be able to generate one.  

 

There are all kinds of ways to get the API data, but for this example, we’ll simply request the zip code and get back some weather info. The request is  

http://api.openweathermap.org/data/2.5/weather?zip=12345,us&appid=YOURAPIKEY 

 

Replace YOURAPIKEY with, you guessed it, your API key that you generated. You can also change zip=12345 to be replaced with the zipcode you want to input. US is the US country code, so don’t change that.  

You can put this directly into a web browser and view the JSON that is returned.  

 

You can also do it in your code. Let’s do this with Python.  

Create a Python file in your Python file called weatherapi.py.  

 

Now, let’s write our script.  

 

For this, we’ll be parsing json and also making a web request. Let’s import the modules we’ll need for this: 

 
```python
import json 

import requests 
```
 

Now, let’s just try calling the API and displaying the data, before we ask for input.  

Use the requests.get to get the data and assign it to a variable called r.  

 
```python
r = requests.get('http://api.openweathermap.org/data/2.5/weather?zip=12345,us&appid=YOURAPIKEY') 
```
 

Now, the data comes back as text as far as python is concerned. We need to tell python that the data is JSON, then assign that to it’ 

 
```python
data=r.json() 
```
 

Now, let’s print the data 

 
```python
print(data) 
```
 

It’s JSON, but it’s not pretty. We can parse it, though, to get exactly what we want. If you take a look, you’ll see one of the top attributes is called “weather”. Let’s parse to get this with the following syntax.  

 

print(data['weather']) 

 

Closer, but I think I still want just the “description”. We can parse it further using the following syntax 

 
```python
print(data['weather'][0]['description']) 
```
 

 

What’s with the [0]? JSon typically returns all of the ‘weather’ attributes, which in this case is only 1. Some cases may return more, so to tell Python which object to grab, we pass a zero-based index value of [0] to tell it to grab the first object. And from that first object, grab the description. Now, let’s prompt for our zip code and assign that value in our URL string using string substitution.  

 
```python
import json 
import requests 
 
print('Please enter your zip code:') 
zip = input() 
 
r = requests.get('http://api.openweathermap.org/data/2.5/weather?zip=%s,us&appid=YOURAPIKEY' % zip) 
data=r.json() 
print(data['weather'][0]['description']) 
```

 

And that’s it, it’s as simple to call json an dig in further.  

Create your Own API 

Now we’re going to write our own API. Let’s do this using Node, because that is the simplest way to create a web server, which we’ll need to serve our API.  

 

In your Node folder, create a new file called api.js  

 

In your file, let’s add our http module, just like we did in lab 5 

These are the basic trappings of an HTTP request server, just like we did in lab 5, only this is a stripped down version.  

 
```javascript
var http = require("http"); 
 
var server = http.createServer(function(req, res){ 
    if (req.url === "/") { 
        res.writeHead(200, {"Content-Type": "text/json"}); 
        res.end(); 
    } 
else { 
        res.writeHead(404, {"Content-Type": "text/plain"});       
        res.end("Data not found");         
    } 
}); 
 
server.listen(3000); 
console.log("Server is listening on port 3000"); 
```

 

Now, we need to get some JSON data to serve to our API users. JSON is the typical data type returned with APIs. It allows javascript the ability to read, filter and display the data to the user very easily. Let’s add a few things to our server to allow us to serve our json data.  

 

First, download the json file from Canvas under Assignments, Lab 9, click “Download widgets.json”. Right-click | Save Link As…  | Save to a directory that you’ll remember (eg. C:\temp)  

 

If you’re on a linux machine, you can just click the file from Windows and copy/paste the contents to a text file on your machine. Save it as widgets.json.  

 

Right under the first line, we’ll pull in our data, just like we did our “http” module. To do this, we’ll just ‘require’ the data.  

 
```javascript
var http = require("http"); 
var data = require("C:/widgets.json"); 
```
 

The “require” value for your data should point to the path of your widgets.json file that you just downloaded. The path on linux will be something like /home/username/widjets.json) 

 

Now, in your server, we need to return our data to make sure it’s working. To do this, find the res.end() entry and edit it to look like this:  

 
```javascript
var server = http.createServer(function(req, res){ 
    if (req.url === "/") { 
        res.writeHead(200, {"Content-Type": "text/json"}); 
        res.end(JSON.stringify(data)); 
    } 
    else { 
        res.writeHead(404, {"Content-Type": "text/plain"});       
        res.end("Data not found");         
    } 
}); 
```
 

JSON.stringify is a delivered Node feature that will take our data and return it as json.  

Also notice we are telling our Content-Type to be “text/json”, so our client (your browser) will know what kind of data they’re getting back.  

 

Save this file and run it.  

 

`$ node api `

 

Open a browser and browse to your web server on port 3000. You should see the data listed as such 

 

 

Take a look at the data for a second. We have 4 items in the JSON data, denoted by the curly braces { } around each object. The whole thing is wrapped in square brackets [ ], which matches the JSON format.  

We have widget1, 2, 3 and X; each with a “color” attribute.  

With this knowledge, let’s go back to our code.  

We’re going to write a function that filters this JSON data, then we’re going to add another route for our API to return only the ‘blue’ widgets.  

Let’s create our function. At the bottom of your api.js file, add the following function called listBlue().  

 
```javascript
function listBlue(res) { 
    var colorBlue = data.filter(function(item) { 
        return item.color === "blue"; 
    }); 

    res.end(JSON.stringify(colorBlue)); 
} 
```
 

 

This function listBlue() takes a `response` object, creates a variable that is assigned the value of a callback function that filters the each returned object in our JSON (4 total) by the `item.color === “blue”` (2 total).  

We then return the JSONified data with our `response.end`.

 

With that function in our code, we can add another `route`, which is as simple as adding an additional ELSE IF statement to our server. A route is also known as a path, which is the `/` at the end of the URL. In this case we’re adding a route for `/blue`.  

 
```javascript
var server = http.createServer(function(req, res){ 

    if (req.url === "/") { 

        res.writeHead(200, {"Content-Type": "text/json"}); 

        res.end(JSON.stringify(data)); 

    } 

    else if (req.url === "/blue") { 

        listBlue(res); 

    } else { 

        res.writeHead(404, {"Content-Type": "text/plain"});       

        res.end("Data not found");         

    } 

}); 
```
 

Now, when a user requests /blue, they should only see the widgets with color = ‘blue’. Let’s try it out. Restart the node server.  

 `$ node api`

In our browser, request `http://localhost:3000/blue`

Now we only have 2 widgets. We can also still browse to ‘/’ to get all the widgets, and a route that doesn’t exists, like ‘/red’ will return a 404.  

Congratulations, you just wrote your first API.  

 

## LAB 9 

Using the Python code example we wrote to call a 3rd party API, and the Node code we used to create our own API, create a Python script that calls your Node API and returns a list of the widget names and their respective colors in an easy to read format, like: 
```
Widget1 is blue.  
Widget2 is green.  
```
Etc… 
 
The best way to do this is to run your NodeJS script and python script on the same server. Run the node API server you created, then call it using localhost:3000 in your python script.  

Formatting the output is important in this lab.  

 

 

 

 

 

 