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

RUN apt-get -y clean all \
	&& apt-get -y purge \
	&& rm -rf /var/cache \
	&& rm -rf /var/lib/apt/lists/*

# Install Go 1.19.1 instead of using the repository version
RUN wget https://go.dev/dl/go1.19.1.linux-amd64.tar.gz \
    && tar -C /usr/local -xzf go1.19.1.linux-amd64.tar.gz \
    && rm go1.19.1.linux-amd64.tar.gz
ENV PATH $PATH:/usr/local/go/bin
RUN go version

# Set workdir
WORKDIR /opt

# Install meryl
RUN wget https://github.com/marbl/meryl/releases/download/v1.4.1/meryl-1.4.1.Linux-amd64.tar.xz \
	&& tar -xf meryl-1.4.1.Linux-amd64.tar.xz \
	&& rm meryl-1.4.1.Linux-amd64.tar.xz

ENV PATH /opt/meryl-1.4.1/bin:$PATH

# Install kfilt
RUN git clone https://github.com/davidebolo1993/kfilt \
	&& cd kfilt \
	&& git checkout fd7fc868a6927095b277dc4d679e5e8cd51e48dd \
	&& go mod init kfilt \
	&& go mod tidy \
	&& go build kfilt \
	&& cp kfilt ../kfilt-tmp \
	&& cd .. \
	&& rm -rf kfilt \
	&& mv kfilt-tmp kfilt

# Set path
ENV PATH /opt:$PATH

# Default command
CMD ["kfilt"]
