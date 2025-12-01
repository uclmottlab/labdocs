# Emacs Tramp

This one is for Emacs users. If you don't use Emacs, you can skip this. Or, perhaps this is reason to start using Emacs!

## Prerequisites

Before doing this, please set up [SSH config and jump hosts](./ssh-config-and-jump-hosts.md) and [Passwordless SSH key-based authentication](./ssh-key-based-authentication.md).

## Edit files on remote machines from the comfort of your local Emacs

When working on remote machines by SSH, you don't have to struggle with terminal editors like nano on the remote machine. Emacs has a feature called "Tramp" that lets you edit files on the remote machine from the comfort of your local graphical Emacs.

Here's how. Suppose you are opening a file `~/foo` on `baddiel`, do `C-x C-f /ssh:baddiel:~/foo`. Notice that this is just like opening a file on your local machine, except there's a funky URL-like path involved.

## Look at images generated on remote machines

Often, you generate plots and other visualizations on the remote machine. And, since you can't view them directly on the terminal, you are forced to copy the file back to your local machine just to open them. But, Emacs Tramp can handle this as well!

Say, you'd like to look at `~/foo.png` on `baddiel`, do `C-x C-f /ssh:baddiel:~/foo.png`. The remote image appears in your local Emacs as though it were a local file.
