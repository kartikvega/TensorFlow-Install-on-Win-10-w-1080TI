# Installing Tensorflow on Windows 10 with GE Force 1080 TI GPU

Over the weekend started to build my new [server](https://pcpartpicker.com/b/XrMZxr) for deep learning exercises and after going over a lot of materials wanted to put together a simple installation guide for putting together all the concoctions for installing Tensorflow(TF) on Win10 running on NVidia GPU. There are a lot of materials with Tensorflow installation for Ubuntu, and I do have a Ubuntu installed as a dual boot and I found it was way easier to install configure all the CUDA/cDNN drivers on Linux than on Windows. So for anyone trying to build TF on Win10, hope this guide helps.

**System Environment.**

OS: Window 10 Enterprise (64bit)

RAM: 64GB DDR

HD: 1TB SSD, 4TB HDD

Processor: Intel Core 17-3.60GHz

GPU: GEForce 1080 TI

**Software Required**

1. **1.** Python 3.5.2 , Anaconda 3 4.2.0(x64)
2. **2.** Visual Studio 2015 Community Edition w Update 3(en\_visual\_studio\_community\_2015\_with\_update\_3\_x86\_x64\_web\_installer\_8922963.exe)
3. **3.** NVidia Drivers v3.8.1
4. **4.** CUDA Toolkit (cuda\_8.0.61.2\_windows.exe)
5. **5.** cudnn-8.0-windows10-x64-v5.1
6. **6.** Tensorflow 1.2.1/Keras 2.0.5

**Install and Configure**

**Anaconda/Python Install**

Instead of going for the latest and greatest version of Python(3.6 at this time of writing) I prefer to stay a version behind as most of the libraries are trying to catch up as well and I can use most of them w 3.5.2 without issues. Anaconda 4.2.0 will install Python 3.5.2 for you if you prefer to have it install it for you. You can find the installer in the archive section on the [continuum site](https://repo.continuum.io/archive/) Follow the guide with the default options as listed [here](https://docs.continuum.io/anaconda/install/windows). Once the installation is complete make sure to add the C:\ProgramFiles\ Anaconda3, C:\ProgramFiles\Anaconda3\Scripts and C:\ProgramFiles\Anaconda3\Library\bin to your path variable. If you have installed in on another drive, make sure to change the path accordingly

### **Visual Studio Install:**

To get the VS2015 community edition installer is a bit involved. You first need to go to this site [here](https://www.visualstudio.com/vs/older-downloads/). Join the free dev essentials and search for Visual Studio 2015 Community Edition Update 3. Download that to your computer, on the setup screen, select custom and make sure the Windows 10 SDK (10.0.10240) is selected, click next and click install. After install, make sure the &quot;C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin&quot; is included in the Bin folder (assuming you have it installed on C:\)

### NVidia Drivers v3.8.1:

For me this was the trickiest part of the entire setup I tried to install the latest driver that NVidia has to offer for GEForce 1080 TI GPU,  and the installation went smoothly, however, when I installed CUDA/cDNN/Tensorflow, it wouldn&#39;t recognize my GPU. Tried to debug / looked at the nvidia-smi for information, ran simple GPU applications in C++ which worked fine, but Tensorflow wouldn&#39;t recognize the GPU Device. So applied the same rule of thumb I used for python, not to use the latest version and rollback to an older version ( [381.65-desktop-win10-64bit-international-whql.exe](https://www.geforce.com/drivers/results/117263)) and everything fell in place.

 ins pic

**Select the custom option and make sure the components are selected, click next and let it install.**

## CUDA Toolkit (cuda\_8.0.61.2\_windows.exe)

After the driver is installed you can now install the CUDA toolkit. Be aware that CUDA toolkit will also try to install NVidia Drivers so make sure to deselect them while installing CUDA toolkit. When i start the installation I do get the error below, not sure the reason yet, but it didn&#39;t hinder anything and the install went smoothly and TF was working

inspic
 

Under Options select custom and since we have already installed the drive, uncheck the driver components and other components and just select CUDA and install.

 inspic

This should install successfully.  After installations make sure the paths below are included in the environment variables and path.

1. CUDA\_Home, CUDA\_PATH, CUDA\_PATH\_V8\_0 all would point to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0.
2. In the path make sure the below paths exsist.

C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin

C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\libnvvp

C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\extras\CUPTI\libx64

C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\lib\x64

## cudnn-8.0-windows10-x64-v5.1

Download the compatible [cudnn](https://developer.nvidia.com/rdp/cudnn-download) and extract it. Now copy the files within the bin,lib,include into the same exact folders within C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0 folder.

## Tensorflow 1.2.0 and Keras 2.0.5

You can create a separate conda environment for Tensorflow  using

$ conda create -n TF-gpu python=3.5.2

$ activate TF-gpu

To install keras 2.0.5 you can use pip install command

(TF-gpu) $ pip install keras==2.0.5

To install Tensorflow GPU 1.2.0, you can simply use the pip install command

 (TF-gpu) $  pip install tensorflow-gpu==1.2.0

## Validating the Install

If there are no evident errors on the install command output above, you can quickly validate your Tensorflow install.

1. Are you able to import Tensorflow?

Go into the python console and then type

import Tensorflow as tf

sess = tf.Session(config=tf.ConfigProto(log\_device\_placement=True))

The commands above should output

&gt;&gt;&gt; sess = tf.Session(config=tf.ConfigProto(log\_device\_placement=True))

2017-08-22 09:06:21.377231: I c:\tf\_jenkins\home\workspace\release-win\m\windows-gpu\py\35\tensorflow\core\common\_runtime\gpu\gpu\_device.cc:1030] Creating TensorFlow device (/gpu:0) -&gt; (device: 0, name: GeForce GTX 1080 Ti, pci bus id: 0000:02:00.0)

Device mapping:

/job:localhost/replica:0/task:0/gpu:0 -&gt; device: 0, name: GeForce GTX 1080 Ti, pci bus id: 0000:02:00.0

2017-08-22 09:06:21.380342: I c:\tf\_jenkins\home\workspace\release-win\m\windows-gpu\py\35\tensorflow\core\common\_runtime\direct\_session.cc:265] Device mapping:

             /job:localhost/replica:0/task:0/gpu:0 -&gt; device: 0, name: GeForce GTX 1080 Ti, pci bus id: 0000:02:00.0

1. Ensure you get the output when you run the comparison script for CPU vs GPU as located [here](https://gist.github.com/3h4/f6e9cabbead201056c4705c2590d3d21#file-0-matrix-py) using Jupyter notebook.

**References**

1. [**Installing and Updating GTX 1080 Ti Drivers / CUDA on Ubuntu**](https://blog.nelsonliu.me/2017/04/29/installing-and-updating-gtx-1080-ti-cuda-drivers-on-ubuntu/)
2. [**ML subreddit**](https://www.reddit.com/r/nvidia/comments/69uv4m/should_i_install_the_driver_for_the_gtx_1080_ti/)
3. [**Installing-Tensorflow-with-GPU**](https://github.com/nicolaifsf/Installing-Tensorflow-with-GPU)
