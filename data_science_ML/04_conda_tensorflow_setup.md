# TensorFlow Setup in Miniconda Using an Nvidia GPU

---

### References

After trying to follow a bunch of Medium articles, I figured this out by referring solely to the official documentation. Lesson learned. I've followed the following guides (as of November, 2023):

* [Install TensorFlow with Pip](https://www.tensorflow.org/install/pip)
* [CUDA on WSL User Guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)

---

### Assumptions

These notes assume you have an Nvidia GPU that uses CUDA 3.5 or higher. You can check your card [here](https://developer.nvidia.com/cuda-gpus).

---

### Install the Most Up to Date Nvidia Drivers on the Windows Side

NOTE: The CUDA instructions above refer to the Game Ready driver. I actually installed the Studio Driver and everything is working AOK. The studio driver is probably what most designers would install, so this is worth mentioning.

Download the most current Nvidia driver for your GPU [here](https://www.nvidia.com/download/index.aspx). Follow the prompts and wait until installation is complete. I'd restart your computer afterwards just to be safe.

**IMPORTANT NOTE:** Do not install any Nvidia drivers within WSL2. From Nvidia's documentation:

> Once a Windows NVIDIA GPU driver is installed on the system, CUDA becomes available within WSL 2. The CUDA driver installed on Windows host will be stubbed inside the WSL 2 as `libcuda.so`, therefore **users must not install any NVIDIA GPU Linux driver within WSL 2**.

Once you have installed the Nvidia driver on the Window's side, you can confirm that it is working within WSL2 by typing `nvidia-smi`. You should see something similar to this:

```bash
thomas@TI83:~$ nvidia-smi
Wed Nov  1 22:54:23 2023
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.120                Driver Version: 537.58       CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA GeForce RTX 2070        On  | 00000000:01:00.0  On |                  N/A |
|  0%   41C    P8              24W / 175W |    670MiB /  8192MiB |      4%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|    0   N/A  N/A        23      G   /Xwayland                                 N/A      |
+---------------------------------------------------------------------------------------+
```



---

### Install CUDA Toolkit

The commands for installation can be found [here](https://developer.nvidia.com/cuda-downloads). Copy each line, one by one. Ensure that you are selecting the following platform options:

* OS = Linux
*  Architecture = x86_64
* Distribution = WSL-Ubuntu
* Version = 2.0
* Installer Type = deb(network) (local is also fine but you'll just have to delete the installer file afterwards)

Make sure that you are installing this outside of a Conda environment. This is a WSL2 wide install.

---

### Upgrade Pip

We've been talking Conda throughout these notes, but [pip](https://en.wikipedia.org/wiki/Pip_(package_manager)) is another package manager. In fact, it is the original package manager. We are going to use pip to install TensorFlow (and the related [CUDA](https://en.wikipedia.org/wiki/CUDA) / [cuDNN](https://developer.nvidia.com/cudnn) libraries) in one easy command. To upgrade pip (inside a Conda environment):

```bash
pip install --upgrade pip
```

---

### Install TensorFlow

Within your Conda environment, we can install TensorFlow (and the related [CUDA](https://en.wikipedia.org/wiki/CUDA) / [cuDNN](https://developer.nvidia.com/cudnn) libraries) like so:

```bash
pip install tensorflow[and-cuda]
```

---

### Verify the Installation

Still within our Conda environment, we can verify that we've successfully installed TensorFlow with a GPU setup with the following command:

```bash
python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

If you have successfully enabled your GPU to work with TensorFlow, you will get an output similar to below. Note that, according to experts, TensorFlow [vomits a lot of useless crap at you](https://youtu.be/0S81koZpwPA?feature=shared), and we are really only concerned with the last line that's output from this command.

```bash
# the command
(env)thomas@TI83:~/repos/tfTest$ python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"

#start of crap vomiting
2023-11-02 23:42:44.606803: E tensorflow/compiler/xla/stream_executor/cuda/cuda_dnn.cc:9342] Unable to register cuDNN factory: Attempting to register factory for plugin cuDNN when one has already been registered
2023-11-02 23:42:44.606844: E tensorflow/compiler/xla/stream_executor/cuda/cuda_fft.cc:609] Unable to register cuFFT factory: Attempting to register factory for plugin cuFFT when one has already been registered
2023-11-02 23:42:44.608321: E tensorflow/compiler/xla/stream_executor/cuda/cuda_blas.cc:1518] Unable to register cuBLAS factory: Attempting to register factory for plugin cuBLAS when one has already been registered
2023-11-02 23:42:44.723861: I tensorflow/core/platform/cpu_feature_guard.cc:182] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
To enable the following instructions: AVX2 FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
2023-11-02 23:42:47.074345: I tensorflow/compiler/xla/stream_executor/cuda/cuda_gpu_executor.cc:880] could not open file to read NUMA node: /sys/bus/pci/devices/0000:01:00.0/numa_node
Your kernel may have been built without NUMA support.
2023-11-02 23:42:47.096695: I tensorflow/compiler/xla/stream_executor/cuda/cuda_gpu_executor.cc:880] could not open file to read NUMA node: /sys/bus/pci/devices/0000:01:00.0/numa_node
Your kernel may have been built without NUMA support.
2023-11-02 23:42:47.096754: I tensorflow/compiler/xla/stream_executor/cuda/cuda_gpu_executor.cc:880] could not open file to read NUMA node: /sys/bus/pci/devices/0000:01:00.0/numa_node
Your kernel may have been built without NUMA support.
# end of crap vomiting

