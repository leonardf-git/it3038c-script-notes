Links:  

- Regex Cheat Sheet - https://www.debuggex.com/cheatsheet/regex/python 
- Beautiful Soup - https://www.crummy.com/software/BeautifulSoup/bs4/doc/ 
- PythonForBeginners - http://www.pythonforbeginners.com/beautifulsoup/beautifulsoup-4-python  
- Regular Expressions 101 - https://regex101.com/  
- Regexdrill - https://twitter.com/regexdrill 

 

## Web Scraping 

 

Let’s do some web scraping – a tool that can be used for both good and evil. Please choose good and be careful, too many scraping attempts can get you blocked or bring a web server to its knees.  

 

First, let’s create a VENV for our scraping environment. If you didn’t already, to do this, run the following commands. This is linux using my users home directory. 

 

### Linux 

 
```bash
$ virtualenv ~/venv/webscraping 

  Using base prefix '/usr/local' 
  New python executable in /home/botheaj/venv/scraping/bin/python3.6 
  Also creating executable in /home/botheaj/venv/scraping/bin/python 
  Installing setuptools, pip, wheel...done. 

$ source ~/venv/webscraping/bin/activate 

(scraping) 
```

`virtualenv --python=/usr/bin/python3 ~/venv/webscraping`

### Windows 

```powershell
PS C:\Users\Administrator> virtualenv venv\webscraping 

Using base prefix 'c:\\python36' 

New python executable in C:\Users\Administrator\venv\scraping\Scripts\python.exe 

Installing setuptools, pip, wheel...done. 

PS C:\Users\Administrator> .\venv\webscraping\Scripts\activate.ps1 

(scraping) PS C:\Users\Administrator> 
```
 

Run pip install for BeautifulSoup4 and two other modules, lxml and html5lib.  

`pip install bs4 lxml html5lib requests`

 
Now, create a new file in your Python directory and call it webscraping.py 

 
```python
from bs4 import BeautifulSoup 
import requests, re 

r = requests.get('https://analytics.usa.gov').content 
soup = BeautifulSoup(r, "lxml") 
print(type(soup)) 
print(soup.prettify()[:100]) 
for link in soup.find_all('a'): print(link.get('href')) 
```
 

I’m going to break this file down for you, just so we’re all on the same page. Some of this stuff I’ve iterated a few times, but it’s important to understand.  
 
```python
from bs4 import BeautifulSoup 
```
 
We are importing the BeautifulSoup module from our bs4 package. The difference between importing BeautifulSoup and importing all of bs4 has to do with best practices. If you’re working in a large application, you may run into problems with global variables being overwritten by the import of a module, so it is generally frowned upon when importing an entire modules when you only need a function of it.  

That being said, we are going to break these rules anyway and import requests and re, which is used for regular expressions. More on that later.  

 
```python
import requests, re 
```


Now to get the content of a web page, we simply need to do a GET on that page. 

```python
r = requests.get('https://analytics.usa.gov').content 
```

Now “r” contains the content of the page analytics.usa.gov. We can then turn that into what’s called a “Soup” object and use it for parsing the data into readable chunks. 


TIP: If you’re wondering what the datatype of a variable might be, simply do type(varname) 

 
```python
type(soup) 
<class ‘bs4.BeautifulSoup’> 
```
 
With this, we can print the page out more “beautifully” 

`print(soup.prettify()[:100])`

--the :100 signifies to only apply to the last 100 lines in the return, so as to keep the output smaller. 

 

We can parse through the data, too.  

Let’s see if we can extract links from this page. We know that in HTML a link is created using an `<a href=’’>` tag, so we just need to find all the “a” tags and print the “href” part out.  
 
```python
for link in soup.find_all('a'):  
    print(link.get('href')) 
``` 

That’s that. Run this code and see what your output is. 

You should see a list of all of the HTTP links on the page.  

We can also loop through and print using Regular expressions, so any href that starts with http will show up on our page.  

 
```python
for link in soup.find_all('a', attrs={'href':re.compile("^http")}):  
    print(link.get('href')) 
```
 

This will return the entire html tag, which is fine. We can go one step further and say we only want links from a certain site, like github.com 

 
```python
for link in soup.find_all('a', attrs={'href':re.compile("^https://github.com")}):  
    print(link) 
```
 

Let’s take a look at the re.compile statement. Notice the ‘^’ icon, which is used in regular expressions. As a reminder from our last class, here is what RegEx can do for you and how it works:  

 

### Regular Expressions 

Regular Expressions or “reg-ex” is a set of characters that act as a ‘catch’ to other text. This text can be read by a computer to interpret expected content and validate it.  

 
An example of validation might come in the form of an email address. When a user submits a form to a web page, we want to validate the values are legit, so we use regular expressions to validate the form before we send it to the web server. An email address contains the following: 
 

Multiple alphanumeric characters and some special characters before the @ symbol 

Multiple alphanumeric characters and some special characters after the @ symbol 

A “dot” (.) Or multiple “dots” (.)  

Multiple alphanumeric characters and some special characters after the @ symbol (in this example, limited to .com, .net, .org, .edu) 

Here’s that website we used last week to test regex:  

https://regex101.com/ 

 

