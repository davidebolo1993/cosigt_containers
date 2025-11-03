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

# Install cosigt
RUN git clone https://github.com/davidebolo1993/kfilt \
	&& cd kfilt \
	&& git checkout e9336955c92898f9c413d1aa03e9f10bb867f77f \
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
