# Workspace Setup
---

### Configuring SSH Keys

---

### Partially Automating the Process

So far, we've done the following steps :

* We've generated a SSH Key Pair
* We've added the SSH public key to Github
* We've committed work to our repository
* We then added our SSH key to the ssh-agent so we don't have to re-type our password for every commit

We can automate this last step. Rather than typing our password every time we want to push to our repo, we only have to type our password once per shell session. We can automate this process in two steps:

1. Configure Bash to start ssh-agent on shell session open
2. Configure the ssh service to add the key to the agent after the initial use and password prompt

#### Configuring Bash to start ssh-agent on open

We can configure bash to start up `ssh-agent` on open. To do so, we need to edit our `.bashrc` (short for bash read command) which is a script that runs every time we start a shell session. Open your `.bashrc` file with nano. 

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

#### Automatically adding our SSH key to the ssh-agent

We can add a `config` file to our `~/.ssh` folder in order to set some settings for SSH communication. 

```bash
nano ~/.ssh/config
```

In the config file you can add the following lines. This configuration adds a key, once used to the ssh-agent for all hosts. If you wanted to further specify this behavior, you could change the asterisk to the host of your choice.

```bash
Host *
  AddKeysToAgent yes
```

Save the file and close. And there we have it! You will only have to enter your password once per shell session when working with Git. 
