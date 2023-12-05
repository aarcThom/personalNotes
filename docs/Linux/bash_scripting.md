# Bash Scripting

---

## Start with a Shebang! (#!)

The *shebang* `#!` indicates the start of an executable in a unix-like operating system. The syntax for using the shebang is:

```bash
#! interpreter [optional-arg]
```

Since we are going to run bash scripts, we start our script with:

```bash
#! /bin/bash
```

---

## Printing Text to the Screen

You can print text to screen using `echo`. If we wanted to write a simple bash script to print 'hello word' it would look like this:

```bash
#! /bin/bash
echo "Hello word!"
```

---

## User Input

### Command Arguments

Bash uses positional parameters for command line arguments with variables numbered from `$1` to `$9`. The `$0` parameter is reserved and predefined as the name of the running script and cannot be used for any other purpose. [RedHat](https://www.redhat.com/sysadmin/arguments-options-bash-scripts) gives this good example:

The Script:

```bash
#!/bin/bash
echo "Name: $1"
echo "Street: $2"
echo "City: $3"
echo "State/Province/Territory: $4"
echo "Zip/Postal code: $5"
```

Using the script:

```bash
[student@testvm1 ~]$ script1.sh "David Both" "80486 Intel St." Raleigh NC XXXXX
Name: David Both
Street: 80486 Intel St.
City: Raleigh
State/Province/Territory: NC
Zip/Postal code: XXXXX
```



### Using read

[Phoenixnap](https://phoenixnap.com/kb/bash-read) has a good overview of the `read` utility. Check out the link to see the arguments available to `read`. One argument that I found useful for dealing with Windows-side files is `-r` which disables the backslash as an escape character:

The Script:

```bash
#! /bin/bash
echo "Paste in the Windows Folder and press [ENTER]"
read -r dir
echo $dir
```

Using the script:

```bash
thomas@ti83:~/bash_scripts$ bash windows_dir.sh 
Paste in the Windows Folder and press [ENTER]
C:\Users\rober\OneDrive\Desktop\projects
C:\Users\rober\OneDrive\Desktop\projects
```

---

## Source (.)

Linux will open a sub-shell when you are running a script. So doing something like this:

```bash
#! /bin/bash
cd "/some/dir"
```

will NOT work if you run the script like so:

```bash
bash somescript.sh
```

If you want to open the current shell in a script you need to use `source`. From [G4G](https://www.geeksforgeeks.org/source-command-in-linux-with-examples/): *source is a shell built-in command which is used to read and execute the content of a file(generally set of commands), passed as an argument in the current shell script.*

Instead you need to type one of the two lines.

```bash
source somescript.sh
#or
. somescript.sh
```

This is why we activate a Python virtual environment like so:

```bash
source venv/bin/activate
#or
. venv/bin/activate
```

