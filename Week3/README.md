# Module 3 - PowerShell 

## List of Links for this week: 

PowerShell Core Download - https://github.com/PowerShell/PowerShell 

Using Gmail to send SMTP email https://www.pdq.com/blog/powershell-send-mailmessage-gmail/ 


 

## PowerShell Core 

All of the PowerShell you thought you knew is changing. PowerShell, a script originally built on the .Net framework, has shifted to PowerShell Core, a free, cross-platform tool that will run the same on Windows, Mac, or Linux.  

To get the latest release of PowerShell Core, you have to pull it from github. You can view current versions, release notes and the code itself on GitHub. https://github.com/PowerShell/PowerShell/releases/  

Use the MSI download for Windows to install. (PowerShell-7.x.x-win-x64.msi)  

Take note that PowerShell is also available for other Oss.  

We now have TWO versions of PowerShell installed on our server. PowerShell (v5) and PowerShell Core (v6). Both are still accessible. From the start menu, the default PowerShell Icon will launch PowerShell (v5). Search for ‘pwsh’ to find PowerShell Core. Feel free to pin both of these to the task bar.  

 
![](/screenshots/21-09-03-15-53-20.png)
Figure 2 PowerShell Icons - Left is the delivered PowerShell (v5), right is PowerShell Core 

 

Let’s launch PowerShell Core (pwsh.exe)  

Type this command 
```powershell
PS C:\>   $HOST
```
The output shows you the version of PowerShell you’re using. 6.1.2 is PowerShell Core. 5.1.xxxx is classic PowerShell. 

 

$HOST is a variable built into PowerShell. PowerShell comes equipped with several of these built in variables and you can derive a lot of information from them. To see them all, run  

```powershell
PS C:\>   Get-Variable 
```

 

All of these variables can be called at any time and each one contains valuable information about your server. You can call a property of each of the listed values to get more information.  
```powershell
PS C:\>   $HOST.Version 
```
 

The output here shows 4 properties, Major, Minor, Build, and Revision. We can dig even deeper to pull one specific property 

 
```powershell
PS C:\>   $HOST.Version.Major 
```
 

This outputs the major version by itself.  

PowerShell is built on the idea of cmdlets, which are basically micro-scripts that you can call directly from PowerShell. You can get a list of cmdlets using  

 
```powershell
PS C:\>   get-command  
```
 

We can apply cmdlets to other cmdlets using the | (pipe) character.  

 
```powershell
PS C:\>   get-command | measure-object 
```
 

The output shows a bunch of blank values, except for count, which is the only one we care about. Count, in this manner shows the number of available commands (both functions and cmdlets) available to us by default. We can wrap this call with parenthesis and specify that we only want the COUNT output.  

 
```powershell
PS C:\>   (get-command | measure-object).Count 
```
 

NOTE: PowerShell is NOT case sensitive in most cases, but Linux is. Keep this in mind.  

Now, let’s compare classic PowerShell to PowerShell Core. Run the command against both version of the console. Note the count difference between the two. This is because some modules are NOT compatible with PowerShell.  

 

There are significantly fewer functions and cmdlets available in PowerShell Core by default. This is the major difference between the two right now and possibly a reason to stay away from PowerShell Core, depending on what you’re trying to do.  

 

A few useful PowerShell commands:  

 

PowerShell Commandlets 
```powershell
get-service  

get-service –name “*net*”  

Status   Name               DisplayName  
------   ----               -----------  

Stopped  aspnet_state       ASP.NET State Service  
Running  Netlogon           Netlogon  
Running  Netman             Network Connections  
Running  netprofm           Network List Service  
Stopped  NetTcpPortSharing  Net.Tcp Port Sharing Service  
```powershell
 
Pipe operator "|"   takes output from one command and passes it to the next. There is no limit to the number of pipe commands you can pass, but be careful.  

A command like 
```powershell
Get-service | start-service  
```
Will start EVERY Windows service. Mostly, you wouldn’t want to do that.  


### Get Help  

#### REMINDER: <tab> is your friend.  
```powershell
Get-H<tab> 
Get-Help  
```
 

