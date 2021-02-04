# Tutorial on SSH security

## Server configuration

Open the `/etc/ssh/sshd_config` to set sane defaults.

Disable root login. The would be the most common target by bots, because a
username is easily known in advance without guessing. You will still be able to
login as a normal user and then sudo.
```
PermitRootLogin no
```

Users with empty passwords should not be able to use ssh, this goes without explanation I guess:
```
PermitEmptyPasswords no
```

OpenSSH can read `/etc/passwd` on its own, therefore it is a good idea to disable PAM (and all its modules) altogether. Who knows where lies the weakness.
```
UsePAM no
ChallengeResponseAuthentication no
```

Generally it is a best idea to disable passwords altogether and use
public/private key cryptography only.
```
PasswordAuthentication no
```

Make the process dealing with network traffic less privileged:
```
UsePrivilegeSeparation sandbox
```

There have been a security issue in X11 forwarding in the past, so disable it
unless you really need it.
```
X11Forwarding no
```

If you are the only person logging to the machine, consider changing the
default port:
```
Port 2222
```
Believe me or not, if you leave it on the default `22`, bots will spam your log
a lot.

## Client commands

Create ssh keys - it is really easy:
```
ssh-keygen
```

If you wonder what is the public key of the current host, run this command:
```
ssh-keygen -l -f /etc/ssh/ssh_host_rsa_key
```
