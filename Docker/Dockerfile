#!/bin/bash

# Paperspace Dockerfile for Gradient base image
# Paperspace image is located in Dockerhub registry: technicqllizqrd/paperspace-rag-tools-rolling


# ==================================================================
# Module list
# ------------------------------------------------------------------
# python                        3.11.6           (apt)
# pip3                          23.3.1           (apt)
# cuda toolkit                  12.0.0           (apt)
# cudnn                         8.9.7            (apt)
# torch                         latest           (pip)
# torchvision                   0.16.1           (pip)
# torchaudio                    2.1.1            (pip)
# tensorflow                    removed          (pip)
# transformers                  latest           (pip)
# datasets                      2.14.5           (pip)
# peft                          0.6.2            (pip)
# tokenizers                    0.13.3           (pip)
# accelerate                    0.24.1           (pip)
# diffusers                     0.21.4           (pip)
# safetensors                   0.4.0            (pip)
# jupyterlab                    3.6.5            (pip)
# bitsandbytes                  0.41.2           (pip)
# cloudpickle                   2.2.1            (pip)
# scikit-image                  0.21.0           (pip)
# scikit-learn                  1.3.0            (pip)
# matplotlib                    3.7.3            (pip)
# ipywidgets                    8.1.1            (pip)
# cython                        3.0.2            (pip)
# tqdm                          4.66.1           (pip)
# gdown                         4.7.1            (pip)
# xgboost                       1.7.6            (pip)
# pillow                        9.5.0            (pip)
# seaborn                       0.12.2           (pip)
# sqlalchemy                    2.0.21           (pip)
# spacy                         3.6.1            (pip)
# nltk                          3.8.1            (pip)
# boto3                         1.28.51          (pip)
# tabulate                      0.9.0            (pip)
# future                        0.18.3       cup    (pip)
# jsonify                       0.5              (pip)
# opencv-python                 4.8.0.76         (pip)
# pyyaml                        5.4.1            (pip)
# sentence-transformers         2.2.2            (pip)
# wandb                         0.15.10          (pip)
# deepspeed                     0.10.3           (pip)
# cupy-cuda12x                  12.2.0           (pip)
# timm                          0.9.7            (pip)
# omegaconf                     2.3.0            (pip)
# scipy                         1.11.2           (pip)
# gradient                      2.0.6            (pip) 
# attrs                         23.1.0           (pip)
# default-jre                   latest           (apt)
# default-jdk                   latest           (apt)
# nodejs                        20.x latest      (apt)
# jupyter_contrib_nbextensions  0.7.0            (pip)
# jupyterlab-git                0.43.0           (pip)


# ==================================================================
# Initial setup
# ------------------------------------------------------------------

    # Ubuntu 24.04 LTS as base image, more up-to-date even if CuPy is not tested against #livehard-diehard
    FROM ubuntu:latest
    RUN yes| unminimize

    # Set ENV variables
    ENV LANG C.UTF-8
    ENV SHELL=/bin/bash
    ENV DEBIAN_FRONTEND=noninteractive

    ENV APT_INSTALL="apt-get install -y --no-install-recommends"
    ENV PIP_INSTALL="python3 -m pip --no-cache-dir install --upgrade"
    ENV GIT_CLONE="git clone --depth 10"


# ==================================================================
# Tools
# ------------------------------------------------------------------

    RUN apt-get update && \
        $APT_INSTALL \
        gcc \
        make \
        pkg-config \
        apt-transport-https \
        build-essential \
        apt-utils \
        ca-certificates \
        wget \
        rsync \
        git \
        vim \
        mlocate \
        libssl-dev \
        curl \
        openssh-client \
        unzip \
        unrar \
        zip \
        awscli \
        csvkit \
        emacs \
        joe \
        jq \
        dialog \
        man-db \
        manpages \
        manpages-dev \
        manpages-posix \
        manpages-posix-dev \
        nano \
        iputils-ping \
        sudo \
        ffmpeg \
        libsm6 \
        libxext6 \
        libboost-all-dev \
        gnupg \
        cifs-utils \
        zlib1g \
        software-properties-common

# ==================================================================
# Git-lfs
# ------------------------------------------------------------------
    
    RUN curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash && \
        $APT_INSTALL git-lfs


