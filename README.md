# Step 1: Get fastq files
```bash
fastq-dump --split-files SRR9595741
```

# Step 2: Check naming

rename to "SRR9595741_S1_L001_I1_001.fastq" (R1, R2 version)

# Step 3: Run cellranger
```bash
bash count.sh
```