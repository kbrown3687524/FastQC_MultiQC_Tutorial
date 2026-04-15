# Version arguments
# ARG variables only persist during build time
ARG FASTQC_VERSION="0.11.9"

FROM mambaorg/micromamba:2.5.0-ubuntu22.04 AS app

ARG FASTQC_VERSION

# Example: setting an environment variable or running a command with it
RUN echo "Successfully pulled FastQC version: ${FASTQC_VERSION}"

# Switch to root only when necessary for system-level labels/setup
USER root
WORKDIR /

LABEL base.image="biocontainers/fastqc:v0.11.9_cv8"
LABEL dockerfile.version="1"
LABEL software="FastQC"
LABEL software.version="${FASTQC_VERSION}"
LABEL description="FastQC is a quality control tool for high throughput sequence data."
LABEL website="https://www.bioinformatics.babraham.ac.uk/projects/fastqc/"  
LABEL license="https://www.bioinformatics.babraham.ac.uk/projects/fastqc/license.html"
LABEL maintainer="Your Name"
LABEL maintainer.email=""

# Set the environment path using DOUBLE quotes and proper spacing
ENV PATH="/opt/conda/envs/fastqc/bin:${PATH}" \
    LC_ALL=C.UTF-8

# Switch back to the default mamba user to safely install packages
USER $MAMBA_USER

# Install FastQC using micromamba (not mamba)
RUN micromamba create -n fastqc -y -c bioconda -c conda-forge fastqc=${FASTQC_VERSION} && \
    micromamba clean -a -y

# Install MultiQC in the same environment used by the tutorial
RUN micromamba install -n fastqc -y -c bioconda -c conda-forge multiqc && \
    micromamba clean -a -y

# Switch to root to modify system directory permissions
USER root

# Set the working directory to /data
WORKDIR /data

# make directory editable
RUN chmod -R a+rwX /opt/conda/envs/fastqc && \
    chown -R $MAMBA_USER:$MAMBA_USER /data

# default command to run when container starts
CMD ["fastqc", "--help"]

# Devcontainer target used for Codespaces interactive sessions.
FROM app AS dev

CMD ["/bin/bash"]

# new base for testing
FROM app AS test

# Switch to root for apt-get installations
USER root

# install dependencies; cleanup apt garbage
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    procps \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Switch back to mamba user for running user-level tests and downloads
USER $MAMBA_USER

# Download the raw data files from GitHub and run FastQC on the test data
RUN mkdir -p data && \
    wget -qO data/HG004_R1.fastq.gz "https://raw.githubusercontent.com/kbrown3687524/FastQC_MultiQC_Tutorial/main/data/HG004_R1.fastq.gz" && \
    wget -qO data/HG004_R2.fastq.gz "https://raw.githubusercontent.com/kbrown3687524/FastQC_MultiQC_Tutorial/main/data/HG004_R2.fastq.gz" && \
    fastqc data/HG004_R1.fastq.gz -o data && \
    fastqc data/HG004_R2.fastq.gz -o data && \
    ls -l data

RUN test -f data/HG004_R1_fastqc.html && \
    test -f data/HG004_R2_fastqc.html && \
    echo "Success! Your HG004 reports were generated."

# list installed packages and versions for testing purposes
RUN micromamba list -n fastqc