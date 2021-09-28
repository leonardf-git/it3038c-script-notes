List of Links for this week:
Lynda - https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training/417077-2.html 

# Node	JS Crash Course
## Installing Node on Windows
Download the installer from https://nodejs.org/en/download/

Select LTS 64bit. (LTS is Long Term Support and is typically an even release)

Verify the installation

`$ node --version`

Verify npm is working

`$ npm --version`


## Install Node on Linux

To install Node on Linux, we’ll use a provided RPM install. According to https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora, just run the following:  
```sh
$ curl --silent --location https://rpm.nodesource.com/setup_14.x | sudo bash -
$ sudo yum -y install nodejs	
```

Again, verify 

`$ node --version`
`$ npm --version`


By now we’re getting a feel for how scripting languages work. Node is a script language that takes advantage of Javascript, making it a more ‘familiar’ language for traditional web developers. 
While Node is a web language, it is also capable of many server-side operations as well. We’ll take a look at both sides this week. 

Create a Node folder in your IT3038C-scripts direcotry and create a new file called test.js.
Add the following code

`console.log("Hello, World")`

Save it and run it using 
```sh
$ node test.js

“Hello, World”
```

Simple... now, let’s put this string in a variable. 

```js
var hello = "Hello from Node JS Variable!"
`console.log(hello)`
```

And run it

`$ node test`

And just like other languages, we have a few options for using variables, particularly with string manipulation. 
We can use a TEMPLATE STRING, similar to Python and Bash. 

This code uses the “back tick” character 

```js
console.log (`Printing variable hello: ${hello}`);
```

This will print the string with the variable included. 

Node also includes several global variables, denoted with a double underscore “_ _”

```js
console.log("directory name: " + __dirname); 
console.log("directory and file name: " + __filename); 
```

Node includes several delivered modules, similar to Python. Modules can be added using npm. More on that later. For now, let’s add the “Path” module. 

At the top of the script, add

```js
var path = require("path");
```
A quick aside about the ‘;’ semicolon usage. Semicolons are implied with Javascript, so you don’t HAVE to have it, but it is a best practice. 

The require method tells our code to import this module. Now we can use “path” functions in our code. Add this line to the bottom of our script.
```js
console.log("Using PATH module:");
console.log(`Hello from file ${path.basename(__filename)}`);
```
If you command out the path module using //  you will see that the script will throw an error: 
ReferenceError: path is not defined

Now, similar to Python, we can pass arguments to our scripts and handle it in our code. 
To do this, we can pull the “process.argv” variable. Add this line to the bottom of our script.  
```js
console.log(`Process args: ${process.argv}`)
```

`$ node test`

Notice the output is an “array” with 2 value. The first value is our Node executable, and the second is our full path to our file. 
If I add an argument after the script by running it with some additional parameters, we’ll see those parameters added to this array. 

`$ node test var1`

Create a new file called getIPByHostname.js

First, we need to make sure we have at least 1 argument in our script. There are several ways to do this, but this method is the most straight-forward. 

```js
if (process.argv.length <= 2) {
   console.log("USAGE: " + __filename + " hostname.com")
   process.exit(-1)
}

var hostname = process.argv[2]

console.log(`Checking IP of: ${hostname}`)
```

Now, let’s find the equivalent of NodeJS to find the IP of our hostname. To do this, we’re going to require to import the DNS module. 
At the top of the file, add 

`var dns = require('dns');`

Now, we can write some javascript to get what we want. 

```js
var dns = require('dns');

function hostnameLookup(hostname) {
   dns.lookup(hostname, function(err, addresses, family){
      console.log(addresses);
   });
}

if (process.argv.length <= 2) {
        console.log("USAGE: " + __filename + " IPADDR")
        process.exit(-1)
}

var hostname = process.argv[2]
console.log(`Checking IP of: ${hostname}`)
hostnameLookup(hostname);
```


Don’t worry if this code doesn’t make a lot of sense. That will come with experience. 
The point here is that we have all of the same abilities in Node that we have in most other programming languages, though it may require a few additional lines of code. 


### Get User Input
We can also get input from the user while the script is running. This uses the stdin function. 
Create a new file called getUserInput.js

This file is going to ask a series of questions and generate some responses, similar to what we did last week in Python. 
Let’s start by asking the user for their name: 

First, we ask with the ‘process.stdout.write’ command or console.log. console.log is just process.stdout.write with a line-break at the end, so either one is fine. 

```js
process.stdout.write("Hello. What is your name? ")

process.stdin.on('data', function(data) {
    console.log("Hello " + data.toString())
});
```

Now, javascript runs asynchronously and javascript loves things called callback functions. This means, we’ll have to create a function that runs whenever an event is fired. In this case, our event is called ‘process.stdin.on’ and we can pass our information, the “data” variable, to our ‘callback function’ that will handle the rest. 
Run the script. 

Now it’s taking our input, but it’s not ending. We can end it by adding `process.exit()`. 

We can also call a function on exit of the application, like so 
```js
process.stdout.write("Hello. What is your name? ")

process.stdin.on('data', function(data) {
    console.log("Hello " + data.toString())
    process.exit()
});

process.on('exit', function() {
    console.log('Thanks for the info!')
});
```

Let’s get to the bread and butter of Node, and that is the web server.
To do this, we will need the HTTP module. 
Create a new file called server.js

```js
var http = require("http");

var server = http.createServer(function(req, res){
    res.writeHead(200, {"Content-Type": "text/text"});

    res.end(“Hello from Node!”)

})

server.listen(3000)

console.log("Server listening on port 3000")
```

This is a simple example of a web server. There are a few ways to hit this web server. 
You can use curl from linux, or you can use a browser on your Windows machine. 

