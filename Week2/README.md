# Lab 2: Bash Crash Course

If you do not have your Github account setup, please do that now. 

## Putty
First, we'll connect to our Linux instance via Putty. If you are NOT using the Sandbox environment, then you just need to connect via SSH to your Linux machine. 

![](/screenshots/21-08-28-13-03-46.png)

## Create a User 
Quick aside, I hate putting in a password every time I su, so let’s create a user that doesn’t have to do that. To do this, we have to sudo su to root first. 
Run the following commands from your Linux PuTTY connection 

```bash
sudo su
sudo useradd username                     #### THIS should be YOUR UC username ####
sudo echo 'username ALL=(ALL:ALL) NOPASSWD:ALL' > /etc/sudoers.d/username
su – username
```

Now, let’s do one more step to make our lives super easy. As the user you just created. We’re going to ‘key’ our user so that we can login without a password from our Windows machine. 

On your Windows VM, right-click the PuTTY icon and click Run PuTTYgen

![](/screenshots/21-08-28-13-05-25.png)

Click Generate and move the mouse until the bar is full. This will generate a unique key. 

![](/screenshots/21-08-28-13-05-35.png)

Once finished, select ALL of the text in the window under Public key for pasting into OpenSSH…. 

![](/screenshots/21-08-28-13-05-52.png)

Click “Save public key” and “Save private key”. Click “Yes” that you are sure you want to save without a passphrase. 
Save them to a location like C:\sshkeys\
Give the file name your username, botheaj.pub and botheaj.ppk respectively. 

Return to your Linux session. Run the command as your user, making sure all spelling matches:

```bash
##you may need to create the .ssh directory. To do this, run 
$ mkdir ~/.ssh/

$ vim ~/.ssh/authorized_keys
```

Press the 'i' key within VIM to enter *INSERT* mode, then paste the contents of the ssh-rsa key from your puttygen window. It should start with **ssh-rsa** and end with **rsa-key-YYYYMMDD**


Save and quit the file by pressing `<esc>` and entering `:wq` (write and quit) 

Now it’s not enough to just have the file, we also need to set permissions on it. Change the perms to 700 for .ssh folder and 600 for the authorized_keys file, that is, read and write for ONLY the owner of the file:

```bash
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/authorized_keys
```

Return to the base PuTTY window. For hostname, enter `username@192.168.33.XX` where username is your user and XX is the last octet of your IP. 

On the left side, under Connections, click SSH | Auth (select Auth, do not click +)
Click Browse and add your SSH Private Key

![](/screenshots/21-08-28-13-09-13.png)

Return to the Session tab and save the session by entering a session name and clicking Save

![](/screenshots/21-08-28-13-09-23.png)

Now, click Open to open the session. You should be connected as a user, with root access, no password needed. Make sure you can `sudo su` as well. 

# Git for Linux
Let's install Git on our Linux machine

```bash
sudo yum -y install git 
```

This command calls the package manager for our system, finds all of the dependencies for Git and installs them. 

All we need to do it run 'git –version' to verify it’s working.
`git --version`


Now that we've already established our Github repo, we just need to pull it down, but first, we have to let our Github repo know who we are, so that we can freely push and pull. 

To do this, we'll need to generate an SSH key on our Linux machine that we can use to authenticate to Github. 

```
ssh-keygen -t ed25519 -C "<your-github-email>"
```
You'll be prompted for a save location. Defaults are fine, and we don't need a password, so keep hitting enter until you are out of the command. 

Now we need to copy our "public" key from our linux server to https://github.com/settings/ssh/new 

Run the following command:
```bash
cat ~/.ssh/id_ed25519.pub 
```

Give it a friendly name (sandbox) then paste the contents of the file above and Save. 

Return to your repo in Github. Click the code button and copy the "SSH" clone location:

![](/screenshots/21-08-28-13-27-44.png)

Make sure it starts with git@github.com 

Back to your putty window, run the command 
```bash
cd ~
git clone git@github.com:<yourusername>/it3038c-scripts.git 
```

You may be promoted to validate the cert. Type `yes`

Now 
```bash
cd it3038c-scripts
```

You now have your Git repo synced to your Linux machine. Let's go ahead and complete our setup by running these two commands: 

```bash
git config --global user.name "<your-username>"
git config --global user.email "<your github email>"
```



# Bash Scripting
Bash = Bourne Again SHell. Bourne shell is the original sh shell that we see on many systems. Bash became the default on Unix V7. Let’s run a few basic commands. 

---
**NOTE**

