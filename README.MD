<h1>Create SSH keys with different names having different accounts</h1>
<b>1. Generate ssh key from bash</b>
  
  <pre>$ ssh-keygen -t rsa -C "your_name@email.com"</pre>

When you see this message

Generating public/private rsa key pair. 
Enter file in which to save the key (/home/user_name/.ssh/id_rsa):
Enter unique name, for example:

id_rsa_home

<b>2.Next, you'll be asked to enter a passphrase.</b>

So, you'll have created SSH key for your home account, now you can generate SSH key for your company account.

<b>3.Call SSH key generator again with second mail.</b>

<pre>$ ssh-keygen -t rsa -C "your_name@company_email.com"</pre>

Enter name for file

id_rsa_company

<b>4.After all steps you can check that all keys were created.</b>

<pre>$ ls ~/.ssh</pre>
You should see a similar files list:

id_rsa_home  id_rsa_company  id_rsa_home.pub  id_rsa_company.pub

<b>5.Now you need a config file for organise these keys.</b>

<pre>
$ cd ~/.ssh/
$ touch config
$ nano config
</pre>
Add into config file:



<pre>
# Home
  Host gitlab-home
  HostName gitlab.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_home

# Company
  Host gitlab.com-company
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_company

</pre>

<b>Next you'll delete cached keys</b>
<pre>$ ssh-add -D</pre>

If you see a message

Could not open a connection to your authentication agent.
Then enter:

eval `ssh-agent -s`

and try again previous command.

Next, you can check that your keys were added:
<pre>
$ ssh-add -l
</pre>
2048 d4:e0:39:e1:bf:6f:e3:26:14:6b:26:73:4e:b4:53:83 /home/user/.ssh/id_rsa_home (RSA)

2048 7a:32:06:3f:3d:6c:f4:a1:d4:65:13:64:a4:ed:1d:63 /home/mateusz/.ssh/id_rsa_company (RSA)

If you haven't any entries then you should add your keys
<pre>
ssh-add ~/.ssh/id_rsa_company

ssh-add ~/.ssh/id_rsa_home
</pre>
<b>Now you can check connection</b>

<pre>
$ ssh -T git@home.github.com
</pre>

Hi home_user!
<pre>
$ ssh -T git@work.github.com
</pre>
Hi company_user! 

<b>IF YOU HAVE ALREADY EXISTING PROJECT THEN UPDATE GIT URL</b>
<pre>
git remote set-url origin git@gitlab-home:your_git_project_clone_url.git
</pre>
<b>IF YOU ARE CLONING NEW REPO THEN</b>
<pre>
git clone git@gitlab-home:your_git_project_clone_url.git
</pre>