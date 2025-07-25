# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    wget \
    cmake \
    autoconf \
    g++ \
    zlib1g-dev \
    libncurses5-dev \
    libncursesw5-dev \
    libbz2-dev \
    liblzma-dev \
    curl \
    libcurl4-openssl-dev \
    ca-certificates && \
    rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

# Install bedtools
RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.31.0/bedtools.static \
	&& chmod +x bedtools.static \
	&& mv bedtools.static bedtools

# add htslib
RUN wget https://github.com/samtools/htslib/releases/download/1.22/htslib-1.22.tar.bz2 \
	&& tar -jxvf htslib-1.22.tar.bz2 \
	&& rm htslib-1.22.tar.bz2 \
	&& cd htslib-1.22 \
	&& ./configure \
	&& make \
	&& make install

# add samtools
RUN wget https://github.com/samtools/samtools/releases/download/1.22/samtools-1.22.tar.bz2 \
	&& tar -jxvf samtools-1.22.tar.bz2 \
	&& rm samtools-1.22.tar.bz2 \
	&& cd samtools-1.22 \
	&& ./configure \
	&& make \
	&& make install

#Set path
ENV PATH /opt:$PATH

# Default command
CMD ["bedtools"]
