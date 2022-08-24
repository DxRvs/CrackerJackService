FROM nvidia/cuda:11.5.1-runtime-ubuntu18.04
LABEL MAINTAINER="df000f5@gmail.com"
EXPOSE 5000
ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
WORKDIR /root
RUN apt-get update && apt-get upgrade -y
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y git screen python3 python3-venv python3-pip sqlite3 ocl-icd-libopencl1 ocl-icd-opencl-dev nvidia-cuda-toolkit xserver-xorg-video-nvidia-470 hashcat && rm -rf /var/lib/apt/lists/*
RUN pip3 install --upgrade pip
RUN git clone https://github.com/ctxis/crackerjack
RUN pip3 install -r crackerjack/requirements.txt
WORKDIR /root/crackerjack
ENV FLASK_ENV=production
ENV FLASK_APP=app 
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8 
RUN flask db init && flask db migrate && flask db upgrade
ENTRYPOINT ["flask","run","--host=0.0.0.0"]
