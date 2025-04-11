# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    wget \
    ca-certificates \
    software-properties-common \
    git \
    pkg-config \
    snapd

RUN apt-get -y clean all \
	&& apt-get -y purge \
	&& rm -rf /var/cache \
	&& rm -rf /var/lib/apt/lists/*

#install go
RUN snap install go --channel=1.19.1/stable --classic

# Set workdir
WORKDIR /opt

# Install cosigt
RUN git clone https://github.com/davidebolo1993/cosigt.git \
	&& cd cosigt \
	&& git checkout 97fd01f1147cfc999800531a58cb9f43cf451703 \
	&& go mod init cosigt \
	&& go mod tidy \
	&& go build cosigt \
	&& cp cosigt ../cosigt-tmp \
	&& cd .. \
	&& rm -rf cosigt \
	&& mv cosigt-tmp cosigt

# Set path
ENV PATH /opt:$PATH

# Default command
CMD ["cosigt"]

