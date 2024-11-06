# MAPK Phylogeny Phylotocol
 
 Principal Investigators: Karen Carvajal-Soriano, Leonardo Ibarra-Castro, Joseph Ryan  

 Version Number: v.0.01

 Date: 6 Nov 2024  

## SUMMARY OF CHANGES FROM PREVIOUS VERSION

NA

## 1 INTRODUCTION: BACKGROUND INFORMATION AND SCIENTIFIC RATIONALE  

### 1.1 _Background Information_  

Genes that code for ELOVL (PF01151) and FADS (PF00487) domains are important for  
long-chain polyunsaturated fatty acid (LC-PUFA) biosynthesis. 

### 1.2 _Rationale_  

We are conducting a phylogenetic analysis to see the distribution of these
genes in key aquaculture fish species.

## 2 STUDY DESIGN AND ENDPOINTS  

#### 2.1 DATASETS AND SOFTWARE

* hmm2aln.pl downloaded from https://github.com/josephryan/hmm2aln.pl
* RNA-Seq data were downloaded from NCBI SRA for Centropomus viridis (SRR8067690), Lutjanus campechanus (SRR12094749), Micropogonias undulatus (SRR10001346), Pagrus pagrus (ERR13654250) Siganus canaliculatus (SRR16574310) and Sciaenops ocellatus (SRR26320965 and SRR26320968)
* Protein models were downloaded from the Darwin Tree of Life project for Hyperoplus immaculatus, Pholis gunnellus, Phoxinus phoxinus,
Pleuronectes platessa, Salmo trutta, Syngnathus acus, Taurulus bubalis, and Telmatherina bonti.
* Protein models were downloaded from from ENSEMBL for Danio rerio, Oncorhynchus mykiss, Oreochromis niloticus, and Salmo salar
* HumRef2024.fa was created in January of 2024 using https://github.com/josephryan/reduce_refseq
* These files were put into a directory called "aa" where these analyses were conducted
* ELOVL PFAM model downloaded from https://www.ebi.ac.uk/interpro/wwwapi/entry/pfam/PF01151?annotation=hmm
* FADS PFAM model downloaded from https://www.ebi.ac.uk/interpro/wwwapi/entry/pfam/PF00487?annotation=hmm

#### 2.2 COMMANDS

For key species where we quality protein models were not available, we downloaded RNA-Seq data from SRA using fastq-dump (version 3.0.0) from NCBI's SRA Toolkit

```bash
fastq-dump --skip-technical --defline-seq '@$sn[_$rn]/$ri' --readids --dumpbase --split-files --split-spot --clip SRR8067690 > SRR8067690.fastq-dump.out 2> SRR8067690.fastq-dump.err
```

We assembled the above rna-seq data using Trinity (v2.15.0)

```bash
/usr/local/trinityrnaseq-v2.15.0/Trinity --seqType fq --max_memory 200G --CPU 40 --trimmomatic --left SRR8067690_1.fastq --right SRR8067690_2.fastq --output SRR8067690.trinity
```

We identify all open reading frames longer than 100 using Transdecoder

```bash
TransDecoder.LongOrfs --gene_trans_map SRR8067690.trinity.Trinity.fasta.gene_trans_map --output_dir SRR8067690.transdecoder.longorfs -t SRR8067690.trinity.Trinity.fasta
```

All definition lines from ORF files and downloaded protein sets are renamed to include genus species information

We are using hmm2aln.pl (v 2.0.0) with the PF01151 and PF00487 PFAM domains with these datasets to 
(1) identify domains in this set and simultaneously construct an alignment to the PFAM model.

```bash
hmm2aln.pl --fasta_dir=aa --threads=100 --hmm=PF01151.hmm --name=ELOVL > ELOVL.aln.fa
hmm2aln.pl --fasta_dir=aa --threads=100 --hmm=PF00487.hmm --name=FADS > FADS.aln.fa
```

Run maximum-likelihood trees

```
iqtree -T AUTO -s ELOVL.aln.fa
iqtree -T AUTO -s FADS.aln.fa
```

## 3 WORK COMPLETED SO FAR WITH DATES  

6 Nov 2024 - I have downloaded the protein model datasets, hmms, and SRA fastq files. I have assembled and translated all of the RNA-Seq data except for 1.

## APPENDIX

Version&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;Date&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Significant Revisions  
1.1  
1.2  
1.3  
1.4  
