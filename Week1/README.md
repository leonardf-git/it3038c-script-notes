# Lab 1 – Getting Started with your VMs
## Introduction
This lab will get you familiar with the VM Sandbox environment, creating a CentOS VM and a Window VM. 


 
Firefox is recommended for all labs. Please note, while the VM Sandbox environment is very robust, it is not perfect. Do not expect high level of performance and anticipate that there might be some latency issues, especially if you are working off-campus.    

Setting-up Virtual Machines for this Class

First, login to https://sandbox02.cech.uc.edu/vcac/ 

Select the domain “ad.uc.edu” and click Next. 

![](/screenshots/21-08-28-11-33-18.png)

Sign in with your UC username and password.

![](/screenshots/21-08-28-11-33-32.png)

Click the “Deployments” tab, which will contain all of your requests. Select your “Scripting Language” Deployment 

![](/screenshots/21-08-28-11-33-44.png)

## Linux Setup and Validation 

Let’s select the CentOS server first. Hover over the machine and click the blue gear icon. Click “Connect Using Remote Console”
 
![](/screenshots/21-08-28-11-34-15.png)

Click into the window and press <ENTER> to show the login prompt. 
Login using the following credentials:
Localhost login: student
Password: Pa$$w0rd 

 


Click Applications | Terminal

![](/screenshots/21-08-28-11-34-39.png)

 
Go ahead and run this command: 
```bash
sudo su
```
Enter the password above once again. 
Now run some of these commands:

```bash
ping www.uc.edu
```
A proper ping response confirms we have an internet connection. Press ctrl-c to end the ping. 

Run the command 
```bash
ip a
```
You should see results similar to what’s below. 

![](/screenshots/21-08-28-11-38-59.png)


While we’re at it, let’s go ahead and change the hostname of your Linux VM. Call is the same name you called your VM when we provisioned it (eg. botheaj-centos)

To change it, 
```bash
hostnamectl set-hostname botheaj-centos
```

Follow this up with a reboot to finalize both our network and hostname settings. 
```bash
reboot
```

That’ll do it for our CentOS setup for now. Let’s switch to Windows. 


## Windows Setup 
Click the Blue Gear icon next to our Windows machine. Again, click “Connect using Remote Console”
 
 ![](/screenshots/21-08-28-11-39-07.png)

At the login prompt, login as 

`User: Administrator`

`Password: Pa$$w0rd `

Click the Start Menu and type PowerShell to launch the PowerShell window.  

![](/screenshots/21-08-28-11-39-15.png)

First, confirm we have an IP address. 
```powershell
ipconfig
```

 ![](/screenshots/21-08-28-11-39-23.png)

Also confirm we can ping a website. 
```powershell
ping www.uc.edu
 ```

![](/screenshots/21-08-28-11-40-00.png)

And why don’t we confirm that we can ping our linux machine. You can get the IP address from the ip addr command that we ran above (hint: it starts with 192.)
```powershell
ping 192.168.33.4    #this will be different for you. 
``` 

![](/screenshots/21-08-28-11-40-13.png)

Finally, let’s change our Windows hostname as well:
```powershell
rename-computer botheaj-win    ###replace botheaj with your username
```

Now, restart your Windows machine 
```powershell
shutdown -r
```

Once rebooted, please take a snapshot of each one of your VMs. From the blue gear menu, click “Create Snapshot”. Give it a name if you want, and click Submit. Do this on both your Windows and Linux VMs. 

![](/screenshots/21-08-28-11-40-39.png)


## Windows Software
Let’s install all of the software we’re going to need for our class. 

Git
Download and install Git from https://git-scm.com/download/win 

![Download and install Git from](/screenshots/21-08-28-11-40-49.png)

Download and install VSCode from https://code.visualstudio.com/download

![Download and install VSCode](/screenshots/21-08-28-11-41-11.png)

Download and install putty from https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

![](/screenshots/21-08-28-11-41-25.png)


After installing, launch Putty. 

Remember your Linux IP address? Enter that into your Host Name value in Putty and click Open. 

![](/screenshots/21-08-28-11-41-34.png)
 
Accept the Servers key by clicking Yes. 

 ![](/screenshots/21-08-28-11-41-39.png)

Login as the cechuser with the password above. 

 ![](/screenshots/21-08-28-11-41-46.png)

And there you have it. You are now logged in to your Linux machine, no need to use the other console. 

## Github Setup

Now, let’s setup our Github account. 

Open https://github.com
Create an account using YOUR UC email address and call it something like uc-username, for example, I will register with botheaj@mail.uc.edu and username uc-botheaj. 

 ![](/screenshots/21-08-28-11-42-01.png)

If you’ve never used Github, this is a great opportunity to get familiar with how it works. You can create the initial README.md file to say a little about yourself. Just click the green “Continue” button and start editing. 

 ![](/screenshots/21-08-28-11-42-08.png)

If you already have a Github.com account, you can create a new Repository with the same name as your username, then create the README.md file within to update your Github homepage. 

We’re going to create our first repository and do our first Github push. From github.com, click the “+” sign in the top-right corner of the screen and click “New repository”

 ![](/screenshots/21-08-28-11-42-19.png)


Name the repository “it3038c-scripts” and leave defaults for everything else.

 ![](/screenshots/21-08-28-11-42-27.png)

Click “Create repository”

Return to your Windows server. 
We’re going connect our systems to Github. 

Open PowerShell as administrator so we can setup our directory for use with Github. Follow these instructions and DO NOT COPY AND PASTE. You’ll need to change certain fields, like your username and email: 

Change dir to the root of C:, then create an it3038c-scripts folder and subfolders with files in them. The commands are:

```bash
cd \
mkdir it3038c-scripts 
cd it3038c-scripts 
mkdir powershell 
mkdir bash 
new-item -type file powershell\keep.git 
new-item -type file bash\keep.git 
```
Initialize the git repo and connect it to github, then sync. The commands are:

```bash
git init 
git remote add origin https://github.com/<your-username>/it3038c-scripts.git 
git config --global user.email "<your-github-email>"
git config --global user.name "<your-github-username>" 
git add .
git commit -m'first commit'
git checkout -b main
git branch -d master
git push origin main
```
`
Login with your github.com credentials. 
If you followed all of these steps correctly, you will see your two folders that you just created in your repo on github.com. 

# Completing LAB 1 
Return to Canvas: 

Under Assignments within Module 1, select Lab 1. Enter a text submission with a link to your Github account in this format: 
Link to Github.com: https://github.com/<your username>

Your Github.com page should contain the it3038c-scripts repository which contains the PowerShell and Bash folders. 

