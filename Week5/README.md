List of Links for this week: 

- Python https://www.lynda.com/Python-tutorials/Learning-Python-3-Standard-Library/550133-2.html 
- Python Books – Automate the boring stuff - https://automatetheboringstuff.com/  

 

# Python 

## Installing Python on Windows 

 

Go to https://www.python.org/downloads/windows/ 

Under Python 3.X.X, select “Download Windows x86-64 executable installer” 

Run the EXE 

Check “Add Python 3.X.X to PATH” and click “Install Now” 

Once finished, click Close. You don’t have to disable path length limit, but you can if you want to.  

Restart PowerShell. Run the above command to confirm Python is working.  

**DO NOT USE PYTHON FROM THE WINDOWS STORE. It doesn't work right** 

 ## Installing Python on Linux 

Python 3 comes installed with Oracle Linux 8, so we don't have to do anything. 


## Verify Python installation 
Open the terminal and verify Python is installed properly 

`python --version`

Then enter the Python terminal by typing `python` by itself 

```
$ python

Python 3.6.8 (default, Mar  9 2021, 15:28:46)
[GCC 8.3.1 20191121 (Red Hat 8.3.1-5.0.1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> _
```

## Python Commands:
Enter a simple math command  
```python
>>> 2 + 2 

4 

 

>>> 4 - 2 

2 

 

>>> 2 * 3 

6 

 

>>> 22 / 2 

11 
```


Order of operations is important in Python, as you’ll see here and later in the lab.  
```python
>>> (5-2) * (7+2)  

27 
```
 

Some Python Data Types: 

INTEGERS are WHOLE NUMBER: -2, -1, 0, 1, 2, 3, 4, 5… 

FLOATING-POINT numbers include decimals: -1.25, -1.0, 0.0, 1.0, 1.75 … 

STRINGS are collections of text:	‘a’, ‘hello’, ‘I have 8 cats’ 

 

Sometimes, you can combine number and string data 

 
```python
>>> print(‘Hello’) 

‘hello’ 

 

>>> print (‘Hello’ * 5) 

‘hellohellohellohellohello’ 

 ```

And sometimes not 

 
```python
>>> print(‘hello’ + 5) 

Traceback (most recent call last): 

  File "<stdin>", line 1, in <module> 

TypeError: must be str, not int 

 ```

You can concatenate strings by ‘adding’ them together 
```python
>>> print(‘hello, ‘ + ‘world!) 
```
 

In my previous example, if I make the + 5 a “string”, I can use the “+” sign to concatenate them: 
```python
>>> print("hello" + "5") 
```
 

 

You can assign values to VARIABLES 
```python
>>> firstnumber = 42 

>>> secondnumber = 58 

>>> firstnumber + secondnumber 

100 

 ```

To quit out of the python shell, type: 
```python
>>> quit() 
 ```

Rules of variables names: 

1. Variable names can be only ONE word. 

2. Variable names can use only letters, numbers, and the underscore (_) character. 

3. Variable names can’t begin with a number. 

 

Let’s write a program that takes an input.  

In your Python git folder, create a new file that’s called ‘nameage.py’ 

Open it in the editor of your choice. This script is going to take inputs from the users and return them in the shell. 	 

```python

print(‘What is your name?’) 

myName = input() 

print(‘Hello, ‘ + myName + ‘. That is a good name. How old are you?’) 

myAge = input() 

print(myAge+’? That’s funny, I'm only a few seconds old.’) 
```
 

Let’s try to run this script.  

And… it works? Assuming all of your single quotes and parenthesis are in the right place.  

 

Let’s add some to this script.  

After this last print, let’s do another print that says “I wish I was (age x 2) years old”:  

 

Let’s just trying passing in myAge*2 and see what happens: 
```python
print(“I wish I was “ + myAge * 2 + “ years old”) 
```
 

Take a look at the output. Assuming your age was 20, you would see the output “I wish I was 2020 years old”, which is not what we expected. Since myAge is a string, because that’s how it was input, we have to convert it to a number so we can use our math against it. We do this by CASTING.  

 ```python
print(“I wish I was “ + int(myAge) * 2 + “ years old”)  
```
 

With the CAST to int saved, run the script.  

Oops, now we get a new error: 

 

`TypeError: Must be str, not int `

 

So we went from a string to an int, but we can’t concatenate our int to our string, because int does not equal string….  

So, we need to re-CAST our int back into a string after we do the math on it. To do this… 

 
```python
print("I wish I was " + str(int(myAge) * 2) + " years old") 
```
 