# ==================================================================
# Python
# ------------------------------------------------------------------

    #Based on https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa

    # Adding repository for python3.11
    RUN add-apt-repository ppa:deadsnakes/ppa -y && \
        $APT_INSTALL \
        python3.11 \
        python3.11-dev \
        python3.11-venv \
        python3-distutils-extra

    # Add symlink so python and python3 commands use same python3.9 executable
    RUN ln -s /usr/bin/python3.11 /usr/local/bin/python3 && \
        ln -s /usr/bin/python3.11 /usr/local/bin/python    

    # Installing pip
    RUN curl -sS https://bootstrap.pypa.io/get-pip.py | python3
    ENV PATH=$PATH:/root/.local/bin    

# ==================================================================
# Installing CUDA packages (CUDA Toolkit 12.6 and CUDNN 8.9.7)
# ------------------------------------------------------------------
    RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-ubuntu2404.pin && \
        mv cuda-ubuntu2404.pin /etc/apt/preferences.d/cuda-repository-pin-600 && \
        # https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=24.04&target_type=deb_local
        wget https://developer.download.nvidia.com/compute/cuda/12.6.3/local_installers/cuda-repo-ubuntu2404-12-6-local_12.6.3-560.35.05-1_amd64.deb && \
        dpkg -i cuda-repo-ubuntu2404-12-6-local_12.6.3-560.35.05-1_amd64.deb && \
        cp /var/cuda-repo-ubuntu2404-12-6-local/cuda-*-keyring.gpg /usr/share/keyrings/ && \
        apt-get update && \
        $APT_INSTALL cuda && \
        rm cuda-repo-ubuntu2404-12-6-local_12.6.3-560.35.05-1_amd64.deb

    # Installing CUDNN
    RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/3bf863cc.pub && \
        add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/ /" && \
        apt-get update && \
        $APT_INSTALL libcudnn9=libcudnn9-cuda-12_9.6.0.74-1 \
                     libcudnn9-dev=libcudnn9-dev-cuda-12_9.6.0.74-1 \
                     


    ENV PATH=$PATH:/usr/local/cuda/bin
    ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH


# ==================================================================
# PyTorch
# ------------------------------------------------------------------

    # Based on https://pytorch.org/get-started/locally/

    RUN $PIP_INSTALL torch==2.1.1 torchvision==0.16.1 torchaudio==2.1.1 --extra-index-url https://download.pytorch.org/whl/cu121

# ==================================================================
# TensorFlow
# ------------------------------------------------------------------

    # Based on https://www.tensorflow.org/install/pip

    RUN $PIP_INSTALL tensorflow==2.15.0 && \


# ==================================================================
# Hugging Face
# ------------------------------------------------------------------
    
    # Based on https://huggingface.co/docs/transformers/installation
    # Based on https://huggingface.co/docs/datasets/installation

    $PIP_INSTALL transformers \
        datasets \
        peft \
        tokenizers \
        accelerate \
        diffusers \
        safetensors && \


# ==================================================================
# JupyterLab
# ------------------------------------------------------------------

    # Based on https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html#pip

    $PIP_INSTALL jupyterlab


# ==================================================================
# Additional Python Packages
# ------------------------------------------------------------------

    RUN $PIP_INSTALL \
        bitsandbytes \
        cloudpickle \
        scikit-image \
        scikit-learn \
        matplotlib \
        ipywidgets \
        cython \
        tqdm \
        gdown \
        xgboost \
        pillow \
        seaborn \
        sqlalchemy \
        spacy \
        nltk \
        boto3 \
        tabulate \
        future \
        jsonify \
        opencv-python \
        openpyxl \
        pyyaml \
        sentence-transformers \
        wandb \
        deepspeed \
        cupy-cuda12x \
        timm \
        omegaconf \
        scipy \
        gradient
    RUN $PIP_INSTALL attrs


# ==================================================================
# Installing JRE and JDK
# ------------------------------------------------------------------

    RUN $APT_INSTALL \
        default-jre \
        default-jdk


# ==================================================================
# CMake
# ------------------------------------------------------------------

    RUN $GIT_CLONE https://github.com/Kitware/CMake ~/cmake && \
        cd ~/cmake && \
        ./bootstrap && \
        make -j"$(nproc)" install


# ==================================================================
# Node.js and Jupyter Notebook Extensions
# ------------------------------------------------------------------

    RUN curl -sL https://deb.nodesource.com/setup_20.x | bash  && \
        $APT_INSTALL nodejs  && \
        $PIP_INSTALL jupyterlab-git==0.50 && 
                

# ==================================================================
# Startup
# ------------------------------------------------------------------

    EXPOSE 8888 6006

    CMD jupyter lab --allow-root --ip=0.0.0.0 --no-browser --ServerApp.trust_xheaders=True --ServerApp.disable_check_xsrf=False --ServerApp.allow_remote_access=True --ServerApp.allow_origin='*' --ServerApp.allow_credentials=True
