---
title: "Annotation"
teaching: 10
exercises: 50
questions:
- "How are proteins predicted from a DNA sequence?"
- "How many proteins are found in *E.coli*?"
objectives:
- "Search for open reading frames"
- "Predict a protein from an open reading frame"
- "Annotate a genome"
keypoints:
- "Genome annotation includes prediction of protein-coding genes, as well as other functional genome units"
- "Annotation starts by identifying open reading frames"
- "Predicted protein-coding sequences are further analysed with BLAST"
- "Larger DNA sequences or genomes require automated prediction and annotation"
---

## Annotation

Now that we have assembled the sequencing reads into contigs the next step to do is annotation of the sequences. The contigs we assembled contain different genomic features. The process of identifying and labelling those features is called genome annotation.

Genome annotation includes prediction of protein-coding genes, as well as other functional genome units such as structural RNAs, tRNAs, small RNAs, pseudogenes, control regions, direct and inverted repeats, insertion sequences, transposons and other mobile elements. It starts by identifying open reading frames (ORFs). Predicted sequences are further analysed to search for similarity to known elements, for example with BLAST.


Have a look at the first 3000 characters of the assembly.

~~~
$ head -c 3000 ~/Secretome_prediction/assembly/ERR022075.fasta
~~~
{: .source}


Let's copy the first contig including the header by moving over it with the mouse and pressing the left mouse button to copy.

> ## Exercise: Annotate a gene
> Go to [ORFfinder](https://www.ncbi.nlm.nih.gov/orffinder/). Paste the sequence including the header into the query 
> field. Choose the genetic code (translation table) 11. Start the search by pressing 'submit'. 
> This will open the Open Reading Frame viewer.
> 
> Search for the longest ORF. If you have found it, click on 'Mark'. Submit the longest ORF to BLAST.
> 
> How will this ORF be annotated? Is it a gene or something else? What does the gene do?
> 
{: .challenge}


## Automated Annotation

Now we will annotate all genomes with an automated approach. PROKKA is a pipeline script which coordinates a series of genome feature predictor tools and sequence similarity tools to annotate the genome sequence or contigs. 
A range of programs are available for these tasks but here we will use PROKKA, which is a pipeline comprising several open source bioinformatic tools and databases.

PROKKA automates the process of locating ORFs and RNA regions on contigs, translating ORFs to protein sequences, searching for protein homologs and producing standard output files. For gene finding and translation, PROKKA makes use of the program Prodigal. Homology searching (via BLAST and HMMER) is then performed using the translated protein sequences as queries against a set of public databases (CDD, PFAM, TIGRFAM) as well as custom databases that come with PROKKA.

We need to make a new folder to contain our annotation results.

~~~
$ cd ~/Secretome_prediction
$ mkdir annotation
~~~
{: .bash}

Now we are all set to annotate our contigs with PROKKA. Again, this will run for a while.
The parameter --outdir tells PROKKA which output directory to write to. This needs to be a new directory. 
The parameter --prefix assigns the sample name as a prefix to all files. If we ommit this, every PROKKA output file would have the same name. The parameter --cpus 1 tells it to use 1 cpu.

~~~
$ prokka --outdir ~/Secretome_prediction/annotation/ERR022075 --prefix ERR022075 ~/Secretome_prediction/assembly/ERR022075.fasta --cpus 1
~~~
{: .bash}

Let's check the output:

~~~
$ cd ~/Secretome_prediction/annotation/
$ ls
~~~
{: .bash}

PROKKA creates a range of output files, including a file containing the protein sequences (.faa). Take a moment to check the files out with 'head'.

> ## Challenge: How many coding regions did PROKKA find in the contigs??
>
> Find out how many coding regions there are in the *E. coli* isolate. The ERR022075.txt files contain some statistics on how many annotated genes are found etc.  
>
> Hint:
> ~~~
> $ grep "CDS" 
> ~~~
> prints matching lines for each input file.
> 
> > ## Solution
> >
> > 
> > ~~~
> > $ cd ~/Secretome_prediction/annotation/ERR022075
> > $ grep CDS ERR022075.txt
> >  
> > CDS: 4235
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Is your solution the same or do you get other numbers of coding regions? What could be possible explanations 
if the solution differs? 

> ## Challenge: Annotation of *S.aureus* 
>
> In addition to *E.coli*, there is an assembled, but unannotated genome of *S.aureus* on your remote machine. Annotate this genome as well.
>
> Hint:
> ~~~
> $ cd ~/Secretome_prediction/assembly/S_aureus
> ~~~
> > ## Solution
> >
> > 
> > ~~~
> > $ cd ~/Secretome_prediction/assembly/S_aureus 
> > prokka --outdir ~/Secretome_prediction/annotation/S_aureus --prefix S_aureus ~/Secretome_prediction/assembly/S_aureus/NC_007793.1.fasta --cpus 1
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

{% include links.md %}
