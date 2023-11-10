# Git Setup
---

### Creating  SSH Keys 

"SSH keys are an authentication method used to gain access to an encrypted connection between systems and then ultimately use that connection to manage the remote system." [jumpcloud](https://jumpcloud.com/blog/what-are-ssh-keys)

We will set up a SSH key on WSL to sync with our Github account.

### Check if you  already have an SSH key saved to your WSL system

Use the command `ls` (list directory contents) to see if you have any keys saved in the default `.ssh` directory.

```bash
ls ~/.ssh
```

If no directory is found, or the directory is empty, you can proceed with the following steps. If you already have an SSH key, you can skip to the *Adding your SSH key to Github* section.

### Creating a New SSH Key

To create a new SSH key, use the following command, replacing the "your email" with the email you set up in the previous `--global user.email` step.

```bash
ssh-keygen -t rsa -b 4096 -C "your email"
```

Press enter to accept the default location (`~/.ssh`).

Enter a passphrase that you will remember. *You will need to remember this passphrase.*

This command will create a private key `id_rsa` and a public key `id_rsa.pub` in the directory `~/.ssh`. To confirm this, you can type

```bash
ls ~/.ssh
```

Note: As of writing (2023-11-09), it appears that the ed25519 algorithm cannot be manage by the Ubuntu WSL2 ssh-agent. This is why we're using rsa instead. [See this link](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

---

### Adding a SSH Key to Github

First, we need to copy the contents of our public key file to our clipboard. To do so in WSL, we use the command

```bash
cat <file we want to copy contents of> | clip.exe
```

In Linux, `cat` (concatenate) reads a file and outputs the contents. A pipe `|` , in Linux, [takes the output of one command and inputs into another command.](https://www.geeksforgeeks.org/piping-in-unix-or-linux/) And within WSL, `clip.exe` accesses Windows' clip board.

So, to copy the contents of our public key, we can type

```bash
cat ~/.ssh/id_rsa.pub | clip.exe
```

 Logged into Github, go to https://github.com/settings/keys . Click *New SSH Key*. Give the key a descriptive name. `Ctrl + V` into the *key* field. Click *Add SSH key*.

---

### Committing Changes to Github

After you've made some changes to files within a given local repo, you cant commit those changes with the following commands:

```bash
git add . # stage all files within the repo
git commit -m "your message" # commit changes with a message
git push #push changes to the github repo
```

---

### Partially Automating the Process

So far, we've done the following steps :

* We've generated a SSH Key Pair
* We've added the SSH public key to Github
* We've committed work to our repository
* We then added our SSH key to the ssh-agent so we don't have to re-type our password for every commit

We can partially automate these last two steps. First, we can configure a commit-template to keep our commit messages consistent.  Second, Rather than typing our password every time we want to push to our repo, we can set up our system so that we only have to type our password once per shell session. 

---

### Configuring a Commit Message

[Lisa Wolderisksen](https://gist.github.com/lisawolderiksen) has written an impeccable git commit template that we will use.

```bash
# Title: Summary, imperative, start upper case, don't end with a period
# No more than 50 chars. #### 50 chars is here:  #

# Remember blank line between title and body.

# Body: Explain *what* and *why* (not *how*). Include task ID (Jira issue).
# Wrap at 72 chars. ################################## which is here:  #


# At the end: Include Co-authored-by for all contributors. 
# Include at least one empty line before it. Format: 
# Co-authored-by: name <user@users.noreply.github.com>
#
# How to Write a Git Commit Message:
# https://chris.beams.io/posts/git-commit/
#
# 1. Separate subject from body with a blank line
# 2. Limit the subject line to 50 characters
# 3. Capitalize the subject line
# 4. Do not end the subject line with a period
# 5. Use the imperative mood in the subject line
# 6. Wrap the body at 72 characters
# 7. Use the body to explain what and why vs. how
```

To add this to your global Git setting, open a new file with:

`nano ~/.gitmessage`

Copy the above text to the file (`Ctrl+ V`) and then save and exit by typing `Ctrl + o` and then `Ctrl + x`.

Next, tell Git to use the template file for all commits by using the command:

```bash
git config --global commit.template ~/.gitmessage
```

Now, you can leave out the option `-m` in your `git commit` command. 

Try saving saving some work in a repo and then running:

```bash
git add .
git commit
```

You'll now see the custom Git template open in Nano!

---

### Saving our SSH key Password per Shell Session

We can automate this process in two steps:

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

Close and restart the terminal.

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

Save the file and close. And there we have it! You will only have to enter your password once per shell session when working with Git!  

