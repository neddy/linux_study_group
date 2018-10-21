# Setting Up New Server

## Create new droplet on DigitalOcean... Then follow below.

## Login to new server
```
ssh root@ip
```

## Create a new user

```
# Create user
adduser USER

# Add to sudo group if admin access is required
usermod -aG sudo USER
```

## Adding ssh keys to new user
```
# Create ssh directory in new users home folder
mkdir /home/USER/.ssh

# add ssh key to new user's account
nano /home/USER/.ssh/authorized_keys
# Then paste in pub ssh key(s)

# Fix file permission of new ssh directory
# change ownership
chown -R USER:USER /home/USER/.ssh
chmod 700 /home/USER/.ssh
chmod 644 /home/USER/.ssh/authorized_keys
```

## Test new ssh connection
Log out of root ssh session, then login back in, but using the new username instead of root
```
ssh USER@IP
```
If this works, then we can disable root ssh access by editing the ssh daemon config file `/etc/ssh/sshd_config`. 

# Need to fix this section (look up how to restrict to key authentication only)
```
# (Full file contents omitted...)
# Make the following changes
PermitRootLogin yes # Change to no
PubkeyAuthentication yes # Make sure this is yes
ChallengeResponseAuthentication no # Make sure it's no
PasswordAuthentication yes # Change to no
```

Then restart the ssh server
```
sudo systemctl restart ssh
```

Now logout of the ssh connection, and try to login as the root user, you should get:
```
Permission denied (publickey).
```

## Edit your welcome message
https://ownyourbits.com/2017/04/05/customize-your-motd-login-message-in-debian-and-ubuntu/
