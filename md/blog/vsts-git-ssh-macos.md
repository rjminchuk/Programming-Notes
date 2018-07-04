# Using GIT as a team with VSTS on MacOS with SSH

If you want to commit work to a private git repo with more than one person on your team, You'll want to use 
SSH to validate connections to your repo. Following is a brief overview of how to setup a VSTS git repo for 
access via SSH by one or many users.

Access public key from your `~/.ssh/id_rsa.pub`

Open your public key in textedit or print it in terminal.

```sh
cat ~/.ssh/id_rsa.pub
```

if you don't have a public key on your machine: execute the following command. 

```
ssh-keygen -t rsa -C "[your email]"
cat ~/.ssh/id_rsa.pub
```

When you execute the command, you have a few options. First, whether you want to put the file in a non 
standard directory (dont). Second giving your private key a passphrase. You can skip this, but if you do, 
you risk exposing any services, that know your public key, to attack if your machine is lost or stolen. If
you do chose to add a passphrase to your private key, you'll have 
to [configure ssh differently](http://www.lmgtfy.com/?q=git+config+ssh+with+passphrase).

Copy and paste your public key into your VSTS repo by navigating to `[your user icon]` > `security` > `SSH public keys` > `add`

Give your public key a descriptive name, like `[your name]` + `[your pc model]` + " Public Key"

In my case:
```Rich's 2017 MacBook Pro Public Key```

Repeat for any other users you'd like to add to your repo.

```
mkdir ~/Source
cd ~/Source/
git clone ssh://myrepo@vs-ssh.visualstudio.com:22/myproj/_ssh/myproj
```
