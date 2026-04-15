# Step 1: Download Data

Download the raw FASTQ data files from GitHub:

```bash
mkdir -p data
wget -qO data/HG004_R1.fastq.gz "https://raw.githubusercontent.com/kbrown3687524/FastQC_MultiQC_Tutorial/main/data/HG004_R1.fastq.gz"
wget -qO data/HG004_R2.fastq.gz "https://raw.githubusercontent.com/kbrown3687524/FastQC_MultiQC_Tutorial/main/data/HG004_R2.fastq.gz"
```

This creates the `data/` directory and downloads both paired-end FASTQ files (HG004_R1 and HG004_R2).
