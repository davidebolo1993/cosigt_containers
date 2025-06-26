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
RUN git clone https://github.com/davidebolo1993/cosigt.git \
	&& cd cosigt \
	&& git checkout 885e6e08e773f8d8497fca2a202980298ace42e6 \
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
