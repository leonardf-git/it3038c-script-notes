# Heroku 

### List of Links for this week: 

Links: https://www.heroku.com/ 

Let’s create a Heroku account. To do this, click the link above and create follow the instructions to create an account. Windows or Linux is fine. I’m using Windows because a lot of browser work will need to happen.  


`npm install -g heroku`

Once Heroku is installed, run  

`heroku login`

 

This will prompt you to login or open a browser to complete your login. If it fails, CTRL-C the script and re-run it. You may have to set your default browser to Firefox or Chrome on your server to complete this step.  

Enter your email address and password. Return to the terminal and you should see a “logged in as” message, indicating a successful login.  

 

### Code Considerations 

If you’re storing your code on a public remote repo like github.com, it’s important to protect your passwords and machines. Scrapers are constantly checking newly added repos for passwords that were left in the code so that they can use them to access your data.  

Here are a few ways you can protect your code:  

 

Have a template file for your config, but keep the real file in your .gitignore 

Store your passwords as environment variables, not in your code 

Hash your passwords in code – this is much more difficult and using env variables is much better.  

Deploying Code for Production  

Let’s walk through a scenario where you might deploy and application.  

To do this, we’re going to use Heroku. We’re also going to create a simple app to host there.  

 

If using github on Linux, run:  

`yum update -y nss curl libcurl`

 

On Windows or Linux, run:

`git clone https://github.com/heroku/node-js-sample.git`

 

If you’re having trouble with github, you can download the file at  

https://github.com/heroku/node-js-sample/archive/master.zip

 
Open this directory in VSCode.  

Find the `index.js` file and edit it. You’ll notice there is a bit of code already written for you. This invokes the `express` module, a popular NodeJS module for web servers.  

 
At the top of the file, add 

`var config = require('config')`

 

Edit `response.send` to include `config.util.getEnv(‘MYNAME’)` and `config.util.getEnv(‘NODE_ENV’)`, something like this: 

 
```javascript
response.send('<b>Hello World! My name is = <em>' + config.util.getEnv('MYNAME') + '</em <br /> My Node Environemnt is :' + config.util.getEnv('NODE_ENV') + '</em></b>') +
```
 

Go ahead and open the terminal in VSCode and run: 

 
```bash
npm install
npm install config
```
 

NPM install will install all of the modules listed in the delivered “package.json” file. We also install the config modue. Config gives us options for assigning environment variables.  

Two ways to set environment variables:  

 

Set a system environment variable from the command line, run: 

On Linux: 

`export NODE_ENV='Testing'`

 

On Windows: 

`$env:NODE_ENV='Testing'`


Start the application
```
node index 
```
 

Browse to http://localhost:5000 in your browser 

You should see something like this:  
![](/screenshots/21-11-03-07-13-15.png)
 

 

We need to define our variable for `MYNAME`, but let’s do that a different way.  

Set the variable before launching the node app. From the command line, run: 

On Linux, run  
```
MYNAME='botheaj' node index 
```
You can also override the port 

```
NODE_ENV='Testing' MYNAME='botheaj' PORT=5001 NODE_ENV='Testing' node index 
```

This will run the app on Port `5001`.  

On Windows, you can’t run variable in-line, so you have to set them like we did above. 
```
$env:MYNAME='botheaj' 

$env:PORT='5001' 
```
 
Restart the process

```
node index 
```

Launch your web browser to http://localhost:5001 and confirm the app is working as expected. 

It should look something like this:  

 
 ![](/screenshots/21-11-03-07-15-58.png)

 
## Code based on ENV 

Knowing that we have the ability to check for our environment variable, we can make accommodations in our code. Right after we send our first response, we can add a bit more code to check for our ENV and change our output. Modify your app.get code to look something like this:  

```javascript
app.get('/env', function(request, response) { 

  if (config.util.getEnv("NODE_ENV") === "Testing") { 

    response.send('<b>You are working in the <em>TEST</em> environment.</b>') 

  } 

  else if (config.util.getEnv("NODE_ENV") === "Heroku Test") { 

    response.send('<b>You are working in the <em>TEST</em> environment that is in Heroku.</b>') 

  } 

  else if (config.util.getEnv("NODE_ENV") === "Production") { 

    response.send('<b>You are working in Production</b>') 

  } 

  else { 

    response.send('<b>Environment is unknown</b>') 

  } 

}) 

```

 

Restart the application and browse to it. Include `/env` at the end http://localhost:5001/env  


## Create a Heroku app 

To create an app in Heroku, login to the web interface at https://dashboard.heroku.com/apps  

From the dashboard, click New | Create New App.  
 

Call the app whatever you want, but it has to be distinct. Something like username-date is usually a safe bet.  

Click Create App. 

There is a section on the following page that shows you how to deploy.  

Let’s follow these steps. We’ve already logged in, so we can skip the first step. We’ll need to initialize our new repo 
```bash
git init 

heroku git:remote –a <NAME OF YOUR APP> 

git add . 

git commit –m 'my first heroku app' 

git push heroku master 
```
 

You will get an error! That’s ok! Let’s find out what it is.  
```
>heroku logs 
```
 

Oh, we forgot to tell our app about our dependency on the configs package. This is in our package.json. We can add it manually, or we can add it from CLI. Let’s run 

 
```
npm install config --save 
```
 

We also need to change the version of Node that this app runs as. Open the package.json file again and change  
```
"engines": { 

    "node": "4.0.0" 

  }, 
```
To 
```
"engines": { 

    "node": "8.0.0" 

  }, 
```
 

Then run…  
```bash
git add . 

git commit –m’heroku ready’ 

git push heroku master 
```
 

There does not appear to be any errors now. We can browse to our app at  

`https://<name_of_heroku_app>.herokuapp.com`

You can also click `Open App` from the Heroku window or type `heroku open` in your terminal.  

Our app works, as expected, but it’s not giving us the `Environment Variables` that we set on our local machines.  

We need to create these `Environment Variables` within Heroku in order to get them to work.  

In our Heroku app, go to `Settings`

 

Click `Reveal Config Vars` 

You’ll see an option for KEY VALUE pairs.  

 

Create a `NODE_ENV` value and set it to `HerokuTest`

Also create our `MYNAME` variable and set it to your username. It should look something like this.  

 
![](/screenshots/21-11-03-06-59-24.png)
 

Reopen the application and verify it looks similar to this.   

![](/screenshots/21-11-03-06-59-38.png)

 

Lab 10 

Using the code you wrote for Lab 9, create a new route at /api that includes the content of the file widgets.json we used last week. Submit the URL of your Heroku app that returns the contents of widgets.json at /api  

Your final page should look like this example, including your correct username, a Heroku environment other than “Development” and a working API link:  

https://botheaj-test-pre-class-492019.herokuapp.com/ 

Here are a few hints:  

Using the example for checking /env, you can create another route for /api. Don’t over think it.  

Refer to last week’s NodeJS code. You’ll need the file, the data and a response.  

Your widgets.json file needs to get sent to Heroku and you need to point to it without knowing the full path. A relative path should do (eg. ./public/widgets.json) 
