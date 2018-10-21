## Generate new Key (ed25519)

[reference](https://medium.com/risan/upgrade-your-ssh-key-to-ed25519-c6e8d60d3c54)

```
ssh-keygen -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "john@example.com"
```

## Generate new Key (RSA 4096 bit)

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

## Add key to SSH agent
[More info about ssh-agent](https://www.ssh.com/ssh/agent)

```
ssh-add ~/.ssh/id_ed25519
```
(replace file with correct file if not adding your new `id_ed25519` as shown above)

## List SSH agent keys

```
ssh-add -l
```

## Remove all keys from SSH agent

```
ssh-add -D
```

## Add settings to `~/.ssh/config`

[reference](https://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/)

```
### default for all ##
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
  IdentityFile ~/.ssh/id_rsa
 
## override as per host ##
Host server1
     HostName server1.cyberciti.biz
     User nixcraft
     Port 4242
     IdentityFile ~/.ssh/id_rsa_old
```

## Process to replace old ssh keys with new ones

```
# Generate new key (Adjust file and user details to suit)
ssh-keygen -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "john@example.com"

# Rename old SSH key
mv ~./ssh/id_rsa ~./ssh/id_rsa_old
mv ~./ssh/id_rsa.pub ~./ssh/id_rsa_old.pub

# List keys loaded in ssh agent
ssh-add -l

# Remove all ssh keys if old ones are listed above
ssh-add -D

# Add desired keys back to ssh-agent
ssh-add ~/.ssh/id_ed25519

## Next add your new public key to a server to test
# Connect to server with old key
ssh -i ~/.ssh/id_rsa_old user@example.com

# Add new key to ~/.shh/authorized_keys
nano ~/.ssh/authorized_keys #Then paste in keys

# Test if new keys work
# exit ssh session, then try to login with default key
ssh user@example.com

# That should be it.
```
