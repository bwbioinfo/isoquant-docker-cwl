# Use the official Ubuntu base image
FROM ghcr.io/bwbioinfo/samtools-docker-cwl:latest

# Set the maintainer label
LABEL maintainer="bwbioinfo"

# Update the package list and install basic packages
RUN yum update -y
RUN yum install -y \
    git \
    wget \
    gcc \
    make \
    zlib-devel \
    bzip2 \
    bzip2-devel \
    xz \
    xz-devel \
    ncurses-devel \
    python3-devel \
    python3-pip \
    g++

# Install minimap2
WORKDIR /opt
RUN git clone https://github.com/lh3/minimap2
WORKDIR /opt/minimap2
RUN make

# set the working directory
WORKDIR /opt
RUN git clone https://github.com/ablab/IsoQuant.git
WORKDIR /opt/IsoQuant
RUN git checkout latest

RUN pip install -r requirements.txt

# docker build -t isoquant-test -f docker/dockerfile .

# Clean up the package lists to reduce the image size
RUN yum clean all && \
    rm -rf /var/cache/yum && \
    rm -rf /tmp/*

# Remove git repositories to reduce image size
WORKDIR /opt
RUN rm -rf /opt/minimap2/.git /opt/IsoQuant/.git

ENV PATH="/opt/minimap2:${PATH}"
ENV PATH="/opt/IsoQuant:${PATH}"

CMD ["isoquant.py"]