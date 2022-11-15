# bumsoo.install
Install notes for private computers

## 1. Install NVIDIA-driver
GIGABYTE based BIOS setting
- First, DO NOT PLUG IN your GPU until the driver is set up.
- Internal Graphic : Auto > Enable
- Display Priority : PCIe > Internal

NVIDIA driver download .run file : [click here](http://www.nvidia.co.kr/Download/index.aspx)

If you click download from the above site, you will get a .run file format for installing drivers.

### 1-1. Stop display manager

Before you run the .run file, you first need to stop your Xserver display manager.

Press [Ctrl] + [Alt] + [F1], enter the script below

```bash
$ service --status-all | grep dm

(Result) [+] [:dm]
```

The part described as [:dm] is your display manager.

Substitute the [:dm] part below with the result of the script above.

```bash
$ sudo service [:dm] stop

(Result) * Stopping Light Display Manager [:dm]
```

### 1-2. Run the nvidia-driver installer

Run the code below. Press 'Yes' for every option they ask.

```bash
$ sh <DIR where you downloaded the .run file>/NVIDIA-Linux_x86_64-390.87.run
```

In cases, the newest nvidia driver provided in the homepage happens to not work.

You may simply determine your latest available driver [here](https://launchpad.net/~graphics-drivers/+archive/ubuntu/ppa), then install via PPA.

```bash
sudo apt-get purge nvidia* # Only if you have any previous versions of nvidia driver installed.

# add PPA
sudo add-apt-repository ppa:graphics-drivers
sudo apt-get update

# for GeForce 10x series
sudo apt-get install nvidia-390

# prevent unwanted automatic updates
sudo apt-mark hold nvidia-390
```

After you have successfully installed, you shall see the same results when typing the code below.

```bash
$ nvidia-smi
```

```bash
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 390.87                 Driver Version: 390.87                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  TITAN X (Pascal)    Off  | 0000:4B:00.0     Off |                  N/A |
| 67%   86C    P2   249W / 250W |   5026MiB / 12221MiB |     82%      Default |
+-------------------------------+----------------------+----------------------+
|   1  TITAN X (Pascal)    Off  | 0000:4C:00.0      On |                  N/A |
| 88%   90C    P2   225W / 250W |   3842MiB / 12213MiB |     78%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID  Type  Process name                               Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+

```

### 1-3. Reboot the system

```bash
$ sudo reboot
```

## 2. Install CUDA toolkit

You can skip the step above and automatically install the driver within CUDA installation.

Check the details below.

### * Download the CUDA download .run file from the CUDA download page

CUDA download page : [click here](https://developer.nvidia.com/cuda-downloads)

Before executing the file, stop the display manager by following the description above.

```bash
$ sudo sh <DIR where you downloaded the .run file>/cuda_9.0.176_384.81_linux.run
```

### * Link your CUDA in .bashrc

```bash
$ sudo apt-get install vim

$ git clone https://github.com/amix/vimrc.git ~/.vim_runtime

# Awesome version
$ sh ~/.vim_runtime/install_awesome_vimrc.sh

# Basic version
$ sh ~/.vim_runtime/install_basic_vimrc.sh
```

Open your ~/.bashrc file.

```bash
vi ~/.bashrc

# Press Shift + G, Add the lines on the bottom

export PATH=$PATH:/usr/local/cuda/bin
export LD_LIBRARY_PATH=/usr/local/cuda/lib64
export CUDA_HOME=/usr/local/cuda
```

To check if the CUDA toolkit is successfully installed, type the line below.

```bash
$ nvcc --version

* (Result)
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2017 NVIDIA Corporation
Built on Fri_Sep__1_21:08:03_CDT_2017
Cuda compilation tools, release 9.0, V9.0.176
```

## 3. Install cuDNN library kit

cuDNN download page : [click here](https://developer.nvidia.com/rdp/cudnn-download)

(Membership is required, just sign in!)

Download the newest cuDNN v7.0.

```bash
$ cd <DOWNLOAD DIR>
$ tar -zxvf ./cudnn-9.0-linux-x64-v7_0_5.tar.gz
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h
$ sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

## Install awesome vimrc (for vim users)
```bash
$ sudo apt-get install vim

$ git clone https://github.com/amix/vimrc.git ~/.vim_runtime

# Awesome version
$ sh ~/.vim_runtime/install_awesome_vimrc.sh

# Basic version
$ sh ~/.vim_runtime/install_basic_vimrc.sh
```

* Disable vim folding
```bash
$ vi ~/.vimrc

set nofoldenable
```

## Linux Mint 19 한글 입력 [here](http://blog.daum.net/bagjunggyu/311)

[key settings](https://rageworx.tistory.com/1757)

```bash
$ sudo apt-add-repository ppa:hodong/nimf
$ apt update
$ apt install nimf nimf-libhangul
$ im-config -n nimf
$ apt autoremove fcitx-*
```

Set <RAlt> as 'Hangul'

```bash
$ sudo vi /usr/share/X11/xkb/symbols/altwin

# Line 5
key <RALT> { type[Group1] = "TWO_LEVEL",
             symbols[Group1] = [Alt_R, Meta_R] };

# Change the above lines into
key <RALT> { type[Group1] = "TWO_LEVEL",
             symbols[Group1] = [ Hangul ] };
```

## Welcome message

Install figlet
```bash
$ sudo apt-get install figlet
```

```bash
$ sudo vi /etc/bash.bashrc

# Press [Shift] + [G] and write the code on the bottom
clear
printf "Welcome to Ubuntu 18.04.5 LTS (GNU/Linux-Mint-19 x86_64)\n"
printf "This is the install guide made by Bumsoo Kim.\n\n"
printf " * Documentation: https://github.com/meliketoy/bumsoo.install\n\n"
printf "##############################################################\n"
figlet -f slant "Bumsoo Kim"
printf "\n\n"
printf " Data Mining & Information System Lab\n"
printf " GPU Computing machine : bumsoo@[:local_ip]\n\n"
printf " Administrator   : Bumsoo Kim\n"
printf " Please read the document\n"
printf "    https://github.com/meliketoy/bumsoo.install/README.md\n"
printf "##############################################################\n\n"
```

## Github control

* netrc configuration

```bash
$ sudo vi ~/.netrc

machine github.com
login [:username]
password [:password]
```

* Git initial settings
```bash
$ git config --global user.name "[:user_name]"
$ git config --global user.email [:user_email]
```

* git alias settings

```bash
$ git config --global alias.last 'log -p -1 HEAD'
$ git config --global alias.lg 'log -10 --pretty=format:"%h - %an : %s" --graph' # [hash / name / msg]
```

## Jupyter notebook configuration

For jupyter notebook configuration, type in the command line below.
```bash
$ jupyter notebook --generate-config

* Result :
Writing default config to: <HOME_DIR>/.jupyter/jupyter_notebook_config.py

$ vi ~/.jupyter/jupyter_notebook_config.py
```

presh [Esc], then enter /ip to find the ip configuration. You will find the line below
``` bash
## The IP address the notebook server will listen on.
#c.NotebookApp.ip = 'localhost'
```

Erase the '#' and change it into ...
```bash
c.NotebookApp.ip = '163.152.163.112' # the ip address for your server
```

presh [Esc], then enter /port to find the port number. You will find the line below
```bash
## The port the notebook server will listen on.
#c.NotebookApp.port = 8888
```

Erase the '#' and enter whatever port number you want
```bash
c.NotebookApp.port = 9999

```
## If python 3 is not there
```
python -m ipykernel install --user --name [virtualEnv] --display-name "[displayKenrelName]"
```

## Extract and Download Anaconda Environments

```bash
# Export
conda env export > environment.yml

# Download
conda env create -f environment.yml
```

Now, Enjoy! :)
