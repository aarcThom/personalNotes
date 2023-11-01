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

