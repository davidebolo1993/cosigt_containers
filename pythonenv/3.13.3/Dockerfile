# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    ca-certificates && \
    rm -rf /var/lib/apt/lists/*

#Build python conda env
RUN curl -LO https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
RUN bash Miniconda3-latest-Linux-x86_64.sh -p /miniconda -b && \
    rm Miniconda3-latest-Linux-x86_64.sh
ENV PATH=/miniconda/bin:${PATH}
RUN conda update -y conda && \
        conda create -y -n pythonenv -c conda-forge -c bioconda \
	python=3.13.1 \
	pandas=2.2.3 \
        numpy=2.2.1
RUN echo "source activate pythonenv" > ~/.bashrc
ENV PATH /miniconda/envs/pythonenv/bin:$PATH