And here’s a Cheat Sheet: 
```
^The – Starts with “The” 

End$ - Ends with “End” 

^The End$ - Starts with “The” and ends with “End” (exact match) 

The* End - matches string followed by 0 or more “e” (The End is valid, the ‘e’ is totally optional) 

The+ End - matches string followed by 1 or more “e” (‘Theeeee End’ is valid, but not ‘Th End’) 

The? End – may or may not have an “e” (The End, Th End, but not Thee End) 

[The ]+End$ - And characters “t”, “h”, “e” or <space>, but nothing else before or after “End”.  ### The square brackets indicate that you can specify that list of characters.  


[A-Z] – Any Capital Letters 

[a-z] – Any Lowercase Letters 

[0-9] – Any Numbers 

[A-z0-9] – Any letters or Numbers, no special characters 

[@%_-.] – Special characters 

\$ - Regex characters used to validate the string need to be ‘escaped’ with a “\” 
``` 

Let's do a quick regex test: 

Create a file called regex.py 

Let’s test the following code: 

 
```python
import requests, re 

data = "Hello. My email is botheaj@ucmail.uc.edu. Please contact me!" 
p = re.compile('[A-Za-z0-9_%.-]+@[A-z0-9_%.-]+\.[A-z0-9]{2,}') 
print(p.search(data).group()) 
```

This code will take our string called data and pull out just the email address. Re.compile takes our regex as an argument and the search function takes our string and outputs the results compared to the regex.  

Now, let’s get back go BeautifulSoup.  

For our next example, let’s use a test scraping site: http://webscraper.io/test-sites/e-commerce/allinone/phones 

Let’s create new file called webscraperio.py 

We’re going to take the input of the webpage, run it through BeautifulSoup and parse the output using RegEx.  

First, import your modules 

```python
import requests, re 

from bs4 import BeautifulSoup 
```

Then, create the request and assign the output of it to a variable called “r” 
 
```python
r = requests.get("http://webscraper.io/test-sites/e-commerce/allinone/phones").content 
```

Now, run r through BeautifulSoup and assign it to another variable called “soup” 

```python
soup = BeautifulSoup(r, "lxml") 
```

Let’s use our soup data to find all of the links on this page. Let’s create a variable called “tags”. This variable will use soup.findAll to search through the page’s content using regular expression.  

We can change our compile code another look for specific text in the href 

```python
tags = soup.findAll("a", {"href":re.compile('(allinone)')}) 
for a in tags: 
        print(a.get('href')) 
```
 

That is pretty simple, but we can get as complex as we want. Change your regex check to something different: 

 
```python
tags = soup.findAll("a", {"href":re.compile('[<>#%|\{\}!\\^~\[\]`/]')}) 
for a in tags: 
        print(a.get('href')) 
```
 
Let’s try searching for all reviews on this page.  

Look at the page source and find the reviews. It looks like the reviews are in a “ratings” div class. Let’s try pulling that.  

 
```python
import requests, re  
from bs4 import BeautifulSoup

r = requests.get("http://webscraper.io/test-sites/e-commerce/allinone/phones").content  
soup = BeautifulSoup(r, "lxml")  
tags = soup.findAll("div", {"class":re.compile('(ratings)')})  
for p in tags:  
        a = p.findAll("p",{"class":"pull-right"})  
        print(a[0].string)  
```
 

Notice the use of the lxml parser when we define our BeautifulSoup object names “soup”.  

Let’s do one more scraping example:  

The method for defining a scraping depends entirely on the way the page is structured. To view this, open a page in chrome, select the information you want to extract, right click and inspect.  

Reebok’s website is super easy to scrape data from. Go to Reebok and select an item to get to the item page. Using the inspector, we can scrape the price and the name of the item on a page. 

Open the page https://www.reebok.com/us/flexagon-energy-shoes---preschool/DV8354.html  

Right-click on the title and click “Inspect” 

Notice the Tag (h1) and the class name. This is what our code will look for.  

Repeat this process for the price of the item on this page.  

Using this knowledge, we can add it to our code to extract the values. Let’s go through this line by line.  
 
```python
import requests, re 
from bs4 import BeautifulSoup 
 
data  = requests.get("https://www.reebok.com/us/flexagon-energy-shoes---preschool/DV8354.html").content  
soup = BeautifulSoup(data, 'html.parser')  
span = soup.find("h1", {"class":"product_information_title___2rG9M product_title gl-heading gl-heading--m"})  
title = span.text 
span = soup.find("span", {"class":"gl-price__value gl-price__value--sale"})  
price = span.text 
print("Item %s has price %s" % (title, price)) 
```
 
Notice a few things: we’re using the html.parser. That’s because it just works better in this case. Reebok’s site has very well-formed HTML, a common attribute of a large site like that. Other sites that don’t have well-formed HTML benefit from using the LXML parser because it parses content differently and makes it easier to get the data. The point it there is no right or wrong way to do it, just use the best parser for the job.  

## Lab 8 

Using a website of your choice, extract some valuable data from it and print it to the console. Print the output in a matter that makes sense, for example, if you’re scraping a store page, print the item name and price on the same line. Be careful, because multiple hits to a website may prevent you from further visiting that site. Also, some sites immediately block scraping attempts. If this happens, select a different site. Commit your script to github and submit a portion of the output from your script.  

 

 