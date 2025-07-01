# Use the official Ubuntu image as a base
FROM ubuntu:24.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHON="python=3.9"
ENV BOOST_VERSION=1.82.0
ENV COMPILER=gxx_linux-64
ENV NUMBER_OF_CORES=nproc
ENV PYTHON_NAME=python39
ENV CC=gcc-13
ENV CXX=g++-13


# Install dependencies
#RUN add-apt-repository ppa:ubuntu-toolchain-r/test && \
RUN apt-get update && \
    apt-get install --only-upgrade -y libstdc++6 && \
    apt-get install -y \
    build-essential \
    cmake \
    git \
    wget \
    python3 \
    python3-pip \
    python3-dev \
    libboost-all-dev \
    libeigen3-dev \
    libsqlite3-dev \
    libpng-dev \
    libfreetype6-dev \
    libz-dev \
    pkg-config \
    g++ \
    make \
    libgl1-mesa-dev \
    mesa-common-dev \
    libc6-dbg  \
    && rm -rf /var/lib/apt/lists/*

# Install RDKit dependencies
RUN apt-get update && apt-get install -y \
    libboost-filesystem-dev \
    libboost-iostreams-dev \
    libboost-program-options-dev \
    libboost-regex-dev \
    libboost-system-dev \
    libboost-thread-dev \
    python3-numpy \
    libxml2-dev \
    libxrender-dev \
    libpng-dev \
    libfreetype6-dev \
    libeigen3-dev \
    && rm -rf /var/lib/apt/lists/*


RUN mkdir -p ~/miniconda3 && wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh && bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3 && rm ~/miniconda3/miniconda.sh

RUN chown -R $(whoami) ~/miniconda3/bin/conda

RUN ~/miniconda3/bin/conda config --set always_yes yes --set changeps1 no && \
    ~/miniconda3/bin/conda config --set channel_priority strict && \
    ~/miniconda3/bin/conda config --add channels conda-forge && \
    ~/miniconda3/bin/conda config --set solver libmamba && \
    ~/miniconda3/bin/conda create --name rdkit_build -c conda-forge ${PYTHON} CMAKE \
                        boost-cpp=${BOOST_VERSION} boost=${BOOST_VERSION} \
                        numpy pillow eigen pandas=2.0 matplotlib-base=3.7  \
                        qt=5 cairo pytest nbval \
                        ipykernel>=6.0 libstdcxx-ng \
                        valgrind ccache

ENV RDBASE=/usr/app/rdkit
ENV PYTHONPATH=${RDBASE}:${PYTHONPATH}
ENV LD_LIBRARY_PATH=${RDBASE}/lib:/root/miniconda3/envs/rdkit_build/lib:${LD_LIBRARY_PATH}
ENV QT_QPA_PLATFORM='offscreen'
ENV CONDA_PREFIX=/root/miniconda3/envs/rdkit_build


# Set up the work directory
WORKDIR /usr/app/rdkit

COPY . /usr/app/rdkit