#the important thing
[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
```

If you see a `PhysicalDevice` within that final Python list, you are good to go!

---

### Troubleshooting if Something Goes Wrong

As of November 3, 2023, it appears that Python 3.11 does not support the `[and-cuda]` option. In other words, when installing TensorFlow in a Python 3.11 Conda environment, you will not be able to install all the related CUDA drivers with the handy `pip install tensorflow[and-cuda]` command. You can confirm this by looking for this line buried within the TensorFlow install output:

```bash
#lots of stuff before this
Collecting tensorflow[and-cuda]
  Using cached tensorflow-2.13.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (3.4 kB)
WARNING: tensorflow 2.13.1 does not provide the extra 'and-cuda' # <--- THIS IS THE LINE YOU ARE LOOKING FOR
Collecting gast<=0.4.0,>=0.2.1 (from tensorflow[and-cuda])
  Using cached gast-0.4.0-py3-none-any.whl (9.8 kB)
Collecting keras<2.14,>=2.13.1 (from tensorflow[and-cuda])
#lots of stuff after this
```

This is a reported and reproducible [error](https://github.com/tensorflow/tensorflow/issues/61993).

If you have just created a new directory for your project and are just beginning to set things up, it's easiest to just delete the current environment and start again using Python 3.10.

```bash
(env)thomas@TI83:~/repos/ai_for_coders/01_intro$ conda deactivate
thomas@TI83:~/repos/ai_for_coders$ cd ..
thomas@TI83:~/repos/ai_for_coders$ rm 01_intro/ -r
thomas@TI83:~/repos/ai_for_coders$ mkdir 01_intro
thomas@TI83:~/repos/ai_for_coders$ cd 01_intro/
thomas@TI83:~/repos/ai_for_coders/01_intro$ conda create --prefix ./env python=3.10
```

If you need to downgrade without deleting your environment, you can downgrade Python:

```bash
(env)thomas@TI83:~/repos/ai_for_coders/01_intro$ conda install python=3.9.0
```

I noticed that TensorFlow was uninstalled when I downgraded Python. You can confirm this by using the command `conda list`:

```bash
(env)thomas@TI83:~/repos/ai_for_coders/01_intro$ conda list
# packages in environment at /home/thomas/repos/ai_for_coders/01_intro/env:
#
# Name                    Version                   Build  Channel
_libgcc_mutex             0.1                        main
_openmp_mutex             5.1                       1_gnu
bzip2                     1.0.8                h7b6447c_0
ca-certificates           2023.08.22           h06a4308_0
ld_impl_linux-64          2.38                 h1181459_1
libffi                    3.3                  he6710b0_2
# ... more stuff below but NO TENSORFLOW

```

If this is the case, reinstall TensorFlow as before. 

Otherwise, if it does appear, test to see if it is working by verifying your GPU (as above). If TensorFlow is installed but does not work after downgrading Python, your best bet is use `pip uninstall tensorflow` and then repeat, `pip install tensorflow[and-cuda]`. If this doesn't work, I'd probably just delete the environment and recreate it without touching the actual code.
