## Introduction

This repository has the list of configuration file that I use to setup my system when I do a clean install of the OS.
This is for Debian based systems only.

## Table of Contents

Todo

## CUDA toolkit installation and cuDNN setup

> The vesrion of python used is `3.7.3` Follow the following steps to steup your system.

### Basic setup

```bash
sudo apt update -y && sudo apt -y upgrade
sudo apt install python3.7 python3.7-dev
sudo apt install python3-protobuf
sudo apt install ffmpeg # For opencv
```

### CUDA and CUDNN Installation (For GPU Acceleration only)

#### Remove any previous versions of CUDA

```bash
sudo apt --purge remove "cublas*" "cuda*"
sudo apt --purge remove "nvidia*"
```

#### Download and Install CUDA 10.0 for Ubuntu 18.04

Navigate [here](https://developer.nvidia.com/cuda-toolkit) and download the cuda installer. You would want to download the `deb` file.

The below instructions are for cuda 10.1 only.

```bash
# Run the installer
sudo dpkg -i cuda-repo-ubuntu1804-10-1-local-10.1.168-418.67_1.0-1_amd64.deb

# Add Public Key
sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub # Usually displayed at the end of the above instruction. Copy that line and run it.

# Perform an update
sudo apt update

# Install CUDA
sudo apt install cuda
```

#### Post installation actions

Some actions must be taken after the installation before the CUDA Toolkit and Driver can be used.

##### Environment Setup

The PATH variable needs to include `/usr/local/cuda-10.1/bin` and `/usr/local/cuda-10.1/NsightCompute-<tool-version>`. `<tool-version>` refers to the version of Nsight Compute that ships with the CUDA toolkit, e.g. `2019.1`.

To add this path to the PATH variable:

```bash
export PATH=/usr/local/cuda-10.1/bin:/usr/local/cuda-10.1/NsightCompute-2019.1${PATH:+:${PATH}}
```

### Using Virtual Environment for Python Dependencies

> It is suggested to use virtual environments to install dependencies.

```bash
pip3 install virtualenv virtualenvwrapper
```

Next, setup environment variables in your `.bashrc` if you use BASH shell and in `.zshrc` if you use ZSH shell.

```bash
# For ZSH
echo '# virtualenv and virtualenvwrapper' >> .zshrc
echo 'export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3' >> .zshrc
echo 'export WORKON_HOME=$HOME/.virtualenvs' >> .zshrc
echo 'source ~/.local/bin/virtualenvwrapper.sh' >> .zshrc # For Python 3.7 only

# For BASH
echo '# virtualenv and virtualenvwrapper' >> .bashrc
echo 'export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3' >> .bashrc
echo 'export WORKON_HOME=$HOME/.virtualenvs' >> .bashrc
echo 'source ~/.local/bin/virtualenvwrapper.sh' >> .bashrc # For Python 3.7 only
```

Now, run the following command or alternatively start a new terminal.

```bash
source ~/.zshrc # For ZSH
source ~/.bashrc # For BASH
```

To create a virtual environment, use the following:

```bash
mkvirtualenv <name_of_env>
```

To enable/disable your virtual environment:

```bash
# To enable
workon <name_of_env>
# To disable
deactivate
```

_Pro Tip: To delete your virtual environment, use the following command_

```bash
rmvirtualenv <name_of_env> # Be as sure as you can...
```

### Installing Dependencies

Run this command to install all dependencies.

```bash
# Activate the virtual environment
workon <env-you-created>
## After you run the command, look for a change in the prompt.
## The nameof the env must ve displayed.

# Now, run the following to install the dependencies
pip3 install -r requirements.txt
```

> You can also install them by yourself. Refer to the list above.
