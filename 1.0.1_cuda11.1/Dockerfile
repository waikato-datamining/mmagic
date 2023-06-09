ARG PYTORCH="1.9.1"
ARG CUDA="11.1"
ARG CUDNN="8"

ARG DOCKER_REGISTRY=public.aml-repo.cms.waikato.ac.nz:443/
FROM ${DOCKER_REGISTRY}pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

# update NVIDIA repo key
# https://developer.nvidia.com/blog/updating-the-cuda-linux-gpg-repository-key/
ARG distro=ubuntu1804
ARG arch=x86_64
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/$distro/$arch/3bf863cc.pub

# https://developer.nvidia.com/blog/updating-the-cuda-linux-gpg-repository-key/
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && \
    apt-get -y install \
          ninja-build \
          libgl1-mesa-glx \
          libglib2.0-0 \
          libsm6 \
          libxrender-dev \
          libxext6 \
          python3-setuptools \
          git \
          python3-opencv \
          pkg-config && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /opt
ENV FORCE_CUDA="1"
RUN pip install openmim
RUN mim install mmcv==2.0.0rc4
RUN git clone https://github.com/open-mmlab/mmagic.git && \
    cd mmagic && \
    git reset --hard b058b4111a355ef761a99ecab7a632fa84f2435b && \
    pip install --no-cache-dir -r requirements.txt  && \
    pip install --no-cache-dir -e .
RUN pip install --no-cache-dir "opex==0.0.2" && \
    pip install --no-cache-dir python-image-complete && \
    pip install --no-cache-dir "simple-file-poller>=0.0.9" && \
    pip install --no-cache-dir redis && \
    pip install --no-cache-dir "redis-docker-harness==0.0.1"
RUN pip install --no-cache-dir accelerate

COPY bash.bashrc /etc/bash.bashrc

ENV PYTHONPATH=/opt/mmagic
