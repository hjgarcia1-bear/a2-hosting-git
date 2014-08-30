The following relates to [A2 Hosting](http://www.a2hosting.com) but it should be a similar process for any server. Public websites should be placed in the **public_html** folder in your home directory. Your home directory path is **/home/username** where username is your A2 Hosting username.

#Connect to your account via SSH

Remotely log in via SSH in your Terminal (or other command-line interface) using your A2 **username** and the **domain** associated with your account. Notice that **x** should be replaced by the default port for A2 Hosting.

```
ssh -p x username@domain.com
```
At the prompt, enter your SSH password. You should now be logged in to your A2 Hosting account.

##Version control and website folders

Setup a directory for the Git version control. This example places the folder in the home directory.
```
mkdir example.git
```

Now setup a directory for your website files.
```
cd public_html
mkdir example.com
```

##Initialize the Git repository

In the Git directory **/home/username/example.git** create a bare repository for the version control.
```
cd /home/username/example.git
git init --bare
````

Go to the hooks folder and create a post-receive file.  
```
cd hooks/
cat > post-receive
```
Now add the following lines to the post-receive file.  
```
#!/bin/sh
git --work-tree=/home/username/public_html/example.com --git-dir=/home/username/example.git checkout -f
```
After adding these lines, save the file by pressing **control d** on your keyboard.

After saving the post-receive file, change the permissions of the file. 
```
chmod +x post-receive
```

#Local Machine

Now do the following on your local machine.

##Git repo and initialize

Create a folder for your files and repository.
```
mkdir testing
```

Now initialize Git in this folder.
```
git init
```

##Add remote path and files

Setup the remote path via SSH. This tells Git to add a remote called **live**.
```
git remote add live ssh://username@domain.com:7822/home/username/example.git
```

Now add some files to the local Git folder. If deploying at website, these files are your HTML, CSS, Javascript, etc. that create your site. 
```
touch file.txt
```

Before sending the files to A2, you must add and commit them to Git.
```
git add .
git commit -m 'message here'
```

Finally, push the to your A2 web host to make them viewable to the public.
```
git push -u live master
```

##Clone repository

You can download the files in your remote repository via SSH. Where **x** is the SSH port number used by your web host.
```
git clone ssh://username@domain.com:x/home/username/example.git
```

