# Getting started with the CrackerJack service
Follow the steps below to bulild and use [crackerjack](https://github.com/ctxis/crackerjack.git) in a docker container supported by GPU (Nvidia). 

## How to build the Docker image
To get started, clone the repository and use Docker to build the Docker image.

<pre>
sudo docker build . -t hweb --network=host
</pre>
### How to export the Docker image

You can save a Docker image to copy to another machine by using the `save` command.
<pre>
sudo docker save --output="hweb_latest.tar" hweb:latest
</pre>

The image will be saved as a tar file.

### How to import the Docker image

To use the image on a new machine you can import the content from an archive.

<pre>
sudo docker load -i ./hweb_latest.tar
</pre>
### How to run the Docker container

Create the HWEB folder to use it together with the crackerjack service. Create folders listed below: hashcat, rule, hashfile, wordlistst, masks.

- hashfile is intended for large hashfiles.
- rule (and masks) is a folder for hashcat rules files.
- wordlists is a folder containing password dictionaries

Start your container using the docker run command.

<pre>
sudo docker run -d -p 8080:5000 -v "$PWD/HWEB":/root/HWEB  hweb:latest
</pre>

## Configuration for the service

Configure the parameters to work with the service according to the picture. 

![image](/screenshots/settings.png)

## CUDA configuration

Use the following comands to set up CUDA.

<pre>
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
</pre>
### Download dependancies for offline installation

<pre>
apt-get download $(apt-cache depends --recurse --no-recommends --no-suggests --no-conflicts --no-breaks --no-replaces --no-enhances nvidia-docker2 | grep "^\w" | sort -u | grep -v i386)
</pre>
### Run with gpu support
<pre>
docker run -d --gpus all -p 8080:5000 -v "$PWD/HWEB":/root/HWEB  hweb:latest
</pre>
### NOTE
##### 1. Base docker image should be the same as the host OS [nvidia/cuda:11.5.1-runtime-ubuntu18.04]
##### 2. To use CUDA change the driver before installation. The currrent driver is xserver-xorg-video-nvidia-470 
