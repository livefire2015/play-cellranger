# Download sratoolkit

Follow this [Github repo](https://github.com/ncbi/sra-tools) to download the tool for your env.
For example, the one I downloaded for my Ubuntu-20.04 is sratoolkit.2.10.9-ubuntu64.

# Download cellranger

Follow this [10x Genomics page](https://support.10xgenomics.com/single-cell-gene-expression/software/downloads/latest) to download the tool for your env.
For example, the one I got is cellranger-5.0.1.

To verify your download, run `cellranger testrun`: Execute the 'count' pipeline on a small test dataset.

# Download lab data

For proof-of-concept test, go to this [SRA archive data](https://trace.ncbi.nlm.nih.gov/Traces/sra/?run=SRR9595741) to get some sample fastq data.

# Download ref data

Human reference (GRCh38) or Mouse reference dataset is required for Cell Ranger. For our lab data SRR9595741, get the mouse ref data aslo on the [10x Genomics page](https://support.10xgenomics.com/single-cell-gene-expression/software/downloads/latest). For example, the one I got is from this cmd:

```bash
curl -O https://cf.10xgenomics.com/supp/cell-exp/refdata-gex-mm10-2020-A.tar.gz
```

# Extract fastq files

The downloaded data, say, SRR9595741, might not be readily useful, so it might be necessary to extract I1, R1, and R2 fastq files from it, by running this cmd.

```bash
fastq-dump --split-files SRR9595741
```

# Check naming

The output from fastq-dump might be just SRR9595741_1.fastq, SRR9595741_2.fastq, SRR9595741_3.fastq. The tool *cellranger* does not recognize them and throws error like this:

```
error: No input FASTQs were found for the requested parameters.

If your files came from bcl2fastq or mkfastq:
 - Make sure you are specifying the correct --sample(s), i.e. matching the sample sheet
 - Make sure your files follow the correct naming convention, e.g. SampleName_S1_L001_R1_001.fastq.gz (and the R2 version)
 - Make sure your --fastqs points to the correct location.
 - Make sure your --lanes, if any, are correctly specified.

Refer to the "Specifying Input FASTQs" page at https://support.10xgenomics.com/ for more details.
```

To fix, do the following renaming:

* *SRR9595741_1.fastq* to *SRR9595741_S1_L001_I1_001.fastq*
* *SRR9595741_2.fastq* to *SRR9595741_S1_L001_R1_001.fastq*
* *SRR9595741_3.fastq* to *SRR9595741_S1_L001_R2_001.fastq*

# Run cellranger count

* id: a random id, will be used for output
* fastqs: the path to the directory where all fastq files locate
* sample: the prefix in the filename of the fastq file
* transcriptome: the path to the ref data directory

```bash
cellranger count --id=run_count_SRR9595741 \
        --fastqs=/mnt/c/Users/CLX-Set/Development/NGS/SRR9595741_fastq \
        --sample=SRR9595741 \
        --transcriptome=/mnt/c/Users/CLX-Set/Development/NGS/refdata-gex-mm10-2020-A
```