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
    htslib-tools \
    wget \
    ca-certificates \
    pkg-config \
    libjemalloc-dev \
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

# Default command
CMD ["wfmash"]

