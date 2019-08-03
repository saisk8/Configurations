# FireDetection using CenterNet

## Dependencies

This section has the list of de[endencies that you must have installed.

```bash
cffi==1.12.3
Cython==0.29.7
h5py==2.9.0
matplotlib==3.1.0
numpy==1.16.3
opencv-python==4.1.0.25
Pillow==6.0.0
progress==1.5
pycocotools==2.0.0
pycodestyle==2.5.0
pycparser==2.19
six==1.12.0
torch==1.1.0
torchvision==0.2.2.post3
celery==4.3.0
flask==1.0.3
```

## 1. Setup Alarm Handler

`Azure IoTHub Client` is used to handle callbacks for Alarm acceptance or rejection. First we need to build `Azure IoTHub Client` for `Python2.7`

#### Clone the Azure IoT Python SDK Repository

```bash
git clone --recursive https://github.com/Azure/azure-iot-sdk-python.git
```

#### For Ubuntu, you can use apt-get to install the right packages:

```bash
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

#### Install `Python2.7`

```bash
sudo apt install python python-dev python-pip
```

#### Verify that CMake is at least version 2.8.12:

```bash
cmake --version
```
>For information about how to upgrade your version of CMake to 3.x on Ubuntu 14.04, read How to install CMake 3.2 on Ubuntu 14.04?.

#### Verify that gcc is at least version 4.4.7:

```bash
gcc --version
```
>For information about how to upgrade your version of gcc on Ubuntu 14.04, read How do I use the latest GCC 4.9 on Ubuntu 14.04?.

#### Change Directory to the repository and go to build_all/linux

```bash
cd azure-iot-sdk-python
cd build_all/linux
```

#### Run `setup.sh`

```bash
./setup.sh
```
> Setup will default to python 2.7 | To setup dependencies for python version greater than 3, run ./setup.sh --python-version X.Y where "X.Y" is the python version (e.g. 3.4, 3.5 or 3.6)

#### Run `build.sh`

```bash
./build.sh
```
>Build will default to python 2.7 | To build with python version greater than 3, run ./build.sh --build-python X.Y where "X.Y" is the python version (e.g. 3.4, 3.5 or 3.6)

#### Copy the `.so` files to python `dist-packages`

```bash
cd ../../
cd device/samples
sudo cp iothub_client.so /usr/local/lib/python2.7/dist-packages/

# Optional
cd service/samples
sudo cp iothub_service_client.so /usr/local/lib/python2.7/dist-packages/
```

#### Verify if the package works

```bash
python -m 'import iothub_client'
```

> If you see no output, you have successfully built the Azure IoTHub Client

## 2. Setup Pipeline

The vesrion of python used is `3.7.3`
Follow the following steps to steup your system.

### System Wide Setup

#### Basic setup

```bash
sudo apt update -y && sudo apt -y upgrade
sudo apt install python3.7 python3.7-dev
sudo apt install python3-protobuf
sudo apt install ffmpeg
```

### CUDA and CUDNN Installation (For GPU Acceleration only)

#### Remove any previous versions of CUDA

```bash
sudo apt --purge remove "cublas*" "cuda*"
sudo apt --purge remove "nvidia*"
```

#### Download and Install CUDA 10.0 for Ubuntu 18.04

```bash
# Download the deb file installer
wget https://developer.nvidia.com/compute/cuda/10.0/Prod/local_installers/cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64 -O cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64.deb

# Run the installer
sudo dpkg -i cuda cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64.deb

# Add Public Key
sudo apt-key add /var/cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48/7fa2af80.pub

# Perform an update
sudo apt update

# Install CUDA
sudo apt install cuda
```

#### Download and Install cudNN 7.5.0 for CUDA 10.0

Head over to [cudnn-archive](https://developer.nvidia.com/rdp/cudnn-archive) and download the runtime library, developer library and Code Samples for Ubuntu 18.04

*Note: You will need an account, sign up if you don't have one.*

```bash
sudo dpkg -i libcudnn7_7.5.0.56-1+cuda10.0_amd64.deb 
sudo dpkg -i libcudnn7-dev_7.5.0.56-1+cuda10.0_amd64.deb 
sudo dpkg -i libcudnn7-doc_7.5.0.56-1+cuda10.0_amd64.deb  
``` 

### Using Virtual Environment for Python Dependencies

>It is suggested to use virtual environments to install dependencies.

```bash
pip3 install virtualenv virtualenvwrapper
```

Next, setup environment variables in your `.bashrc` if you use BASH shell and in  `.zshrc` if you use ZSH shell.

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

*Pro Tip: To delete your virtual environment, use the following command*

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

>You can also install them by yourself. Refer to the list above.
