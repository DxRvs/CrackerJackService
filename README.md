# CrackerJack service
It is instruction for bulild and use crackerjack (https://github.com/ctxis/crackerjack.git) in docker container with support GPU (Nvidia).
## Build docker image
<pre>
docker build . -t hweb --network=host
</pre>
### Export docker image
<pre>
docker save --output="hweb_latest.tar" hweb:latest
</pre>
### Import docker image
<pre>
docker load -i ./hweb_latest.tar
</pre>
### Run docker container
<pre>
docker run -d -p 8080:5000 -v ./HWEB:/root/HWEB  hweb:latest
</pre>
## Configuration for service
<pre>
/usr/bin/hashcat
/root/HWEB
/root/HWEB/rule
/root/HWEB/hashfile
/root/HWEB/wordlists
Use --force
</pre>

## Configure CUDA
<pre>
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
</pre>
### For offline
<pre>
apt-get download $(apt-cache depends --recurse --no-recommends --no-suggests --no-conflicts --no-breaks --no-replaces --no-enhances nvidia-docker2 | grep "^\w" | sort -u | grep -v i386)
</pre>
### Run with gpu support
<pre>
docker run -d --gpus all -p 8080:5000 -v ./HWEB:/root/HWEB  hweb:latest
</pre>
### NOTE
##### 1. Base docker image should be the same as the host OS [nvidia/cuda:11.5.1-runtime-ubuntu18.04]
##### 2. For use CUDA change install driver in docker file. Current driver like xserver-xorg-video-nvidia-470 
