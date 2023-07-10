# Setup PointGPT

Personal reminder notes for setting up in Ubuntu 20.04 (GCloud)

## Launch GPU instance

```bash
gcloud beta compute instances create pointgpt \
--zone=us-central1-c \
--machine-type=n1-highmem-8 \
--accelerator type=nvidia-tesla-v100 \
--boot-disk-size=50GB \
--boot-disk-type=pd-ssd \
--image-project=ubuntu-os-cloud \
--image-family=ubuntu-2004-lts \
--maintenance-policy=TERMINATE
```

## Tunneling

```bash
gcloud compute ssh pointgpt -- -L 2222:localhost:22
```

```bash
scp -P 2222 local.txt user@localhost:/home/user
ssh -p 2222 user@localhost
```

## Clone repo
```bash
git clone git@github.com:tomas-aftalion/PointGPT.git
```

## Python3
```bash
sudo apt update
sudo apt install -y python3 python3-pip
sudo ln -s /usr/bin/python3 /usr/bin/python
```

## Install CUDA drivers
```bash
curl https://raw.githubusercontent.com/GoogleCloudPlatform/compute-gpu-installation/main/linux/install_gpu_driver.py --output install_gpu_driver.py
sudo python3 install_gpu_driver.py
```
Check installation:

```bash
nvidia-smi
```

## Install CUDA

Use Cuda 11.7 from [toolkit archive](https://developer.nvidia.com/cuda-toolkit-archive).
```bash
wget https://developer.download.nvidia.com/compute/cuda/11.7.1/local_installers/cuda_11.7.1_515.65.01_linux.run
sudo sh cuda_11.7.1_515.65.01_linux.run
```
Update environment
```bash
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```
Check nvcc
```bash
nvcc -V
```

## Install Python packages
Torch 2.0 has cuda 11.7 by default

```bash
pip install torch==2.0 torchstat
pip install -r requirements.txt
```

```bash
pip install "git+https://github.com/erikwijmans/Pointnet2_PyTorch.git#egg=pointnet2_ops&subdirectory=pointnet2_ops_lib"
pip install --upgrade https://github.com/unlimblue/KNN_CUDA/releases/download/0.2/KNN_CUDA-0.2-py3-none-any.whl
```

## Install Ninja

```bash
pip install ninja
sudo apt-get install ninja-build
```

## Build extensions

```bash
cd ./extensions/chamfer_dist
python setup.py install --user
cd ./extensions/emd
python setup.py install --user
```

## Install gcloud cli
```bash
sudo apt-get install apt-transport-https ca-certificates gnupg
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
sudo apt-get update && sudo apt-get install google-cloud-cli
```

```bash
gcloud auth activate-service-account --key-file=/path/to/key-file.json
```

## Download data
```bash
gcloud storage cp -r gs://tomasaftalion-pointbert/data .
```

## Run
```bash
```

