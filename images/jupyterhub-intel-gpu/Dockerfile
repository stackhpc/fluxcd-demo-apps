FROM quay.io/jupyter/pytorch-notebook:2025-01-28

#####
# Add Intel GPU components
#####

USER root

ENV LANG=C.UTF-8

ARG DEBIAN_FRONTEND=noninteractive

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        apt-utils \
        build-essential \
        ca-certificates \
        clinfo \
        curl \
        git \
        gnupg2 \
        gpg-agent \
        rsync \
        sudo \
        unzip \
        wget && \
    apt-get clean && \
    rm -rf  /var/lib/apt/lists/*

RUN wget -qO - https://repositories.intel.com/gpu/intel-graphics.key | \
    gpg --dearmor --yes --output /usr/share/keyrings/intel-graphics.gpg
RUN echo "deb [arch=amd64 signed-by=/usr/share/keyrings/intel-graphics.gpg] https://repositories.intel.com/gpu/ubuntu jammy unified" | \
    tee /etc/apt/sources.list.d/intel-gpu-jammy.list &&\
    apt update

ARG ICD_VER=24.22.29735.27-914~22.04
ARG LEVEL_ZERO_GPU_VER=1.3.29735.27-914~22.04
ARG LEVEL_ZERO_VER=1.17.6-914~22.04
ARG LEVEL_ZERO_DEV_VER=1.17.6-914~22.04

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    intel-opencl-icd=${ICD_VER} \
    intel-level-zero-gpu=${LEVEL_ZERO_GPU_VER} \
    libze1=${LEVEL_ZERO_VER} \
    libze-dev=${LEVEL_ZERO_DEV_VER} && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB \
   | gpg --dearmor | tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null && \
   echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" \
   | tee /etc/apt/sources.list.d/oneAPI.list

ARG DPCPP_VER=2024.2.1-1079
ARG MKL_VER=2024.2.1-103
ARG CCL_VER=2021.13.1-31

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        intel-oneapi-runtime-dpcpp-cpp=${DPCPP_VER} \
        intel-oneapi-runtime-mkl=${MKL_VER} \
        intel-oneapi-runtime-ccl=${CCL_VER} && \
    apt-get clean && \
    rm -rf  /var/lib/apt/lists/*

RUN groupadd -g 110 render

#####
# User-level python components
#####

USER ${NB_USER}

RUN pip3 install -U torch torchvision torchaudio \
  --index-url https://download.pytorch.org/whl/test/xpu