The code examples below includes a "$" only when differentiating user input and expected output in the same code block, otherwise they are omitted. 

---


Get Bash Version using the machine variable $BASH_VERSION
```
$ echo $BASH_VERSION

4.2.46(2)-release
```



Get our current working directory (print working directory)
```bash
$ pwd
/home/<username>
```

If you’re not in your home directory, 
```bash
cd ~
```

Let’s do some basic file manipulation. First, let’s create a directory called playground and cd into that directory. 

```bash
$ mkdir playground
$ cd playground
$ pwd
/home/username/playground
```

You can create an empty file using the touch command
```bash
touch 1.txt
```

You can create several files by passing in a parameter multiple times
```bash
touch 1.txt 2.txt 3.txt
```

You can create several files using “Brace Expansion”
```bash
touch file_{1..100}.txt
```

Show all the files 
```bash
ls
```

Now remove them
```bash
rm *
ls
```

Create several files with several brace expansions
```bash
touch hello_{1..5}.txt world_{1..5}.txt
ls
```

Get a line count from an input (ll in this case)
```bash
$ ls | wc –l
10
```

Clear the terminal window
```
clear
```

Count from x to y by z’s {x..y..z}
```bash
$ echo {0..10..2}
2 4 6 8 10
```

Print letters in a range
```bash
$ echo {A..Z}
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

The range can include ASCII characters
```bash
$ echo {A..z}
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z [  ] ^ _ ` a b c d e f g h i j k l m n o p q r s t u v w x y z
```

## Grep

Using grep filters out results, plus all kinds of new stuff. 
```bash
man grep
<q to quit>
```

Use grep to filter through the ls command
```bash
ls | grep 5
ls | grep hello
```
Filtering results with grep:

Run a ‘cat’ command on /var/log/secure to get a list of recent authentications
```bash
sudo cat /var/log/secure
```

Now apply a GREP filter to it. Let’s find all instances of student
```bash
sudo cat /var/log/secure | grep student
```

That’s quite a bit of data. Let’s find specifically any time root was impersonated
```
sudo cat /var/log/secure | grep 'session opened for user root'
```
This is much more readable…

## AWK

You can use AWK to filter results as well. Take our `ll` command as example. 
If you count the columns, you’ll see there are 9 total. We can use AWK to only print certain columns

```bash
$ ll | awk '{print $9}'
```

Combining Grep and AWK commands, we can get specific data from a command like our cat command above. Let’s see if we can extract the date and time and username only from our secure log file. 

```bash
sudo cat /var/log/secure | grep student | awk '{print $1 $2 $3 $13}'
```

Not bad. If we want to make this a little prettier, simply added empty quotations between each variable 
```bash
sudo cat /var/log/secure | grep student | awk '{print $1 " " $2 " " $3 " " $13}'
```

Let’s apply this filter to another command: `ip a`
```bash
ip a
```

We want to grab the primary IP address of this machine, so we have to find out what makes a single line distinct for this. We can see that ens192 is the name of our connection, so let’s GREP on that
```bash
$ ip a | grep 'ens192'
```

Better, but we’re still grabbing an extra line. Let’s add one more qualifier to our grep command to get a single line. Looks like if we grab noprefixroute ens192” we’ll get our single line output. 
```bash
ip a | grep 'noprefixroute ens192'
```

Now we can use AWK to grab the IP alone, without the rest of the line. 
```bash
ip a | grep 'noprefixroute ens192' | awk '{print $2}'
```

Excellent, we just grabbed our IP (with subnet mask) using simple BASH commands. 

### Other Commands
Here are some other commands we’ll be using in our first script
Date lets you manipulate date data
```
$date
Fri Jan 29 11:11:11 EDT 2021
$date +”%d-%m-%Y”
29-01-2021
$date +”%H:%M:%S”
12:34:56
```


We can grab our username using the Machine Variable $USER
```bash
$ echo "Name: " $USER
botheaj
```

This is ok, but we can do better. 
printf let’s us create simple, formatted strings that you can pass variables into our strings. \t is a tab, %s denotes a string for our variable that we pass as $USER, \n is a new line.

Let’s see if we can make the above statement prettier with printf
```bash
$ printf "Name: %s" $USER
name: student[student...
$ printf "Name: %s\n" $USER
name: student
$ printf "Name: \t%s\n" $USER
name:   student
```

# Creating a Bash Script

As your newly created user, run these commands to create a scripts that contains variables, arguments, and flow-logic. First, let's cd into our bash directory in our git repo and create a file

```bash
cd ~/it3038c-scripts/bash
vim myfirstscript.sh
```

