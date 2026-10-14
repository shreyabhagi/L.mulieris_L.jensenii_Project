> **My contributions (Shreya Bhagi)**
> This is my fork of a four-person Putonti Lab team project at Loyola University Chicago (Spring 2025). My work included:
> - Writing my own version of the Sylph pipeline script with timestamped logging, tool checks, per-sample error handling, intermediate-file cleanup and command-line arguments (visible in this repository's commit history; it was later consolidated into the team's final script)
> - Writing the reference-genome section of this README
>
> The team's final pipeline screens female gut metagenomes from NCBI SRA for two urogenital species, *L. mulieris* and *L. jensenii*, using Sylph ANI profiling.

---

**Are _Lactobacillus mulieris_ and _Lactobacillus jensenii_ present in the gastrointestinal tract?**

**Background:**

Both _L.mulleris_ and _L.jensenii_ are dominant members of the female urogenital microbiome. _L.mulleris_ is a recently recognized species, however, they share identical 16S rRNA sequences. This project aims to assess whether these species are also present in the gastrointestinal (GI) tract. The interest of these species in the GI tract is important as _L.jensenii_ helps the body digest food and absorb nutrients.

**Dependencies:**
- argparse
- sys
- os
- NCBI Datasets
- Sylph
- prefetch
- Fasterq-Dump
- Conda
- Bioconda

**Getting Started:**

1. Clone the dictionary using git clone

```
git clone https://github.com/Jsaroca227/L.mulieris_L.jensenii_Project.git
```

2. Download conda, if not installed already using the bash script
```
bash install_dependencies.sh
```
- Please follow the prompts in the terminal!

3. Download Sylph, if not installed already using conda

```
conda install -c conda-forge -c bioconda sylph
```

Sylph Toolkit: https://sylph-docs.github.io/sylph-cookbook/
- For more information on Sylph


**NCBI links to Reference Genomes:**

NCBI link for _L.jensenii_: https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_001936235.1/

NCBI link for _L.mulieris_: https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_042997415.1/

NCBI link for _E.coli_: https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_000005845.2/

- If you would like to use this wrapper for your own purposes, proceed with these following steps
   - These steps can be repeated to create the _E.coli_ database

Instructions to download the reference genome:
  1. Copy and paste the command from the "Dataset" tab of the NCBI link
```
datasets download genome accession GCF_001936235.1
```

```
datasets download genome accession GCF_042997415.1
```

  3. Unzip the dataset
```
unzip.ncbi_dataset
```
  4. Create directories for the database
```
mkdir ncbi_dataset
```
  5. Move the files inside the dataset
```
 mv L_jensenii/ncbi_dataset/data/GCF_001936235.1/* Jensenii_db/
 mv L_mulieris/ncbi_dataset/data/GCF_042997415.1/* Mulieris_db/
```
  6. Sketch the reference genomes to one database
```
sylph sketch Jensenii_db/*.fna Mulieris_db/*.fna
```

**Code and Test Data:**
- Test data can be found in: L.mulieris_L.jensenii_Project/sample_dataset.txt
- Code can be found in: L.mulieris_L.jensenii_Project/sylph_wrapper.py

Command to run wrapper in the background:
```
nohup python sylph_wrapper.py -i L.mulieris_L.jensenii_Project/sample_dataset.txt -p L.mulieris_L.jensenii_Project/sylph_db/database.syldb -t 4 &
```

**Overview of Wrapper:**

- Arguments
  - 3 command line arguments needed
    - -i SRA list.txt
    - -p reference genome database pathway
    - -t thread count

- Initialization
  - Checks for processed SRAs and reads the list of SRA IDs from input file

- Iterates through SRA IDs for processing
  - Skips already processed SRAs
  - Begins to process through SRA ID in the list

- Download and convert SRA to FASTQ
  - Utlizie prefetch to download the SRA file and utlize fasterq-dump to convert the SRA file to FASTQ file(s) format

- Sylph and ANI Profiling
  - Utilize sylph sketch to generate sketches from the FASTQ file(s) and run sylph profile to compare sketch against the reference genome database
    - Output of ANI score to results.tsv

- Processed SRA IDs
  - Updates processed_sra.log
    - Processed_sra.log: lists the SRA ID that has been processed and completed from the file
  - Updates lacto_db.tsv with SRA ID, Sylph header and correpsonding results
  - Removes SRA ID files to free up space


- Repetition
  - Continues processing all SRA IDs in the list until it is completed
  - As the script progreseses, processing.log is updated
    - Processing.log: lists the SRA ID that is currently being processed in real time

**Overview of Output Files:**

1. lacto_results.tsv: lists the SRA ID with the corresponding Sylph header and ANI results against the _Lactobacillus mulieris_ and _Lactobacillus jensenii_ database
2. ecoli_results.tsv: lists the SRA ID with the corresponding Sylph header and ANI results against the _E.coli_ database

