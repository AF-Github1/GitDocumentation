# GitDocumentation
Self learning Git Documentation


# Resources used
```
https://git-scm.com/book/en/v2

https://docs.github.com/

https://docs.gitlab.com/

```

# Git documentation self learning

# Installation (Redhat)

Install git

**sudo dnf install git-all**

And because I like nano over other text editors

**sudo yum install nano**



# .gitignore

Lets you specify which files you do not want showing up in your git status checks or be automatically added

*.[oa]
*~

This ignores anything ending in .oa



# Bare repository

Repository you can’t commit files on. You can still fetch, push and pull from/to it, just not work on it directly


# Setting up your git server

Getting Git on a Server

You can clone an existing repository ( or just create a fresh one)

**git clone https://github.com/AF-Github1/GitTest**

Create the bare version of the repo

**git clone --bare GitTest GitTest.git**

Send it to your server machine that you have ssh access to. In my case im in AWS so I also need to present the key I'm using

**scp -i vockey.pem -r GitTest.git ec2-user@192.168.0.9:~**



ALTERNATIVE
If you don't need a key it should look like this

**scp -r gitfile.git user@ipORhostname:~**

And when you wish to clone the file from the server


**GIT_SSH_COMMAND="ssh -i vockey.pem" git clone ec2-user@192.168.0.9:~/GitTest.git**

ALTERNATIVE
Again if you don’t need the key, you can just use 

**git clone user@ipORhostname:~/gitfile.git**


# Gitlab installation and setup

Before installing read the minimum requirements page
```
https://docs.gitlab.com/ee/install/requirements.html
```
# Installation instruction official docs links
```
https://docs.gitlab.com/omnibus/

https://about.gitlab.com/install/#amazonlinux-2022
```

I used the amazon documentation because the RHEL one was not working properly for me even in my RedHat distro. The Amazon installation commands work just fine.

**sudo dnf install -y curl policycoreutils openssh-server perl**

You will need postfix if you are going to eventually setup email notifications for your gitlab instance

```
sudo dnf install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
```

**curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash**

**sudo EXTERNAL_URL="https://gitlab.example.com" dnf install -y gitlab-ee**

The installation will take a few minutes.

# External URL for accessing gitlab

Change any IPs to your own as needed

Go here

**nano /etc/gitlab/gitlab.rb**

Edit this line

**external_url "http://3.211.152.174"**

Use this command. Might take a few minutes

**sudo gitlab-ctl reconfigure**

Then try something like this link 

**http://3.211.152.174/users**

Trying to go through directly to just the IP does not work, you have to specify the users or the users/sing_in option in the link

You should be able to access gitlab with this, but first you will be met with a login screen


Use username **root**
Use the password from **/etc/gitlab/initial_root_password**

This password will get deleted after 24 hours. So **change your password after logging in**

You can do this from your profile

**Click the user profile icon to the right of the looking glass (top left side of the window)**

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/dd59661a-c0ec-4113-8ce4-336a8831b843)

**Edit Profile>Password**

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/97825b43-dffb-46fb-a498-8149b0771919)


![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/c4f78cd4-cb53-4b65-b510-3dfb5a773e52)

Then it's only a matter of changing your password to something else. 
You will need to login again immediately after changing it so don’t mess it up.


# Relevant command/processes list

**git**

Main command

**git config**

Sets config/shows config

**git config --global user.name "AF"**

Sets user name

**git config --global user.email AF@example.pt**

Sets user email

**git config --list --show-origin**

Shows configuration

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/4013ffae-61a0-4106-b646-91a1eabbfbbc)


**git config --global core.editor TextEditor**

Defines the text editor to be used when needed
As an example:

git config --global core.editor nano 

**git config --global init.defaultBranch BRANCHNAME**

Changes name of initial branch. By default the initial branch will be called Master.

**git status**

Checks state of files, like the current commit status

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/7a55c8bb-2df9-4833-a5df-f6567d836526)


Untracked files look like this

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/38279ce3-7fb4-485d-b11f-84c9041f7104)

**git add FILENAME**

Starts tracking the file with the same name

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/80f84501-d615-4d3e-95c5-9f8fed0194b9)


**git diff**

Shows differences in the files that were changed

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/4af5191b-d446-4593-898f-ac8fc3030185)


**git fetch REPNAME**

Fetches any new changes made to a repository.

**git push**

Pushes the changes for your remote repository. You will need your access token to use as the password it will ask you as it will not let you use your password to push.

The github documentation goes into greater detail on the process:

[Managing your personal access tokens - GitHub Docs 
](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

**git commit**

Commits anything that was staged to the repository. This includes removing files through git rm

**git rm FILENAME**

Removes a file

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/9b3df14c-6179-4d61-87b6-3069464923a1)

**log**

Shows all commits in the current branch, most recent first.

**git log -p**

-p or –patch shows the actual changes done for every version

**git log –all**

Shows all commits across every single branch

**git log BRANCHNAME**

Shows all commits for the specific branch you want

# Amending commits

Find the commit you wish to fix with git log

**git commit -m 'Initial commit'**

Add a file you wish to add

**git add FILENAME**

Commits the file without making a new commit, it alters the commit you chose to amend. You may even change the message if you want.

**git commit --amend**

# Unstaging files

After adding a file you might not actually want to commit you might need to unstage it

One can check the staged but yet not committed files through git status

And then you can reset any given file with this command

**git reset FILENAME**

Alternative command

**git restore -staged FILENAME**

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/fcb8524d-0362-40ed-9fa3-0a92afbcff2d)


# Undoing changes to a file

Find any file you have modified but not committed with git status

Then use this command for any file whose changes you wish to revert

**git checkout -- FILENAME**

Alternative command

**git restore FILENAME**

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/ab816928-9154-4c4f-b636-ca74bee80c04)

# remote

Lets you check on remove repositories you have cloned from

Default name for the server from which you cloned from is 'origin'

**git remote -v**

Shows the full links and operations for all your remote repositories used

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/94743c1a-7c0d-4145-b39d-74fa0396dae6)

**git remote show REPNAME**

Enables you to see more detailed information related to your remote repositories

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/87968e2f-5625-4d0c-94a3-92eb8f31f3f0)

**git remote rename NAME1 NAME2**

Renames your remote

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/ddda8595-d963-4b96-8071-2be96f34c05a)


# Adding remote repositories

You can use this command to create your own name for your repositories that you can then use to refer to them in other commands

**git remote add AF https://github.com/AF-Github1/GitTest**

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/1b9c2be1-1557-4773-a988-0fd9e795220d)

For example this command would now refer to the full repository of 
https://github.com/AF-Github1/GitTest

**git fetch AF**

# Using aliases

Enables you to modify what you need to type to output a command

For example

**git config --global alias.l log**

This make it so you only need to type git l instead of git log

You could give it your own alias that you find more convenient of course.

This doesn’t delete the previous command, you can create the git l alias and still use git log.

**git mv README.md README**

Moves and renames files.

Changing to a branch

**git checkout -b BRANCHNAME**


**git branch**

Shows all branches, shows current branch you are on

![image](https://github.com/AF-Github1/GitDocumentation/assets/133685290/9dd4b3a0-5f0b-42b0-93e1-ebcdbdf4f564)

Pushing a branch

**git push --set-upstream origin BRANCHNAME**

# Gitlab commands


**gitlab-ctl status**

Check states of all gitlab related services/components

**sudo gitlab-ctl reconfigure**

Basically the equivalent to systemctl restart service.
Do this command after you change something in the configuration






















