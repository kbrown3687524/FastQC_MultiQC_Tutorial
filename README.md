# FastQC and MultiQC Tutorial

This repository contains a practical tutorial for quality control of FASTQ files using FastQC and MultiQC.

FastQC checks read-level quality metrics for individual samples. MultiQC collects those results into a single, easy-to-review report across all samples.

## What You Will Learn

1. How to set up a working environment for FastQC and MultiQC.
2. How to run FastQC on one or more FASTQ files.
3. How to combine the FastQC results into a MultiQC report.
4. How to interpret the most important quality metrics.

## Requirements

- Linux, macOS, or Windows with WSL.
- FASTQ or FASTQ.GZ files to inspect.
- A terminal with either Conda or Python available.

## Recommended Setup

The easiest way to install both tools is with Conda.

```bash
conda create -n fastqc-multiqc -c bioconda -c conda-forge fastqc multiqc -y
conda activate fastqc-multiqc
```

If you already have the tools installed, confirm they are available:

```bash
fastqc --version
multiqc --version
```

## Tutorial Workflow

### 1. Prepare a working directory

Organize your input files in a folder such as `data/`.

```bash
mkdir -p data fastqc_results multiqc_report
```

Put your FASTQ files into `data/`. For example:

- `data/sample_1.fastq.gz`
- `data/sample_2.fastq.gz`
- `data/sample_3.fastq.gz`

### 2. Run FastQC

Run FastQC on all FASTQ files in the input directory.

```bash
fastqc -o fastqc_results data/*.fastq.gz
```

FastQC creates one HTML report and one ZIP file per sample in `fastqc_results/`.

If you want to process uncompressed FASTQ files, use the same command with `*.fastq`.

### 3. Aggregate results with MultiQC

Once FastQC has finished, summarize all sample reports with MultiQC.

```bash
multiqc fastqc_results -o multiqc_report
```

MultiQC scans the FastQC output and builds a single report at:

```text
multiqc_report/multiqc_report.html
```

### 4. Open the reports

Open the FastQC reports for sample-by-sample inspection and the MultiQC report for an overall summary.

```bash
xdg-open fastqc_results/sample_1_fastqc.html
xdg-open multiqc_report/multiqc_report.html
```

On macOS, use `open` instead of `xdg-open`.

## How To Read the Results

### FastQC modules to review first

- Per base sequence quality: checks whether read quality drops toward the 3' end.
- Per sequence quality scores: shows the overall quality distribution across reads.
- Per base sequence content: highlights nucleotide composition bias.
- Adapter content: detects leftover adapter contamination.
- Duplication levels: shows whether the library has many repeated reads.
- Overrepresented sequences: identifies sequences that appear unusually often.

### Common interpretations

- A quality drop at the end of reads is common in Illumina data and may justify trimming.
- Strong adapter content usually means trimming is needed before alignment or assembly.
- High duplication can be acceptable for some assays, but may indicate overamplification in others.
- Strong sequence bias at the start of reads can be expected in some protocols, but should be checked carefully.

### Why MultiQC is useful

MultiQC makes it easier to compare multiple samples at once. It is especially helpful when you want to spot outliers, batch effects, or one problematic library among many good ones.

## Example Analysis Session

Use this as a simple end-to-end example.

```bash
mkdir -p data fastqc_results multiqc_report
fastqc -o fastqc_results data/*.fastq.gz
multiqc fastqc_results -o multiqc_report
```

After running the commands above, review:

1. The individual FastQC HTML files in `fastqc_results/`.
2. The summary tables and plots in `multiqc_report/multiqc_report.html`.

## Troubleshooting

- If `fastqc` or `multiqc` is not found, make sure your Conda environment is activated.
- If no files are processed, confirm that your FASTQ path and file extensions are correct.
- If reports look incomplete, verify that FastQC finished without errors before running MultiQC.
- If you are working with many large files, expect FastQC to take some time and use more memory.

## Optional Next Steps

After QC, you can continue with trimming, alignment, and deeper downstream analysis. A typical workflow is:

1. Run FastQC on raw reads.
2. Trim adapters or low-quality bases if needed.
3. Run FastQC again on trimmed reads.
4. Use MultiQC to summarize before and after QC results.

## Notes

This tutorial is intentionally tool-focused and does not require any additional project files. If you want, you can extend it later with sample data links, screenshots, or a trimming step using Trim Galore, cutadapt, or fastp.
