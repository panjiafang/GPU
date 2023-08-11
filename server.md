Ubuntu 20.04 
12核 44GB 100Mbps


## 安装cuda

* 各版本cuda下载地址：https://developer.nvidia.com/cuda-toolkit-archive
   
Network
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda
```
*腾讯云安装的有点慢啊*

Local(没有尝试)
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.2.1/local_installers/cuda-repo-ubuntu2004-12-2-local_12.2.1-535.86.10-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-12-2-local_12.2.1-535.86.10-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2004-12-2-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda
```

#### 上面安装的是最新版本，也可以安装11.7.1版本
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring*.deb
sudo apt-get update
sudo apt-get -y install cuda
```

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.7.1/local_installers/cuda-repo-ubuntu2004-11-7-local_11.7.1-515.65.01-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-11-7-local_11.7.1-515.65.01-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2004-11-7-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda
```


## 安装Pytorch

```bash
pip3 install torch torchvision torchaudio
```

## 安装ModelScope

```bash
pip install modelscope
```

## 安装Transformers

```bash
pip install transformers -U
```

## 环境变量设置
```bash
WARNING: The script modelscope is installed in '/home/ubuntu/.local/bin' which is not on PATH.
Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location. 
```

通过`PIP`安装会收到这个WARNING，需要将`/home/ubuntu/.local/bin`加入到PATH中

在.bashrc中添加

`export PATH=$PATH:/home/ubuntu/.local/bin`

保存之后

`source .bashrc`
