# Miniconda Setup

---

### References

I learnt ALOT from [The Carpentries Incubator's](https://github.com/carpentries-incubator) [*Introduction to Conda for Data Scientists*](https://carpentries-incubator.github.io/introduction-to-conda-for-data-scientists/index.html). A lot of my set up was gleaned from their material.

---

### Installing Miniconda

As per the [Miniconda docs](https://docs.conda.io/projects/miniconda/en/latest/index.html#quick-command-line-install), we can 'quickly and quietly' install the latest 64-bit version with a few commands.

```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
```

We can then initialize Miniconda for our Bash shell:

```bash
~/miniconda3/bin/conda init bash
```

We need to close our terminal session and reopen a new terminal for the effect to finalize. Once you've reopened your terminal, you'll see that Miniconda, by default, initializes and activates the default environment `base`. 

---

### Disabling Conda Base Auto-Activation

The `base` in parentheses indicates we're currently in that environment. **We want to avoid installing additional packages into our base environment. It's best practice to setup a new environment for each project.**

```bash
(base) thomas@TI83:~$
```

What's more, I prefer no to have Miniconda initialize automatically. To turn-off this behavior, run the following command which modifies Miniconda's configuration file `.condarc`.

```bash
conda config --set auto_activate_base false
```

---

### Activating Conda / Deactivating Conda

We can use `conda activate` to activate Conda in the `base` environment, and `conda deactivate` to deactivate the current Conda environment.

```bash
thomas@TI83:~$ conda activate
(base) thomas@TI83:~$ conda deactivate
thomas@TI83:~$
```

---

### Creating Environments with Specific Locations

One thing that has kept me from exploring Conda for quite a while is that I liked how easily [pip](https://packaging.python.org/en/latest/tutorials/installing-packages/) creates an environment in the *current directory*, and how this is default behavior. By default Conda environments are created in the `envs/` folder of the `miniconda3` directory.

It turns out, I'm just a bit lazy, and it is pretty easy to do this in Conda as well.

```bash
thomas@TI83:~$ cd repos/ # move to my repos directory
thomas@TI83:~/repos$ mkdir condaTest # make a new directory for my project 'condaTest'
thomas@TI83:~/repos$ cd condaTest/ # move to the project directory

#AND HERE LIES THE IMPORTANT STEP - ENVIRONMENT CREATION
thomas@TI83:~/repos/condaTest$ conda create --prefix ./env python=3.11
```

The last command can be broken down as:

* `conda create --prefix ./env` - This creates an environment in a new sub-directory `./env`. 
* `python=3.11` - This specifies the version of Python we want to use in this environment.

If we added a few files to the project, our folder structures looks like this.

```bash
thomas@TI83:~/repos/condaTest$ tree -L 2
.
├── code.py
├── env
│   ├── bin
│   ├── compiler_compat
│   ├── conda-meta
│   ├── include
│   ├── lib
│   ├── man
│   ├── share
│   ├── ssl
│   ├── x86_64-conda-linux-gnu
│   └── x86_64-conda_cos7-linux-gnu
└── notebook.ipynb
```

---

### Activating Environments that were Created in a Specific Directory

To activate this environment you would move to the directory where the project is located - in this case `condaTest` - and then use the command `conda activate ./env`.

```bash
thomas@TI83:~$ cd repos/condaTest/
thomas@TI83:~/repos/condaTest$ conda activate ./env
(/home/thomas/repos/condaTest/env) thomas@TI83:~/repos/condaTest$
```

___

### Changing your Environment's Name from that Long Absolute Path to Something Manageable

In the above example, our environment's name is `(/home/thomas/repos/condaTest/env)`. That is annoyingly long. You can change this behavior on a Conda-wide level by running the following command that edits your aforementioned `~/.condarc` properties file.

```bash
$ conda config --set env_prompt '({name})'
```

Now the previous example will look like this:

```bash
thomas@TI83:~$ cd repos/condaTest/
thomas@TI83:~/repos/condaTest$ conda activate ./env
(env)thomas@TI83:~/repos/condaTest$
```
