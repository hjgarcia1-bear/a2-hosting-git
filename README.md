# Using Git instead of FTP

The following relates to [A2 Hosting](http://www.a2hosting.com) but it should be a similar process for any web host server. For A2 Hosting, public websites should be placed in the **public_html** folder in your home directory. Your home directory path is **/home/username** where username is your A2 Hosting username. There are 3 main steps to using Git as a replacement for FTP with your web host:  
1. Connect to your web host via SSH  
2. Configure Git on the web host (remote)  
3. Configure Git on your local machine

## Connect to your web host via SSH

Remotely log in via SSH in your Terminal (or other command-line interface) using your A2 **username** and the **domain name** associated with your account. Notice that **x** should be replaced by the default port for A2 Hosting.

```
ssh -p x username@domain.com
```
After entering the above command in the Terminal, enter your SSH password. You should now be logged in to your A2 Hosting account and be able to browse the files and directories associated with your account.

## Configure Git on the web host

Once logged in via SSH, setup a directory on your web host for the Git version control that will be associated with a particular web site. The following example places the folder in the home directory of your web hosting account.
```
mkdir example.git
```
Now go to the **public_html** directory and create a folder for your website files
```
cd public_html
mkdir example.com
```
Go back to the Git directory **/home/username/example.git** then initialize a bare repository for the Git version control.
```
cd /home/username/example.git
git init --bare
````
After the Git has been initialized, go to the hooks folder and create a post-receive file.  
```
cd hooks/
cat > post-receive
```
Now add the following lines to the post-receive file.  
```
#!/bin/sh
git --work-tree=/home/username/public_html/example.com --git-dir=/home/username/example.git checkout -f
```
After adding the above lines, save the post-receive file by pressing **control d** on your keyboard. Next, change the permissions of the post-receive file using the **chmod** command. 
```
chmod +x post-receive
```

## Configure Git on your local machine

Now do the following on your local machine. Create a folder for your web site files.
```
mkdir testing
```
Go to the local folder, then initialize Git in this local folder.
```
cd testing
git init
```
After Git is initialized in the local folder, set up a remote path via SSH. This tells Git to add a remote repository (the Git folder on your web host) named **live**. Remember that **x** is the SSH port number.
```
git remote add live ssh://username@domain.com:x/home/username/example.git
```
Now add some files to the local Git folder. If deploying a web site, these files are your HTML, CSS, Javascript, etc. that are used to create your site. 
```
touch file.txt
```
After you have created your web site and added all necessary files, you must **add** and **commit** them to Git.
```
git add .
git commit -m 'message here'
```
Finally, push your web site files to your web host to make them viewable to the public.
```
git push -u live master
```

## Clone the remote repository

You can download the web site files from your web host via SSH using the **clone** command in Git. Again, note that **x** is the SSH port number used for your web host.
```
git clone ssh://username@domain.com:x/home/username/example.git
```

