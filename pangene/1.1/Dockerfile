# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    cmake \
    autoconf \
    g++ \
    git \
    zlib1g-dev \
    wget \
    make \
    ca-certificates \
    pkg-config \
    libncurses5-dev \
    libncursesw5-dev \
    libbz2-dev \
    liblzma-dev \
    curl \
    libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

# Install pangene
RUN wget https://github.com/lh3/pangene/releases/download/v1.1/pangene-1.1-bin.tar.bz2 \
	&& tar -xjvf pangene-1.1-bin.tar.bz2 \
	&& rm pangene-1.1-bin.tar.bz2

# Set env path
ENV PATH /opt/pangene-1.1-bin/bin_x64-linux:$PATH
ENV PATH /opt/pangene-1.1-bin/scripts:$PATH

# Install htslib
RUN wget https://github.com/samtools/htslib/releases/download/1.22/htslib-1.22.tar.bz2 \
	&& tar -jxvf htslib-1.22.tar.bz2 \
	&& rm htslib-1.22.tar.bz2 \
	&& cd htslib-1.22 \
	&& ./configure \
	&& make \
	&& make install

# Default command
CMD ["pangene"]