First time you run Get-Help, you may be asked to install help files locally. Go ahead and do this.   
```powershell
Get-Help Get-Service  

Get-Help Get-Service -Examples  

Get-Help Get-Service –Details  

Get-Help Get-Server –Online  
 

### Cmdlets 

Show every available cmdlet, function, alias 
```powershell
get-command  
```
 

Filter by simply adding part of the name 
```powershell
get-command get-hotf* 
```
 

  

### Aliases 

 

These 3 commands are all the same: 
```powershell
ls  

dir  

get-childitem  
```
 
Why, because of aliases
```powershell
get-alias ls  

get-alias dir  
```
 

shows all help, including aliases  

 
```powershell
get-help get-childitem –full  
```
 

Gives more information about alias commands  
```powershell
get-help about_aliases  
```

Shows all existing aliases for a cmdlet  
```powershell
Get-Alias –Definition Get-ChildItem  
```
 

Set and alias and use it 
This sets “ll” alias for Get-ChildItem  
```powershell
set-alias ll Get-ChildItem  
ll
```

PowerShell has a bit of its own query language too…  

This command shows all stopped services, like you see through services.msc  

```powershell
Get-Service | Where-Object {$_.status –eq “stopped”}  
```

### Functions  
This list of functions. Functions are basically cmdlets, which add additional functionality for simple tasks. Functions typically take cmdlets and bring them to work together.   

```powershell
get-command | where-object {$_.commandtype –eq “Function”}  
```

### Testing Commands  

Whatif will show you what will happen  

```powershell
Get-Service Spooler | Stop-Service -Whatif  
```
 
Confirm will ask if you are sure  
```powershell
Get-Service S* | Stop-Service –confirm  
```

### ISE 

~~~
Note: PowerShell Core does not use ISE. It is compatible with VS Code, however, I think ISE is still a powerful tool to understand, so let’s go ahead and launch it from our Classic PowerShell console.  
~~~

 

In ISE, click the “Show Script Pane Top” 

![](/screenshots/21-09-03-16-01-51.png)
 

 

### Working with Output  

 

Display a list of services with these two parameters  
```powershell
Get-service | format-list displayname, status  
```
Display a list of services in a table with these two parameters  
 
```powershell
get-service | format-table displayname, status  
```

Display a list of services in a table with all parameters  
 
```powershell
get-service | format-table *  
```
Display a list sorted by a property  
```powershell
get-service | sort-object –property Status –descending | format-table displayname, status  
```
 
Sending output to a file  
```powershell
 get-service | sort-object –property Status –descending | format-table displayname, status | out-file C:\services.txt   
 notepad C:\services.txt  
 Remove-Item C:\services.txt  
``` 

Gridview  
```powershell
 get-service | out-gridview  
 get-service | select-object displayname, status | out-gridview  
 get-service | select-object * | out-gridview  
```
 
*Gridview can’t be combined with format-list or format-table  
 
### Variables 

Variables work the same in PowerShell as they do in most scripting languages. You define a variable with a $  
```powershell
$Hello = "Hello, PowerShell" 
Write-Host($Hello) 
```

![](/screenshots/21-09-03-16-03-48.png)
 
F5 or the green arrow will run the script.  

 

Figure 4 F5 or Green Arrow to run the script 

 

Let’s do something similar to what we did in Linux last week and get the IP address of our server.  

Jump down to the bottom pane, which is a PowerShell Window and run the following commands.  

 
```powershell
PS C:\>   Get-NetIPAddress 
```
 

This cmdlet gives us information about our system’s network. To get more information about this cmdlet, type  

 
```powershell
PS C:\>   get-help get-netipaddress 
```
 

Two things are your friend in ISE: Intellisense, which will autocomplete a lot of your code, and the commands pane on the right. Type get-netipaddress into the commands pane. This will show you all of the available parameters as well as allow you to call the command.  

 

So since this command gives us access to the IP, we need to filter the output down to a more usable format. 

 
```powershell
PS C:\>   get-netipaddress.ipaddress 
```
 

We get an error? Why?  

Well, let’s find out what type of command it is  

 
```powershell
get-command get-netipaddress 
```
 

It’s a function type, not a cmdlet. So we don’t have access to all of the properties like we would a normal cmdlet. But, we can access it using parenthesis to treat the output of the function as an object with properties that we can call.  

 
```powershell
PS C:\>   (get-netipaddress).ipaddress 
```
 

Ok this is better, but I don’t care about IPv6. I think I saw a command to specify ipv4, so let’s try that.  

 
```powershell
PS C:\>   (get-netipaddress).ipv4address 
```
  

Yes, that’s what I want… but… I don’t want the loopback address (127.0.0.1), so I need to filter that out.  

To do that, we can use a GREP like command called “Select-string” 

 

PS C:\>   (get-netipaddress).ipv4address | Select-String "192*" 

 

 

This gets us the output we want… but what if we could put this into a function so we don’t have to call this whole string every time.  

Let’s do that.  

In the Script Pane, copy and paste the code we wrote that gets us the output we want. Then, we simply wrap this code in a function call 

 
```powershell
function getIP{ 

    (get-netipaddress).ipv4address | Select-String "192*" 

} 
```
 

Now this function simply runs the code. We haven’t told it what to do with the function, so we have to also write the output of the function to the host. Add this line under the function 

 
```powershell
write-host(getIP) 
```
 

The output will be the IP address, as is was below. Now, taking this function, we can apply the output of it to a variable and use this variable in a string. There are two ways to add variables to a string in PowerShell:  

 
```powershell
$IP = getIP 

