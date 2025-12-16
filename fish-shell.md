# fish shell

We often struggle with the default shells (csh, bash, etc.) on our HPC machines. But we don't have to!

[fish](https://fishshell.com/docs/current/fish_for_bash_users.html) is the best shell you're not using! fish has simply the best auto-completion suggestions and interactive features of any shell. And, it all works out of the box with zero configuration. Instead of me rambling on about it, just try it and you'll never turn back!

Thankfully, the CS machines (baddiel, pchuckle, etc.) have fish installed. Just run `fish` after logging in, and you will drop into the fish shell. Unfortunately, myriad does not have fish.

## Caveats
### Slightly non-standard syntax

Syntax differs slightly from bash and other POSIX (POSIX is a standard for Unix systems) shells. Check out the [fish for bash users](https://fishshell.com/docs/current/fish_for_bash_users.html) guide.

### Don't use for shell scripts

And, best to write your shell scripts in bash as usual. The fish language is nice, but you're probably better off sticking to a POSIX shell for scripts.
