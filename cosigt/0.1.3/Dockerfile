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
    pkg-config

RUN add-apt-repository ppa:longsleep/golang-backports

RUN apt-get -y install golang-go \
	&& apt-get -y clean all \
	&& apt-get -y purge \
	&& rm -rf /var/cache \
	&& rm -rf /var/lib/apt/lists/*

# Set workdir
WORKDIR /opt

# Install cosigt
RUN git clone https://github.com/davidebolo1993/cosigt.git \
	&& cd cosigt \
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
