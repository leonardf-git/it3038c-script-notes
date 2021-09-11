List of Links for this Week 

Git Branching: https://learngitbranching.js.org/ 

Git Markdown Cheatsheet: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet  

 
![](https://imgs.xkcd.com/comics/git.png)

# Github CLI

There's a relatively new tool called "Github CLI". It's a command-line program you install to manage your Github repos. 

Let's try it out. 

Go to https://cli.github.com/ and click Download for Windows. 

Run the installation, accepting defaults is fine. 

Now, restart PowerShell and set it up. 

Run 
```powershell
gh auth login

? What account do you want to log into? GitHub.com
? What is your preferred protocol for Git operations? HTTPS
? Authenticate Git with your GitHub credentials? Yes
? How would you like to authenticate GitHub CLI? Login with a web browser

! First copy your one-time code: 8DC2-5326
- Press Enter to open github.com in your browser...
✓ Authentication complete. Press Enter to continue...
gh repo clone uc-botheaj/it3038c-script-notesgh repo clone uc-botheaj/it3038c-script-notes
- gh config set -h github.com git_protocol https
✓ Configured git protocol
✓ Logged in as uc-botheaj

```
Now, instead of using the git command, you can use the gh command. 

```powershell
gh repo clone uc-botheaj/it3038c-script-notes
```

I'm not sold on the advantages of gh cli yet, but there you go. 

# In-Depth with Git 

The following lesson can be done on the OS of your choice. 

**NOTE** 
I use the "touch" command alot which is a command that creates an empty file. This is NOT a valid PowerShell command. But we can make it one by using the "set-alias" command
```powershell
set-alias touch new-item
```


At this point you should have a git repo with some files in it. You should at least have a bash and powershell folder, with a script or two in each folder. You've run a few git commands, but let's dig and and really try to understand what's going on here. 
 

Let’s review some common Git commands.  

```bash
$ git init            					//initialize local git repository 

$ git add <file>  					//Add files(s) to index.  “.”  will add all files 

$ git status						// Check Status of Working Tree 

$ git commit						//Commit changes to “index” 

$ git push						// Push to Remote Repository 

$ git pull						// Pull from Remote Repository 

$ git clone						// Clone repo into a new directory 

$ git remote 						//manage remote repository 
```

 



There are 4 primary git commands. Think of git as if you’re going shopping.  

`git add` puts items in your shopping cart.  

`git commit` paying for them at the checkout counter and putting them in your car

`git push` is taking the groceries home 

`git pull` would be like if your roommate also brought groceries home. Now you both have all the groceries.  
 
In our it3038c-scripts directory, create a folder called “testing” and within, create 2 files (app.js, index.html).  

From your it3038c-scripts directory 

 
```bash
mkdir testing 

touch testing/app.js testing/index.html 

git status 
```
 

Notice we now have “Untracked” files, meaning that they exist in our git repo, but git is not tracking or handling these files. Let’s add some content to our index.html file. Open it with vim or with your Windows text editor and add the following: 

 
```html
<html> 
  <head> 
    <title> My App </title> 
  </head> 
  <body> 
        This is my app 
  </body> 
</html> 
```
 

 

Save and close the file. Now, let’s add just this html file to our repository 

 
```bash
git add testing/index.html 

git status 

# On branch main 
# 
# Initial commit 
# 
# Changes to be committed: 
#   (use "git rm --cached <file>..." to unstage) 
# 
#	new file:   index.html 
# 
# Untracked files: 
#   (use "git add <file>..." to include in what will be committed) 
# 
#	app.js  
```
 

Notice that we have 1 file to be committed (index.html), and one file that is untracked (app.js). To remove a file from being tracked, run `git rm --cached`. Let’s do that now.  

```bash 
git rm –-cached testing/index.html 

git status 
```
 

Now both files are untracked again. Let’s create a second index.html file called index2.html and let’s use a wildcard ‘*’ to add both html files back to “shopping cart” 

 
```bash
cp testing/index.html testing/index2.html 

git add *.html 

git status 
```
 

Now you should have 2 .html files, both being tracked. Let’s try deleting index2.html 

 
```bash
rm index2.html 

ls 

git status 
 
# On branch main 
# 
# Initial commit 
# 
# Changes to be committed: 
#   (use "git rm --cached <file>..." to unstage) 
# 
#	new file:   index.html 
#	new file:   index2.html 
# 
# Changes not staged for commit: 
#   (use "git add/rm <file>..." to update what will be committed) 
#   (use "git checkout -- <file>..." to discard changes in working directory) 
# 
#	deleted:    index2.html 
# 
# Untracked files: 
#   (use "git add <file>..." to include in what will be committed) 
# 
#	app.js 
``` 

Notice our file index2.html is still showing up as “deleted”, so our git repo still knows it’s a thing, even though we deleted it. To remove it from git, we have to run  

 
```bash
git rm --cached testing/index2.html 
```
 

Now it is removed, but we can do it in one command rather than two. Create the index2.html file again.  

 
```bash
cp testing/index.html testing/index2.html 

git add *.html 

git status 
```
 

Then delete the file AND remove it from git 

 
```bash
git rm -rf index2.html 
```
 

Now the file is gone and also no longer being tracked by git 

 

Let’s now add the rest of the files to the repo. Do this with  

 
```bash
git add . 

git status 
```
 

This shows we have both our index.html and app.js files tracked by git. Let’s make a change to index.html 

 
```html
<html> 
  <head> 
    <title> My App </title> 
  </head> 
  <body> 
        This is my app!!!! 
  </body> 
</html> 
```
 

I added a few exclamation points to my file. Now save and  

 
```bash
git status 
 

 

# On branch main 
#
# Initial commit 
# 
# Changes to be committed: 
#   (use "git rm --cached <file>..." to unstage) 
# 
#	new file:   app.js 
#	new file:   index.html 
# 
# Changes not staged for commit: 
#   (use "git add <file>..." to update what will be committed) 
#   (use "git checkout -- <file>..." to discard changes in working directory) 
# 
#	modified:   index.html 
 

Git is telling us that we have added new files, but also that our index.html file has been modified, so we need to add to our shopping cart to be ‘staged’ 

 
```bash
git add . 
```
 

Now let’s commit. This is the “checkout” instance of our shopping analogy 

 
```bash
git commit -m'Initial commit' 

# [main(root-commit) a32bd90] Initial commit 

# 2 files changed, 8 insertions(+) 

# create mode 100644 app.js 

# create mode 100644 index.html 

git status 

# On branch main 

# nothing to commit, working directory clean  
```
 

Let’s edit the app.js file now and add the following: 

```javascript
Console.log(‘Hello’); 
```
 

Save and close this file. Now, use the repeatable process of git.  

 
```bash
git add . 
git commit -m'Added output to app.js' 
git status 
```


Notice how my comment gives a little hint to what I actually did.  

Let’s create a .gitignore file. If you’re working in Windows, you’ll have to add this file via command line, because Windows Explorer won’t let you create a file with ‘.’ In front 

 
```bash
touch .gitignore 
```
 

Let’s create a file to ignore. Something like output.log. First create the file.  
```bash
$ touch output.log 

$ git status 

 

# On branch main 

# Untracked files: 

#   (use "git add <file>..." to include in what will be committed) 

# 

#	output.log 

# nothing added to commit but untracked files present (use "git add" to track) 

```

Notice the file shows up, but when we edit our gitignore file and add it 
```bash
vim .gitignore 
####if using Powershell you can edit this file with notepad. Feel free to set another alias `set-alias vim notepad`
notepad .gitignore
```
Add the following contents to the .gitignore file. 
```
output.log 
```
 
Then run: 
```bash
$ git status 

# On branch main 

# Untracked files: 

#   (use "git add <file>..." to include in what will be committed) 

# 

#	.gitignore 

# nothing added to commit but untracked files present (use "git add" to track) 
```
 

Our only file is the .gitignore file we just created. Now add it and commit it 
```bash
git add . 
git commit -m'added .gitignore file' 
```
 

You can also add entire directories the exact same way. Options include 
```
/directoryname 

*.txt 

.pycache 
```
 

 
 Let’s talk about branches. A branch is the functionality you would use if working on a team or working by yourself on code that you want to complete before it is merged into the Production code. This allows others to make changes without impacting you, and vice versa. To create a branch, type: 

 
```bash
git branch dev 
```
 

Git branch creates the branch, but doesn’t switch to it. To checkout this branch, do  
```bash
git checkout dev 

git branch 
```
 

You can see the * indicates we are in the ‘dev’ branch. Now create a file called new.js. Edit it and add some text to it.   
```bash
vim new.js 
```

Add the contents:
```
This is a test 
```
 

Also edit index.html and make some minor changes.  
```html
<html> 
  <head> 
    <title> My App </title> 
  </head> 
  <body> 
        This is my app!!!! Hi Dev! 
  </body> 
</html> 
```
 

Now add and commit these changes.  
```bash
git add . 
git commit -m'adding new stuff' 
```
 

Now let’s switch back to the mainbranch.  

 
```bash
git checkout main 
```
 

Notice that all of our changes are gone. That’s because they only exist in the dev branch, but not main. You can freely switch between the two and see the files disappear and reappear.  

 
```bash
git checkout dev 
```
We can also use the git diff command to see what’s changed between the two branches.   

```bash
$ git diff dev main 


diff --git a/index.html b/index.html 

index f450e0b..1925422 100644 
--- a/index.html 
+++ b/index.html 
@@ -3,6 +3,6 @@ 
     <title> My App </title> 
   </head> 
   <body> 
-       This is my app!!!  Hi Dev! 
+       This is my app!!! 
   </body> 
 </html> 
diff --git a/new.js b/new.js 
deleted file mode 100644 
index 90bfcb5..0000000 
--- a/new.js 
+++ /dev/null 
@@ -1 +0,0 @@ 
-this is a test 
```
It’s not a pretty output, but it works. Now, if we’re done with our new functionality, we can merge our changes from dev to main. From the mainbranch, git merge dev 

 
```bash
git checkout main 
git merge dev 
```

There’s a lot more to branching and merging, so for now, just let’s keep it simple.  

Now, what if we didn’t want to merge those files? If you want to rollback a commit, like before we merged Dev, we can do that with git reset. To rollback a merge, first we need the commit ID, which is an ugly string of letters and numbers. Type 

 
```
$ git log 

commit 4c9929c51640b92677c95f6e26f66d6e632a4dae 

Author: github teacher <botheaj@ucmail.uc.edu> 

Date:   Tue Feb 6 16:47:12 2018 -0500 

 

    adding new stuff 

 

commit 9803a75f77b5347fa3169e08b1bc54604c600008 

Author: github teacher <botheaj@ucmail.uc.edu> 

Date:   Tue Feb 6 16:38:24 2018 -0500 

 

    added .gitignore file 

 

commit a32bd90a88938f341e13c5c43b1cb00ecc9eb1b2 

Author: github teacher <botheaj@ucmail.uc.edu> 

Date:   Tue Feb 6 16:29:32 2018 -0500 

 

    Initial commit 
```
 

Grab that “GUID” from the “added .gitignore file” commit. Then do  

 
```
git reset –-soft 4c9929c51640b92677c95f6e26f66d6e632a4dae 
git status 
```
 

Notice our new files are now tracked, but uncommitted. --soft and --hard resets have two different results: 

 

#REF refers to the reference number in the log. 
```
git reset --soft <REF>  Stage files from another commit. Rewinds current commit 

git reset --mixed <REF>  

git reset --hard <REF> Use with caution Restores everything back to a specific commit. Loses all commits after that occurred after 
```

If we don’t want the files at all, we can follow up our git reset with a git clean -f  
 
```
$ git clean -f 
```
That removes all of the files that were left over. Good news is, we still have our dev branch, unaffected by all of these actions.  

 

# README 

The README.md file displays on your Github page as an HTML document. It’s a good way to document what your application does and how to use it.  

Let’s create a sample. Edit README.md and add the following: 

# MARKDOWN EXAMPLE
======

### Welcome My App 

====== 
 
My app is so great, sometimes it works! Just download the script, add some execute permissions and run it. The results should show you a list of all of the cat pictures on your machine. 

```javascript 
Javascript code block to highlight whats up in my code 
```

* A single star creates a large heading
** Two stars is less
*** Three stars even less
**** Four stars looks normal

======

Refer to the Markdown Cheatsheet for examples:  

https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet 

You can also look at the source of all of these notes, as they are written in markdown! 

 
Now, add your README to your repo and push. 
```
git add . 
git commit -m'added README.md' 
```
 

At this point we have not done anything to our remote branch. Now, we’ll push all the changes we’ve made to our remote branch so it can be viewable on github.com.  

 
```
git push origin main 
```


 

Now to add our dev branch, checkout dev and push that too.  

 
```bash
git checkout dev 
git push origin dev 
```
 

Verify you can see the Dev branch now as well in your github.com page. You can view your README changes on there as well.  

README’s can be very helpful for your users and SHOULD be used to let users (and me) know what you’re doing with your scripts. Hey, you can use it for Project 1! 

Branching is a valuable feature in git and will allow you to change files without risking overwriting code or functionality.  

Switch back to your main branch since that will be the primary branch we use for this class.  

```bash 
git checkout origin main 
```
 

If you’re using Windows, open VSCode and open that folder. You’ll notice there is new highlighting and git options in VSCode, because VSCode knows that this is a git repo.  

You can actually do most of your Git work within VSCode without using command line. Just click the Github icon 


# Lab 4

Complete the in-class lab. In addition, create some more folders on the main branch and include some empty files so that Git will register them (Git doesn't register empty directories). Submit the URL to your github.com repo Lab 4. 

It should include Lab 2 and Lab 3 scripts, as well as a "Dev" branch. You should also include at least these folders : powershell, bash, node, python, labs, project1.
 

 

 