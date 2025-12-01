# Passwordless SSH key-based authentication

UCL (both Computer Science and Advanced Research Computing) give us SSH password authentication by default. But, there is a better way—one that is both more convenient and more secure at the same time—SSH key based authentication.

## Prerequisites

Before doing this, please set up [SSH config and jump hosts](./ssh-config-and-jump-hosts.md).

## Generate a SSH key pair

First, you need to generate an SSH key pair (a public key and a private key). Look in your `~/.ssh` directory; if you already have something like `id_rsa`, `id_ecdsa`, `id_ed25519`, etc., you already have an SSH key pair and can skip this step. If you don't (or prefer having a new key pair), generate the key pair using
```
ssh-keygen
```
Accept default settings by pressing ENTER when prompted. When prompted for a passphrase, leave it empty and press ENTER.

## Authorize your SSH key on the remote

To authorize our newly generated key pair on our remote machines, we need to append our public key to `~/.ssh/authorized_keys` on each remote machine.

Assuming `id_ecdsa.pub` is our new public key (it could instead be `id_rsa.pub`, `id_ed25519.pub`, etc.), we do this for tails like so.
```
ssh tails mkdir -p .ssh
cat ~/.ssh/id_ecdsa.pub | ssh tails tee -a .ssh/authorized_keys
```
First, we create an `~/.ssh` directory on the remote if it does not exist already. Then, we append our newly created key to `~/.ssh/authorized_keys`.

Repeat this process for pchuckle and any other machines you are interested in. baddiel shares its home folder with pchuckle. So, if you have done it for pchuckle, you don't have to repeat it for baddiel.

Now, the next time you login to any of these machines, you will not be prompted for a password!
