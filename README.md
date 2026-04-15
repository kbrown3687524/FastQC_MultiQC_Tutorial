# FastQC Tutorial

This repository contains a practical tutorial for quality control of FASTQ files using FastQC.

We have also included a `Dockerfile` to provide a pre-configured container environment (using micromamba) to ensure reproducibility across different operating systems.

## What You Will Learn

1. How to set up a working environment for FastQC.
2. How to run FastQC on one or more FASTQ files.
3. How to interpret the most important quality metrics.

## Requirements

- Linux, macOS, or Windows with WSL.
- FASTQ or FASTQ.GZ files to inspect.
- A terminal with Conda, or Docker installed.

## Recommended Setup

### Option 1: Conda Setup (Standard)
The easiest way to install FastQC locally is with Conda:

```bash
conda create -n fastqc-env -c bioconda -c conda-forge fastqc -y
conda activate fastqc-env

fastqc --version