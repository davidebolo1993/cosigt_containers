# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    cmake \
    zlib1g-dev \
    libgsl-dev \
    liblzma-dev \
    libbz2-dev \
    wget \
    ca-certificates \
    pkg-config \
    curl \
    libcurl4-openssl-dev \
    libjemalloc-dev \
    && rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

# htslib
RUN wget https://github.com/samtools/htslib/releases/download/1.22/htslib-1.22.tar.bz2 \
        && tar -jxvf htslib-1.22.tar.bz2 \
        && rm htslib-1.22.tar.bz2 \
        && cd htslib-1.22 \
        && ./configure \
        && make \
        && make install

# Install
RUN wget https://github.com/waveygang/wfmash/releases/download/v0.14.0/wfmash-v0.14.0.tar.gz \
	&& tar -xvzf wfmash-v0.14.0.tar.gz \
	&& rm wfmash-v0.14.0.tar.gz \
	&& cd wfmash-v0.14.0 \
	&& cmake -H. -Bbuild \
	&& cmake --build build -- -j 8

# Set env path
ENV PATH /opt/wfmash-v0.14.0/build/bin:$PATH

# Default command
CMD ["wfmash"]

