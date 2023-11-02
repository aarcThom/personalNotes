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

The commands for installation can be found [here](https://developer.nvidia.com/cuda-downloads). Ensure that you are selecting the following platform options:

* OS = Linux
*  Architecture = x86_64
* Distribution = WSL-Ubuntu
* Version = 2.0
* Installer Type = deb(network) (local is also fine but you'll just have to delete the installer file afterwards)

Make sure that you are installing this outside of a Conda environment. This is a WSL2 wide install.

---







