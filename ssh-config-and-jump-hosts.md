# SSH config and jump hosts

Many of our machines require successively logging through several jump hosts to reach. Remembering and typing all those ssh commands everytime is tedious and a completely unnecessary cognitive burden. Instead, put this into your `~/.ssh/config` taking care to replace `rmott` and `ucbtabc` with your respective usernames on those machines.
```
Host tails
  HostName tails.cs.ucl.ac.uk
  User rmott

Host wise
  HostName wise
  User rmott
  ProxyJump tails

Host pchuckle
  HostName pchuckle
  User rmott
  ProxyJump tails

Host baddiel
  HostName baddiel
  User rmott
  ProxyJump tails,pchuckle

Host ucl-ssh-gateway
  HostName ssh-gateway.ucl.ac.uk
  User ucbtabc

Host myriad
  HostName myriad.rc.ucl.ac.uk
  User ucbtabc
  ProxyJump ucl-ssh-gateway
```
Then, you can login into any of these machines by simply running
```
ssh baddiel
ssh myriad
```
and similar.