Just open a web browser and type in localhost:3000. 

`http://localhost:3000/`

Linux NOTE: You can call a website from command line using curl:

```sh
curl localhost:3000
```

You can also hit it from your Windows machine if you disable the linux firewall and call the IP address of your linux machine in a web browser:

`sudo systemctl stop firewalld.service`

Remember our use of the backtick earlier? We can apply that here and add full html to our response, with variables included
```js
var http = require("http");

var server = http.createServer(function(req, res){
    res.writeHead(200, {"Content-Type": "text/html"});

    res.end(`
        <!DOCTYPE html>
        <html>
          <head>
            <title>Node JS Response</title>
          </head>
          <body>
            <h1>Hello!</h1>
            <p>${req.url}</p>
            <p>${req.method}</p>
          </body>
        </html>
    `)

})

server.listen(3000)

console.log("Server listening on port 3000")
```

This makes it easy to include information in our responses. We can modify this a bit to make it a little more usable, by changing the res.end to use a body variable and setting the body variable to our content. We can also chain our listen(3000) function to the end of our web server. 
```js
var http = require("http");

var server = http.createServer(function(req, res){
    res.writeHead(200, {"Content-Type": "text/html"});

    res.end(`
        <!DOCTYPE html>
        <html>
          <head>
            <title>Node JS Response</title>
          </head>
          <body>
            <h1>Hello!</h1>
            <p>${req.url}</p>
            <p>${req.method}</p>
          </body>
        </html>
    `)

}).listen(3000)

console.log("Server listening on port 3000")
```

This gets us the same results with a little less code. 

Right now, regardless of the request, our NODE site serves the same page with a dynamic variable. If you go to a subpage of our site, you’ll get see our content change on the page, but this isn’t very practical. Let’s use a module for reading from the file system to get straight HTML content. Create a new folder called Public within your Node folder. Create a file called index.html within the Public folder. 

Put the following HTML code in Index.html:
```html
<!DOCTYPE html>
    <html>
      <head>
        <title>Node JS Response</title>
      </head>
      <body>
        <h1>Hello! Welcome to my site!</h1>
      </body>
    </html>
```

Return to server.js. 

Let’s add an additional path to our server. To do this, we need to account for every request coming into our web server. 
The default request of / will serve our static HTML message.

Any other request should return a 404 error, using the req.url variable we defined earlier to give the user a clearer message. 

```js
var http = require("http");

http.createServer(function(req, res){

    if (req.url === "/") {
        fs.readFile("./public/index.html", "UTF-8", function(err, body){
        res.writeHead(200, {"Content-Type": "text/html"});
        res.end(body);
    });
}
    else {
        res.writeHead(404, {"Content-Type": "text/plain"});
        res.end(`404 File Not Found at ${req.url}`);
    }
}).listen(3000);

console.log("Server listening on port 3000");
```


Now, we’ll add another path on to our web server that can include anything we want. How about we add a /sysinfo path and serve some system information to it. 
I want sysinfo to return information about the OS, so I’m going to include the require(“OS”) module. I’m also going to add the IP module to get our IP address. 

```js
var http = require("http");
var fs = require("fs");
var os = require("os");
var ip = require('ip');

http.createServer(function(req, res){

    if (req.url === "/") {
        fs.readFile("./public/index.html", "UTF-8", function(err, body){
        res.writeHead(200, {"Content-Type": "text/html"});
        res.end(body);
    });
}
    else if(req.url.match("/sysinfo")) {
        myHostName=os.hostname();
        html=`    
        <!DOCTYPE html>
        <html>
          <head>
            <title>Node JS Response</title>
          </head>
          <body>
            <p>Hostname: ${myHostName}</p>
            <p>IP: ${ip.address()}</p>
            <p>Server Uptime: </p>
            <p>Total Memory: </p>
            <p>Free Memory: </p>
            <p>Number of CPUs: </p>            
          </body>
        </html>` 
        res.writeHead(200, {"Content-Type": "text/html"});
        res.end(html);
    }
    else {
        res.writeHead(404, {"Content-Type": "text/plain"});
        res.end(`404 File Not Found at ${req.url}`);
    }
}).listen(3000);

console.log("Server listening on port 3000");
```


OOPS, something is missing when I run it. 
Well, for starters, IP is not a delivered module. So we need to install it. To do this, type

`npm install ip`

Now run it again. 
We’ll get into modules more in the coming weeks. 
Let’s do one more thing to our index.html page. Add a hyperlink to the /sysinfo page
```html
<!DOCTYPE html>
    <html>
      <head>
        <title>Node JS Response</title>
      </head>
      <body>
        <h1>Hello! Welcome to my site!</h1>
   		<p><a href='/sysinfo'>Get Info About This System</a></p>
      </body>
    </html>
```


For our assignment, we’ll be filling in the blanks on our sysinfo page above. 

LAB 6
- Create a Node web server and display the following data:
- In addition to the hostname and IP as shown above, add in the server uptime in Days, Hours, Minutes, Seconds (as in the sample below), Total Memory in MB, Free Memory in MB, and Number of CPUs. 
- Use https://nodejs.org/api/os.html for more info. Some *HINTS*: 
- CPU is an array. That has to count for something
-	Memory is returned in bytes. How many bytes are in a Megabyte? 
-	Server time is returned in seconds. How many seconds are in a day? Subtract as many days as you can. Now subtract at many hours as you can. Now subtract as many minutes as you can... Etc… 
-	Google is your friend. If you’re stuck, you can typically find how to get these outputs via Google. 

Here is a sample page: 
![](/screenshots/21-09-28-13-20-59.png)

