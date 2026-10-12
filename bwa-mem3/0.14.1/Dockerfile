# bwa-mem3 publishes no prebuilt binaries, and building it from source needs a
# newer compiler than bullseye ships, plus libsais and OpenMP. The bioconda build
# is installed instead, which also means SOFTWARE=apptainer and SOFTWARE=conda run
# the same binaries.
#
# micromamba rather than Miniconda: it is pinned, and it never touches
# repo.anaconda.com, so there are no channel Terms of Service to accept.
FROM debian:bullseye-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    bzip2 \
    && rm -rf /var/lib/apt/lists/*

# Pinned micromamba
RUN curl -Ls https://micro.mamba.pm/api/micromamba/linux-64/2.3.3 \
    | tar -xj -C /usr/local bin/micromamba

ENV MAMBA_ROOT_PREFIX=/opt/micromamba

# bwa-mem3 0.14.1 links htslib 1.24, so samtools has to be on the same htslib.
# The cosigt rule pipes bwa-mem3 straight into samtools sort and view, so both
# have to be in this image.
RUN micromamba create -y -p /opt/bwa-mem3 -c conda-forge -c bioconda \
        bwa-mem3=0.14.1 \
        samtools=1.24 \
    && micromamba clean -a -y

ENV PATH=/opt/bwa-mem3/bin:$PATH

CMD ["bwa-mem3"]
