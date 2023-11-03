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