Order of operations is important, as I stated earlier. Working from the inner-most parentheses, we cast our MyAge variable to int, multiply it by 2, then cast the results to a string. Then we concatenate it into our output.  

Alternative to this, we can cast our input when we get it. Just change  

`myAge=input()`

to 

`myAge=int(input())`

Then remove the int() cast in our output.  

 

Then we can use variable substitution (remember our bash script) in our string to make is output as a string automatically. Using %s in our string tells Python that this needs to be a string variable, no matter what we pass into it. Therefore, it will automatically CAST our variable to a string: 

 

 ```python

myAge = input() 

print('%s? That’s funny, I'm only a few seconds old.' % myAge) 

print("I wish I was %s years old" % myAge) 
```
 

Don’t forget to apply our * 2 to our age, but watch out cause order of operations is at play again. Try it with and without the parenthesis around myAge * 2 

 
```python
myAge = input() 

print('%s? That’s funny, I'm only a few seconds old.' % myAge) 

print("I wish I was %s years old" % (myAge * 2)) 
```
 

Let’s add a bit more to this script… 

Let’s have the script sleep for 3 seconds after the last question, then sign off.  

Add the following two lines to the bottom of the script 

 ```python

time.sleep(3) 

print(“I’m tired. I go sleep sleep now.”) 
```
 

Try to run it now. You’ll get the same questions, then the end will ERROR?  

 

`NameError: name ‘time’ is not defined `

 

Why? 

 

Python works with modules. It comes delivered with TONS of modules, but you have to import them. “Time” is part of the “Standard Library” of Python modules. Let’s import the “time” module so we can use the sleep function.  

At the top of the file, type 

 

`import time `

 

Save and rerun the script. This time you should get a final output after 3 second.  

We can also use this module to get the run time of this application, and replace “I’m only a few seconds old” with the actual length of time the program has been running.  

 

To do this, we need to think it through a bit. Let’s use our python command line to work out a solution to get the length of time the program has actually been running. Hop into the Python shell (in Windows or Linux) 

Let’s work through this line by line 

First, we import the time module, so we can use it.  

```bash
$ python
Python 3.6.8 (default, Mar  9 2021, 15:28:46)
[GCC 8.3.1 20191121 (Red Hat 8.3.1-5.0.1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import time
```

Time has an object time with property time, we can print out the current time with  

 

`>>>time.time() `

This gets us a crazy, unreadable output, but if we run it multiple times, we start to see that the number just left of the decimal is 1 second. We don't care about the rest of it, so let’s cast our time to an integer. 

  

`>>> int(time.time()) `

Now to get the current time, we can assign our start time to a variable when the program starts. Let’s try that now 

```
>>>start_time = time.time() 

>>>start_time 
```

start_time is set to whatever time.time() was when we ran this command. Then we can subtract the current time from our start time to get the difference 
```
>>>time.time() – start_time 

>>>int(time.time() – start_time) 
```
 

The output should be a few dozen seconds, and will increase each time you run it. Now that we know what works, we can add it to our script.  

Right under import time 

`start_time = time.time() `

 

under myAge input, lets assign our calculated time to a variable 

`programAge = int(time.time() – start_time) `

 

Add to funny string. Just like above, we have to cast our int to a string in order to use it in our output.  

 
```python
print("%s? That’s funny, I'm only %s seconds old." % (myAge, programAge)) 
```
 

Save and run. You should have a successful run. If not, let’s find out why.  


Now, we can expand on this program, adding a few more fun things. First, let’s commit our script to GIT. This is a good habit to get into, so let’s do it.  
 

Now, reopen our script and let’s add some logic in here. Let’s start simple and do something like “if myAge < 13 ….” 

 

Open the script and right after we set myAge, let’s add some logic on a new line: 

 
```python
if myAge < 13: 

  print("Learning young, that’s good") 

elif myAge == 13: 

  print("You're a teenager now... that's cool, I guess") 

elif myAge > 13 and myAge < 30: 

  print("Still young, still learning…") 

elif myAge >= 30 and myAge < 65: 

  print("Now you’re adulting.") 

else: 

  print("... you’ve lived a long time?") 
```
 

This will print different results based on the number we input.  

Save and run it.  

 

We can do something similar with our name. Let’s have the program continuously ask for our name, until we get it right. To do this, we’ll use a while loop: 

 
```python
myName = input() 

while myName != 'your name': 

  print('This is not “your name”. Please type “your name”?') 

  myName = input() 

print('Hello, ' + myName + '. That is a good name. How old are you?') 

myAge = int(input()) 
```
 

