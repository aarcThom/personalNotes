# Workspace Setup

### Setting up GIT



#### Configuring Bash to start ssh-agent on open

We can configure bash to start up `ssh-agent` on open. This is helpful as we can also configure our SSH keys to be automatically added to `ssh-agent` after one Github password prompt. In other words, rather than typing our password every time we want to push to our repo, we only have to type our password once.

Open your `.bashrc` file with nano. 

```bash
nano ~/.bashrc
```

Paste the following command into the end of the document, save, and close the file. 

```bash
#starting the ssh-agent - but suppressing the output
eval `ssh-agent` > /dev/null
```

On a side note, `> /dev/null` is a really useful command that redirects the *standard output* ( `stdout`)  to the null device `/dev/null`.

You can confirm that `ssh-agent` is running by typing `echo "$SSH_AUTH_SOCK"`.

```bash
$ echo "$SSH_AUTH_SOCK"
/tmp/ssh-XXXXXX2dfjZo/agent.13048
```
