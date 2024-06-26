ARG PYTORCH="2.0.1"
ARG CUDA="11.7"
ARG CUDNN="8"

ARG DOCKER_REGISTRY=public.aml-repo.cms.waikato.ac.nz:443/
FROM ${DOCKER_REGISTRY}pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

# update NVIDIA repo key
# https://developer.nvidia.com/blog/updating-the-cuda-linux-gpg-repository-key/
ARG distro=ubuntu2004
ARG arch=x86_64
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/$distro/$arch/3bf863cc.pub

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && \
    apt-get install -y libglib2.0-0 libsm6 libxrender-dev libxext6 libgl1-mesa-glx python3-dev python3-pip git wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /opt
ENV FORCE_CUDA="1"
ENV TORCH_CUDA_ARCH_LIST="6.0 6.1 7.0+PTX"
ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"
ENV CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"
RUN pip install --no-cache-dir \
    openmim \
    "diffusers==0.23.0" \
    "gradio==4.27.0" \
    "xformers==0.0.22" \
    accelerate
RUN mim install mmcv==2.0.1
RUN git clone https://github.com/open-mmlab/mmagic.git && \
    cd mmagic && \
    git checkout tags/v1.2.0 && \
    pip install --no-cache-dir -r requirements.txt  && \
    pip install --no-cache-dir -e .
RUN pip install --no-cache-dir \
    "opex==0.0.3" \
    python-image-complete \
    "simple-file-poller>=0.0.9" \
    redis \
    "redis-docker-harness==0.0.4"

COPY bash.bashrc /etc/bash.bashrc
COPY mmagic_* /usr/local/bin/

ENV PYTHONPATH=/opt/mmagic