We can even nest these in if statements. Let’s catch the ‘joke’: 

 
```python
myName = input() 

while myName != 'AJ': 

  if myName == 'your name': 

    print('Ha ha, very funny. Seriously, who are you?') 

    myName = input() 

  else: 

    print(‘That is not your name. Please, tell me your real name.') 

    myName = input() 

print('Hello, ' + myName + '. That is a good name. How old are you?') 
```
 

An alternative way to do this is to loop until a TRUE statement is reached, then break. In a separate file called `yourname.py`: 

 
```python
while True: 

  print('Please type your name.') 

  name = input() 

  if name == 'your name': 

    break 

print('Thank You') 
```

Now that we got our Python feet wet, let’s write a few more scripts to give you some basic examples of what you might run in to with Python.  

 

Now, since we’re supposed to be scripting network and security tasks, let’s do that.  

Create a new file called `sockettest.py`

 

Here is some code that uses a FOR loop. First, let’s import the modules we’ll be using.  

```import socket```

 

Then, we’ll define an array of hostnames. 

 

`hosts = [‘www.uc.edu’, ‘www.google.com’, ‘www.bing.com’] `

 

now, we can iterate through this list with a FOR loop 

 
```python
for h in hosts: 
  print(socket.gethostbyname(h))
```
 

 

Here is the full script: 

 
```python
import socket 

 

hosts = ['www.google.com', 'www.bing.com', 'www.uc.edu'] 

print ('Working from host: ' + socket.getfqdn()) 

for h in hosts: 

  print (h + ': ' + socket.gethostbyname(h)) 

 ```

 

socket.getfqdn() without any parameters will display the current hosts information.  

We can use the input from the terminal as well to find an IP. This is essentially just doing a ping, but maybe we can make it more useful later.  

 

To do this, let’s create a new file called `myGetIP.py`

 

Import our modules. We’ll need 2 for this 

 

`import socket, sys `

 

Now, write the code 

 
```python
import socket, sys 

 

hostname = str(sys.argv[1]) 

ip = socket.gethostbyname(hostname) 

print (hostname +' has an IP of ' + ip) 
```
 

We pass sys.argv[1] because [0] is the name of the python script we ran. [1] is the first argument.  

We can catch any potential errors by using a try except statement, errors like putting in an bad hostname 

 
```python
import socket, sys 

 

try: 

  hostname = str(sys.argv[1]) 

  ip = socket.gethostbyname(hostname) 

  print (hostname +' has an IP of ' + ip) 

except: 

  print("Oops, something is wrong with that host") 

 ```

There are several ways to catch errors based on the type of exception. See https://docs.python.org/3/tutorial/errors.html for more details.  

 

One of the best things we can do with Python is create our own functions.  

 

We can also use a Python Function to call this. Simply wrap this code in a def functionname() command and call it: 

 
```python
def getHostnameByIP(h): 

try: 

  hostname = str(h) 

  ip = socket.gethostbyname(hostname) 

  print (hostname +' has an IP of ' + ip) 

except: 

  print("Oops, something is wrong with that host") 
```
 

Now, call from within this script.  

 

`getHostnameByIP(sys.argv[1])`

 

Same results, but now our code is reusable.  

 

One more thing, let’s take a look at one of the log files on our LINUX machine and parse it for certain data.  

This will require us to read in a file from the file system.  

 

Create a new file called `getLogData.py`. You can create this on Windows and move to Linux, or just use your preferred editor on Linux.  

 

Now, we create the file reader and set our file path. For this, we’ll use a text file that we'll create.  

in a file called test.txt
```
this is a test
hello world
this is a test
hello world
```

Create a textfile 

```python

import os

filename = "test.txt" 

with open(filename) as f:
  lines = f.readlines()
  for line in lines:
    if "world" in line:
      print(line)

```
 

This script calls a protected file, so you’ll have to run it using SUDO on linux.  

 

 

LAB 5 

Create a folder called ‘Labs’ at the root of your git repository.  

Create a file called `Lab5.py`. In it, select 1 of the following problems and write a solution in Python.  

Take a birthday date input and calculate how many seconds old you are  

Take a word as input and count how many letters, how many vowels and how many consonants  

Take a number input and calculate how many prime numbers come between it and 0  

Write a ‘guess the number game’ where a random number is generated and the user must guess the number. The program says if their number is too high or too low until the right answer is guessed.  

Submit the github.com URL to your `Lab5.py` file to Assignments Lab 5. 

 

 

 

 
