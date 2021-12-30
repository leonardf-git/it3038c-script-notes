List of Links for this week: 

https://www.lynda.com/Linux-tutorials/Using-awk-advanced-text-processing/604236/642385-4.html 

https://www.lynda.com/Linux-tutorials/Using-sed-AWK-more-powerful-scripting/504429/536324-4.html 

https://www.lynda.com/Linux-tutorials/Welcome/162719/173835-4.html?srchtrk=index%3a1%0alinktypeid%3a2%0aq%3aawk%0apage%3a1%0as%3arelevance%0asa%3atrue%0aproducttypeid%3a2 

http://www.grymoire.com/Unix/Awk.html 

 

 

AWK 

Awk is used for advanced text processing. It is its own language in a sense, but it ties closely in with bash scripting. It is incredibly complex and powerful, but there is a bit of a learning curve.  

 

You will need to login to your LINUX VM to complete these exercises.  

 

Sample command: 

First, let’s create a simple file called /tmp/awktest.txt 

 

vim /tmp/awktest.txt 

 

In this file, add a few lines with some text. You can put in anything you want, but I’ll just use a few simple lines of text: 

 

Hello from AWK! 

This is not amazing yet! 

Here is a number: 7 

 

Save and close this file. Now, we can use awk to perform some tests against this file: 

 

FORMAT for AWK REQUEST: 

awk ‘BEGIN{some starting code} { a pattern or command} END{ some ending code}’ /path/to/working/file 

 

AWK code is always wrapped in single or double-quotes and always has curly braces around the command being run 

Example: 

awk 'BEGIN{print "start of file"} {print} END{print "end of file"}' /tmp/awktest.txt 

 

Here’s how it works:  

Awk has a BEGIN and an END, which are optional for some commands. The center command will take whatever command, in this case “print”, and execute it for each line passed to the awk command.  

We can get AWK input a few different ways.  

 

cat /tmp/awktest.txt 

cat /tmp/awktest.txt | awk 'BEGIN{print "start of file"} {print} END{print "end of file"}' 

 

We can apply AWK to any kind of inputs, not just a text file.  

ifconfig | awk ‘{print $2}’ 

The results of the command is the same with or without awk, but you get the idea. We can apply additional bash commands like GREP to filter this, like we did in a previous lab 

ifconfig | awk '{print $2}' | grep 10 

 

AWK can assign variables and print those as well.  

echo | awk '{var1="v1"; var2="v2"; print var1,var2}' 

 

AWK has several variables delivered out of the box.  

NR: Current line number 

NF: Current number of fields for that line 

$0: variable for the current line 

$1: variable for the first field. Can be $1, $2…$n for the number of fields (NF) 

 

Let’s use these to see what we get. We’ll use our test file for input.  

awk '{print NR NF}' /tmp/awktest.txt 

 

The output shows a few 2 digit numbers, but it’s actually showing the first column is the row number and the second column the number of fields.  

Let’s concatenate the output to make it more readable.  

awk '{print "Rows: "NR" Fields: "NF}' /tmp/awktest.txt 

 

On its own, this may not feel particularly useful, but you can start to see there are implications for finding the number or rows and fields in a line.  

We can also pass the number of the field to display.  

awk '{print $1}' /tmp/awktest.txt 

 

This displays the first ‘field’ of each row, delimited by a space.  

 

Using this information on how the basics of AWK works, we can apply it to finding some real, readable information about our system.  

Here are some examples:  

 

ps –ef  -  gives us a list of running processes 

 

ps -ef | awk '{print $1 $2 $8}’ 

 

This give us the User, Pid and Name of the process. Let’s apply some formatting to this to make it more reasonable.  

ps -ef | awk '{print "User: " $1 " Pid: " $2 " Process: "$8}' 

 

Using PIPE (|) we can get all kinds of valuable information about our system.  

We can use this to find data, like displaying data from the /etc/passwd file. You can change the delimiter with -F 

cat /etc/passwd | awk -F: '{print $1,$7}' 

 

This shows us which users have a default shell.  

Some other helpful awk commands: 

Get dis-space percentage used 

 

df -H | grep -vE '^Filesystem|tmpfs|cdrom' | awk '{ print $5 " " $1 }' 

 

Get IP addresses with awk, grep and regex (more on that in later weeks) 

ifconfig | awk '{print $2}' | grep [0-9]\.[0-9]\.[0-9]\.[0-9] 

 

List number and type of active network connections 

netstat -ant | awk ‘{print $NF}’ | grep -v ‘[a-z]’ | sort | uniq -c 

 

 

 

OPTIONAL LAB  

Review some of the AWK commands at https://blog.urfix.com/25-awk-commands-tricks/ 

Review the tutorial to generate HTML at http://www.thegeekstuff.com/2010/01/awk-tutorial-understand-awk-variables-with-3-practical-examples 

Select two commands from the first link and implement them into a BASH script that generates an HTML file on your Linux server. Push the script to github.uc.edu.  

 

 