Press the `i` key in Vim will allow you to insert. 
Start file with “shabang"

```bash
#!/bin/bash

# This script outputs the IP address and Hostname of a machine
echo ‘This is a script. Hello!’ 
```

Press ESC to exit `INSERT` mode to Save and exit vim
`:wq`    or    `:x`

Try to run the script: 

```bash
./myfirstscript.sh
```

You’ll get a permissions error. That’s because we don’t have execute on the script. Grant execute permissions with the following command: 
```bash
$ chmod +x myfirstscript.sh
$ ./myfirstscript.sh

This is a script. Hello!
```

There are a few options with Echo. Use vim to continue editing the file: 

$ vim myfirstscript.sh

```bash
greeting="This is a script. Hello!"
echo $greeting, thanks for joining us!
echo '$greeting, thanks for joining us! You owe me $20'
echo "$greeting, thanks for joining us!"
echo "$greeting, you owe me $20"
```

Save and exit vim with `<esc>` `:wq`

`./myfirstscript.sh`

output:
```sh
This is a script. Hello!, thanks for joining us!
$greeting, thanks for joining us! You owe me $20
This is a script. Hello!, thanks for joining us!
This is a script. Hello!, you owe me 0
```

Notice how each command has a slightly different output based on the use of single or double quotes. 

Reopen the script with vim and in the last line, change `$` to `\$`

`echo "$greeting, you owe me \$20"`

Save and exit vim with `<esc>` `:wq`

Run the script and notice how the use of escape characters has fixed the last line.
```bash
$ ./myfirstscript.sh
This is a script. Hello!, thanks for joining us!
$greeting, thanks for joining us! You owe me $20
This is a script. Hello!, thanks for joining us!
This is a script. Hello!, you owe me $20
```

### Variables
Open your script for editing and add the following lines:

```bash
echo Machine Type: $MACHTYPE
echo Hostname: $HOSTNAME
echo Working Dir: $PWD
echo Session length: $SECONDS
echo Home Dir: $HOME
```
These are know as Machine variables and they are available on virtually all flavors of Linux. 

Save and close your script and run it. You should see an output of all these reserved variables. 
```bash
Machine Type: x86_64-redhat-linux-gnu
Hostname: username-centos
Working Dir: /home/botheaj
Session length: 0
Home Dir: /home/botheaj
```

Reopen your script and add the following lines:
```bash
a=$(ip a | grep 'noprefixroute ens192'| awk ‘{print $2}’)
echo My IP is $a
```

Save and run the script again. You should see an output that includes your IP address. 

# A Real World Example
The takeaway with bash scripting is you can take any set of Linux commands and turn them into a bash script. 

Let's write a simple script that pulls from data from the internet. For this we'll use the `curl` command plus another tool called `jq`. Both of these should already be installed

Here is an example of the command we'll be turning into a script: 
```
curl https://api.covidtracking.com/v1/us/current.json | jq '.[0]'
```

The output will return in json format. The `jq` command parses the output and allows us to get just the data we want. Using some of the techniques we've already learned, we can create a simple script that gives us the `date` and the number of `positive` cases. 

`vim covid.sh`
```bash
#!/bin/bash
# This script downloads covid data and displays it

POSITIVE=$(curl https://api.covidtracking.com/v1/us/current.json | jq '.[0].positive')
TODAY=$(date)

echo "On $TODAY, there were $POSITIVE positive COVID cases"

```
Save your script. Remember to add executable rights `chmod +x covid.sh`
Then run it with  
`./covid.sh`

The output should be `On Sat Aug 28 14:23:53 EDT 2021, there were 28756489 positive cases`


## Avoid being rate-limited
API's are typically rate limited based on the number of calls per second. If you call this API multiple times in your script, you are likely to be rate limited. With one small tweak, we can do 1 call per script invocation. 

```bash
#!/bin/bash
# This script downloads covid data and displays it

DATA=$(curl https://api.covidtracking.com/v1/us/current.json)
POSITIVE=$(echo $DATA | jq '.[0].positive')
TODAY=$(date)

echo "On $TODAY, there were $POSITIVE positive COVID cases"
```

# Lab 2
Using this template as a starting point, add more data to this script. Please include at least 3 additional datapoints, displayed in a readable output. 

### Example
`On Sat Aug 28 14:23:53 EDT 2021, there were 28756489 positive cases, 74582825 negative tests, 500500 deaths and 776361 hospitalized.`


Finally, add your script to Github and push. 

```
git add .
git commit -m'Lab 2'
git push origin main
```
