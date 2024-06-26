---
layout: default
parent: Single cell
title: Examples
nav_order: 4
---

## Real-world example

This is an example of an end-to-end cmd-line-only workflow:

    # Download HiFi reads
    $ wget https://downloads.pacbcloud.com/public/dataset/Kinnex-single-cell-RNA/TUTORIAL-DATA-PBMC-single-cell-mini/ccs.bam
    $ wget https://downloads.pacbcloud.com/public/dataset/Kinnex-single-cell-RNA/TUTORIAL-DATA-PBMC-single-cell-mini/ccs.bam.pbi

    # Download cDNA primers
    $ wget https://downloads.pacbcloud.com/public/dataset/Kinnex-single-cell-RNA/TUTORIAL-DATA-PBMC-single-cell-mini/primers.fasta
    
    # Download cell barcode include list
    $ wget https://downloads.pacbcloud.com/public/dataset/Kinnex-single-cell-RNA/REF-10x_barcodes/3M-february-2018-REVERSE-COMPLEMENTED.txt.gz

    # Check lima version to be >= 2.6.0
    $ lima --version
    lima 2.6.0

    # Check isoseq version to be >= 4.0.0
    $ isoseq --version
    isoseq 4.0.0 

    # cDNA primer removal and read orientation
    $ lima --per-read --isoseq ccs.bam primers.fasta output.bam

    # Clip UMI and cell barcode
    $ isoseq tag output.5p--3p.bam flt.bam --design T-12U-16B

    # Remove poly(A) tails and concatemer
    $ isoseq refine flt.bam primers.fasta fltnc.bam --require-polya

    # Correct single cell barcodes based on an include list
    $ isoseq correct -B 3M-february-2018-REVERSE-COMPLEMENTED.txt.gz fltnc.bam corrected.bam

    # Deduplicate reads based on UMIs
    $ samtools sort -t CB corrected.bam -o corrected.sorted.bam
    $ isoseq groupdedup corrected.sorted.bam dedup.bam 
