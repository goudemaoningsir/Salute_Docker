FROM debian:bullseye-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    gnupg \
    ca-certificates \
    wget \
    && rm -rf /var/lib/apt/lists/*


ADD https://developer.download.nvidia.com/compute/cuda/repos/debian11/x86_64/cuda-keyring_1.1-1_all.deb cuda-keyring.deb
RUN dpkg -i cuda-keyring.deb && rm cuda-keyring.deb

RUN apt-get update && apt-get install -y --no-install-recommends \
    cuda-toolkit-12-4 \
    && rm -rf /var/lib/apt/lists/*

ENV PATH=/usr/local/cuda-12.4/bin:${PATH}
ENV LD_LIBRARY_PATH=/usr/local/cuda-12.4/lib64:${LD_LIBRARY_PATH}


FROM python:3.12.2-slim

COPY --from=0 /usr/local/cuda-12.4 /usr/local/cuda-12.4

ENV PATH=/usr/local/cuda-12.4/bin:${PATH}
ENV LD_LIBRARY_PATH=/usr/local/cuda-12.4/lib64:${LD_LIBRARY_PATH}

WORKDIR /app

RUN python3.10 -m pip config set global.index-url https://mirror.baidu.com/pypi/simple

RUN apt-get update && \
    apt-get install -y --no-install-recommends openssh-server  curl vim  wget sudo  git unzip tar

RUN echo 'root:123456' | chpasswd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config