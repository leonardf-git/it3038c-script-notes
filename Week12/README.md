# Websites in Python using Flask

In this lab, we’ll create a simple web application using Python Flask.  

Flask allows you to use Python to create web apps similar to the way NodeJS does.  

Create web.py file 

Add the following code: 

```python
from routes import app 
 
if __name__ == "__main__": 
    app.run(port=5000, host='0.0.0.0') 
```
 
Create `routes.py` file  

Add the following code: 
```python
from flask import Flask 
 
app = Flask(__name__) 
app.config.from_object(__name__) 
 
@app.route('/') 
def hello(): 
   return 'Hello, World!' 
```
 

Let’s create our Flask venv.  

Here is a shortcut: 

``` 
#Windows: 
py -3 -m venv venv 
venv\Scripts\activate.ps1 

#unix:
python3 -m venv venv 
. venv/bin/activate 
```
 

Once your VENV is activated, Install flask 

`pip install flask`

Now, run your flask app 

`python web.py`

 

Open a web browser and browse to  

`http://localhost:5000`
 

You should see a “Hello, World” page. That’s it! You’ve just created a Flask app. Of course, that’s only the beginning. Let’s see what other options we have.  


Let’s create another route called `/welcome`

There are a few things we’re going to add to this. First, let’s add a template. To add a template, create a new folder called `templates`.  

Within this folder, create a file called `layout.html`

Add the following code:  

 

 
```html
<!DOCTYPE html> 
  <head> 
  </head> 
  <body> 
      <H1>Flask app!</H1> 
      <br /> 
      {% block content %} 
      {% endblock %} 
  </body> 
</html> 
```
 

Let’s create Another file in our templates folder called `welcome.html` 

```html
{% extends "layout.html" %} 

{% block content %} 

<p><em>This is the Welcome page!</em></p> 

{% endblock %} 
```
 

Save all of these files.  

Return to your `routes.py` file and add the following code: 

```python
@app.route('/welcome') 

def welcome(): 

   return render_template("welcome.html") 
```
 

Run the app and go to the welcome page 

`http://localhost:5000/welcome`

 

OOPS! We got an error. Let’s turn on debugging and try to solve this issue.  

Open `web.py` 

Add the following argument to app.run 
```python
    app.run(debug=True, port=5000, host='0.0.0.0') 
```

Rerun the application and open the welcome page again.  

Now we have some more information about our error. We could google it, but I now the solution.  

Return to routes.py and add the following to the first line in your script 

```python
from flask import Flask, render_template
```

Save the file.  

Ahh, notice your command line. Since we are using the `Debug=True` flag, our program will automatically restart whenever we make changes. This is very convenient.  

Open `http://localhost:5000/welcome`

 

There are a few takeaways here, which I’ll highlight by making some additional changes to our file.  

Within `@app.route(‘/’)`, change the it to the following: 

```python
@app.route('/') 
def hello(): 
   name = 'AJ' 
   return render_template("index.html", value=name) 
```
 
Please note the added variable `name` with a value. Also notice that we’ve added the `render_template` function, passing the name of our template and a value of the variable. Let’s see what we can do with this.  

From the templates folder, create a file called `index.html`

Add the following content: 

```python
{% extends "layout.html" %} 

{% block content %} 

<p><em>Hello, {{ value }}. Welcome to my app!</em></p> 

{% endblock %} 
```
 
Save this file and browse to `http://localhost:5000`

A few things to notice: first, our header is the same, since it inherits from the layout.html template, same as our welcome.html page. Second, our variable is displayed on the page. This opens a few doors of possibility for us. Let’s expand on this a bit more. I want the user to input the name themselves, and then display it. There are a few ways to do this.  

Let’s first create a form. Open `index.html` and change it to the following: 
```python
{% extends "layout.html" %} 

{% block content %} 
<p><em>What is your name?</em> 
   <form action="/welcome" method="POST"><input name="myName"><input type="submit" value="Submit"></form> 
</p> 
{% endblock %} 
```

Notice our form action points to `/welcome`. This means we’re going to have to make changes to the `/welcome` route AND the `welcome.html` template. Let’s change the route first.  

First, comment out our name variable from our “/” route, we don’t need it anymore.  
Open `routes.py`

```python
@app.route('/') 

def hello(): 
  #myName = 'AJ' 
   return render_template("index.html")  
   #, name=myName) 
```
 
Next, edit the `welcome` route.  
```python
@app.route('/welcome', methods=['POST']) 

def welcome(): 
   return render_template("welcome.html", myName=request.form['myName']) 
```
 

Next, edit the first line to include the import of the request module: 

```python
from flask import Flask, render_template, request 
```

Finally, edit the welcome.html template: 

```python
{% extends "layout.html" %} 

{% block content %} 
<p><em>Hello, {{ myName }}!</em></p> 
{% endblock %} 
```

Save all files and refresh.  

Congrats! You just created your first flask app. Feel free to add on to this, add some CSS styles, make it pretty, etc.  

Check out https://github.com/miguelgrinberg/microblog/tree/v0.1 which is the official flask tutorial. This application is called flaskr, a microblog built in flask.  
Follow the README on instructions on how to run this.  
 
Once it’s running, take a look at the application. Create an account, login and create a post. Delete or edit the post. Continue until you feel comfortable with the application. Take a look at the code. Locate the routes for each type of request, like add, edit, update, and delete.  

 
## Lab 11

Submit the link to your github folder containing the flask app you created above. It should run and prompt me to input my name, and work when I submit it.  

 
