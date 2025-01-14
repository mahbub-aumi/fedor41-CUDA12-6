# Fedora 41 CUDA Toolkit 12.6
Installation guide for Cuda 12.6, CUDNN and Tensorrt on Fedora 41


## Setting up GCC and G++
### Install GCC 13 and G++ 13
```bash
sudo dnf install gcc-13
sudo dnf install g++-13
```

### Backup the current GCC binary and set GCC 13 as the default
```bash
sudo mv /usr/bin/gcc /usr/bin/gcc.bak
sudo ln -s /etc/alternatives/gcc /usr/bin/gcc
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-13 50
sudo update-alternatives --set gcc /usr/bin/gcc-13
```

### Backup the current G++ binary and set G++ 13 as the default
```bash
sudo mv /usr/bin/g++ /usr/bin/g++.bak
sudo ln -s /etc/alternatives/g++ /usr/bin/g++
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-13 50
sudo update-alternatives --set g++ /usr/bin/g++-13
```

### Verify the GCC and G++ versions
```bash
gcc --version
g++ --version
```

### Revert to the previous GCC and G++ versions if needed
```bash
sudo rm /usr/bin/gcc
sudo mv /usr/bin/gcc.bak /usr/bin/gcc
sudo rm /usr/bin/g++
sudo mv /usr/bin/g++.bak /usr/bin/g++

## CUDA Installation

### Install development tools required for building software
```bash
sudo dnf5 group install "development-tools" -y
```

### Install additional tools required for building kernel modules
```bash
sudo dnf install -y make kernel-devel dkms
```

### Download and install the CUDA 12.6.3 installer
```bash
wget https://developer.download.nvidia.com/compute/cuda/12.6.3/local_installers/cuda_12.6.3_560.35.05_linux.run
sudo sh cuda_12.6.3_560.35.05_linux.run
```

### Update environment variables for CUDA
```bash
export PATH=/usr/local/cuda-12.6/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-12.6/lib64:$LD_LIBRARY_PATH
```

### Apply changes to the shell environment
For Zsh users:
```bash
source ~/.zshrc
```
For Bash users:
```bash
source ~/.bashrc
```

### Verify CUDA installation
```bash
nvcc --version
```

---

## CUDNN Installation

### Download CUDNN archive for CUDA 12
```bash
wget https://developer.download.nvidia.com/compute/cudnn/redist/cudnn/linux-x86_64/cudnn-linux-x86_64-9.6.0.74_cuda12-archive.tar.xz
```

### Extract the CUDNN archive
```bash
tar -xvf cudnn-linux-x86_64-9.6.0.74_cuda12-archive.tar.xz
```

### Copy CUDNN files to the CUDA directory
```bash
sudo cp cudnn-*-archive/include/* /usr/local/cuda-12.6/include/
sudo cp cudnn-*-archive/lib/* /usr/local/cuda-12.6/lib64/
```

### Set proper permissions for the CUDNN files
```bash
sudo chmod a+r /usr/local/cuda-12.6/include/cudnn*.h /usr/local/cuda-12.6/lib64/libcudnn*
```

### Verify the CUDNN installation
```bash
cat /usr/local/cuda-12.6/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```

---

## TensorRT Installation

### Download TensorRT archive for CUDA 12.6
```bash
wget https://developer.nvidia.com/downloads/compute/machine-learning/tensorrt/10.7.0/tars/TensorRT-10.7.0.23.Linux.x86_64-gnu.cuda-12.6.tar.gz
```

### Extract the TensorRT archive
```bash
tar -xvf TensorRT-10.7.0.23.Linux.x86_64-gnu.cuda-12.6.tar.gz
```

### Copy TensorRT files to the CUDA directory
```bash
sudo cp TensorRT-10.7.0.23/include/* /usr/local/cuda-12.6/include/
sudo cp -r TensorRT-10.7.0.23/lib/* /usr/local/cuda-12.6/lib64/
```

### Set proper permissions for the TensorRT files
```bash
sudo chmod a+r /usr/local/cuda-12.6/include/Nv*.h /usr/local/cuda-12.6/lib64/libnvinfer*
```

### Verify TensorRT installation
```bash
ls /usr/local/cuda-12.6/lib64 | grep nvinfer