Write-Host(“This machine’s IP is $IP”) 

Write-Host(“This machine’s IP is {0}” -f $IP) 
```
 

 

Before we continue 

**Save** our file as sysinfo.ps1. Save it to C:\it3038c-scripts\powershell\sysinfo.ps1  

 

### Git on Windows 

Let’s sync our Git Repo to our Windows server too. In powershell, locate the IT3038C-Scripts directory you just created.  

```powershell
PS C:\>  cd C:\it3038c-scripts 

PS C:\>  git status

PS C:\>  git add . 

PS C:\>  git commit -m'powershell files'

PS C:\>  git pull


PS C:\>  git push

```
 

Return to your LINUX machine and pull the latest changes.  

`$ git pull origin main`

 

Now your systems should be in sync. Any time you make changes you will have to push your changes to github for them to be seen. Then, if you switch to another machine, you will need to pull the latest changes.  

 

# LAB 3 

Complete the script below with the following information and use Send-MailMessage to mail the information to yourself (and me for credit).  **Alternatively**, you can output the contents of the script to a file and attach that file to the assignment. 


PowerShell REQUIRES an SMTP server to send email. 
Office365 will sometimes let you send email, in which case you can use smtp.office365.com. In the case that doesn’t work, you need to find another SMTP service to use. I recommend smtp.gmail.com or another SMTP host provided by your email provider.  


### Using Gmail SMTP Server 

Using Gmail SMTP server is easy if you do NOT use two-factor authentication. If you do, you have to create an App Password to use. App Passwords bypass 2FA and allow certain services access to your Gmail account.  

Go to https://myaccount.google.com/apppasswords and login with your Google account.  

Select an app (Mail) and a device (Other) and type “PowerShell SMTP” and click GENERATE 
 

The displayed password can be used to authenticate your email messages when sent. Using Get-Credential will ask PowerShell to prompt you for login. This is the time you will put in your Google username and app password.  

 

If you do NOT use 2FA on your Google account:  

Browse to https://myaccount.google.com/security  

Select Turn on access under Less secure app access 

Graphical user interface, text, application, email

Description automatically generated 

### **WARNING: DO NOT LEAVE THIS ENABLED. Once you finish the lab, please disable this.**


Here’s an example of Send-MailMessage 

```powershell
Send-MailMessage -To "me@mail.uc.edu" -From "me@mail.uc.edu" -Subject "IT3038C Windows SysInfo" -Body $BODY -SmtpServer smtp.google.com -port 587 -UseSSL -Credential (Get-Credential) 
```


The output of the email should look similar to this:  

`This machine's IP is 192.168.33.53. User is Administrator. Hostname is botheaj-win. PowerShell Version 5. Today's Date is Tuesday, <Month> <Day> <Year>`



For **Lab 3**, submit your github url containing your PowerShell script to Canvas. 
The PowerShell script, when run, should output the $Body variable to the console and either send an email or output to a file. 
