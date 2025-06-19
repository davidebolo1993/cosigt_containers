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
    libhts-dev \
    wget \
    ca-certificates \
    pkg-config \
    libjemalloc-dev \
    git \
    && rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

# Install
RUN wget https://github.com/waveygang/wfmash/releases/download/v0.14.0/wfmash-v0.14.0.tar.gz \
	&& tar -xvzf wfmash-v0.14.0.tar.gz \
	&& rm wfmash-v0.14.0.tar.gz \
	&& cd wfmash-v0.14.0 \
	&& cmake -H. -Bbuild \
	&& cmake --build build -- -j 8

# Set env path
ENV PATH /opt/wfmash-v0.14.0/build/bin:$PATH

#we need bgzip

RUN git clone --depth 1 --branch 1.22 https://github.com/samtools/htslib.git \
    && cd htslib \
    && make bgzip \
    && cp bgzip /usr/local/bin/ \
    && cd .. && rm -rf htslib

# Default command
CMD ["wfmash"]

