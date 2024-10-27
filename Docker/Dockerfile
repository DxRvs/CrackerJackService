FROM ubuntu:24.04
LABEL MAINTAINER="df000f5@gmail.com"
EXPOSE 5000
ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
WORKDIR /root
RUN apt-get update && apt-get upgrade -y && DEBIAN_FRONTEND=noninteractive apt-get install -y git wget screen python3 python3-venv python3-pip sqlite3 ocl-icd-libopencl1 ocl-icd-opencl-dev hashcat && apt-get clean && rm -rf /var/lib/apt/lists/*
RUN wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB | gpg --dearmor | tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null
RUN echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" | tee /etc/apt/sources.list.d/oneAPI.list
RUN apt-get update && DEBIAN_FRONTEND=noninteractive  apt-get install -y intel-oneapi-runtime-libs && apt-get clean 
RUN git clone https://github.com/ctxis/crackerjack.git
RUN python3 -m venv /root/.venv && . /root/.venv/bin/activate && pip3 install --upgrade pip && pip3 install -r crackerjack/requirements.txt
WORKDIR /root/crackerjack
ENV PATH="/root/.venv/bin:$PATH"
ENV FLASK_ENV=production
ENV FLASK_APP=app 
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8 
RUN flask db init && flask db migrate && flask db upgrade
ENTRYPOINT ["flask","run","--host=0.0.0.0"]
