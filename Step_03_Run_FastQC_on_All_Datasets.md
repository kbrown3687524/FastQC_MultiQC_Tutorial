# Step 3: Run FastQC on All Datasets

Execute FastQC on each FASTQ file in the `data/` directory:

```bash
fastqc data/HG004_R1.fastq.gz -o output
fastqc data/HG004_R2.fastq.gz -o output
```

FastQC will generate:
- `HG004_R1_fastqc.html` - Quality report for first read pair
- `HG004_R1_fastqc.zip` - Detailed data archive
- `HG004_R2_fastqc.html` - Quality report for second read pair
- `HG004_R2_fastqc.zip` - Detailed data archive

All output files are saved in the `output/` directory.
