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
- "It often starts by identifying open reading frames"
- "Predicted sequences are further analysed with BLAST"
- "Larger DNA sequences or genomes require automated prediction and annotation"
---

## Annotation

Now that we have assembled the sequencing reads into contigs the next step to do is annotation of the sequences. The contigs we assembled contain different genomic features. The process of identifying and labelling those features is called genome annotation.

Genome annotation includes prediction of protein-coding genes, as well as other functional genome units such as structural RNAs, tRNAs, small RNAs, pseudogenes, control regions, direct and inverted repeats, insertion sequences, transposons and other mobile elements. It starts by identifying open reading frames (ORFs). Predicted sequences are further analysed to search for similarity to known elements, for example with BLAST.


Have a look at the 50 first lines of your genome.

~~~
$ head -n50 ~/112018_Secretome/assembly/ERR022075/scaffolds.fasta
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

The names of the contigs produced by SPades are quite long. PROKKA needs name which are shorter than 20 characters. We will therefore shorten these names first by making a copy of the contig files. Then it renames the word "NODE" to the letter "C" in your file and it outputs the resulting fasta file to a file called ERR022075.fasta.


~~~
$ cd ~/112018_Secretome/assembly/ERR022075
cut -f 1,2 -d "_" scaffolds.fasta | sed s/NODE/C/g > ERR022075.fasta  
> done
~~~

Let's see what this has done. First let's have a look at the original files.

~~~
$ head -n10 ERR022075/scaffolds.fasta
~~~
{: .bash}

~~~
>NODE_1_length_43202_cov_4.41729_ID_34907
TAGCTTGTCGAGCGTGCGGGGGCTTATGCGTCTGCTCGCCCTAGAGACTGAGCAAGATCT
CCCGGGCCAGCGCCGAGGTCGCCGATGGGGTCTTGCCGACCTTCACACCGGCGGCCTCCA
GGGCCTCTTGCTTGGCCGCCGCTGTGCCAGACGAGCCGGAGACGATGGCGCCGGCGTGGC
CCATCGTCTTGCCTTCGGGTGCGGTAAATCCGGCGACATAGCCGACGACCGGCTTGGACA
CGTTGGTCTTGATGAAGTCTGCGGCCCGCTCCTCGGCGTCACCACCGATCTCGCCGATCA
TCACGATGAGCTTGGTGTCCGGATCCCTCTCGAAGGCCTCGATGGCGTCGATGTGGGTAG
TGCCAATCACCGGATCACCACCGATGCCGATCGCCGTGGAGAATCCAAGGTCGCGCAGTT
CGAACATCATCTGGTAGGTCAACGTCCCCGACTTGGACACCAGACCAATTGGACCGGGTC
CGGTGATGTTGGCCGGCGTGATACCGGCCAGCGACTGACCGGGACTGATAATGCCAGGAC
~~~
{: .output}

Now, let's inspect the new file

~~~
$ head -n10 ERR022075.fasta
~~~
{: .bash}

~~~
>C_34907
TAGCTTGTCGAGCGTGCGGGGGCTTATGCGTCTGCTCGCCCTAGAGACTGAGCAAGATCT
CCCGGGCCAGCGCCGAGGTCGCCGATGGGGTCTTGCCGACCTTCACACCGGCGGCCTCCA
GGGCCTCTTGCTTGGCCGCCGCTGTGCCAGACGAGCCGGAGACGATGGCGCCGGCGTGGC
CCATCGTCTTGCCTTCGGGTGCGGTAAATCCGGCGACATAGCCGACGACCGGCTTGGACA
CGTTGGTCTTGATGAAGTCTGCGGCCCGCTCCTCGGCGTCACCACCGATCTCGCCGATCA
TCACGATGAGCTTGGTGTCCGGATCCCTCTCGAAGGCCTCGATGGCGTCGATGTGGGTAG
TGCCAATCACCGGATCACCACCGATGCCGATCGCCGTGGAGAATCCAAGGTCGCGCAGTT
CGAACATCATCTGGTAGGTCAACGTCCCCGACTTGGACACCAGACCAATTGGACCGGGTC
CGGTGATGTTGGCCGGCGTGATACCGGCCAGCGACTGACCGGGACTGATAATGCCAGGAC
~~~
{: .output}

The header was shortened to less than 20 characters. It is important that each header is still unique. We can check this by inspecting a few headers.

~~~
$ grep  ">" ERR022075.fasta
~~~
{: .bash}

~~~
>C_36541
>C_36543
>C_36545
>C_36547
>C_36549
>C_36551
>C_36553
>C_36555
>C_36557
>C_36559
>C_36561
>C_36563
>C_36565
>C_36567
>C_36569
>C_36571
>C_36573
>C_36575
>C_36577
>C_36579
>C_36581
~~~
{: .output}

These are the last few lines of our output. Every line has a unique number.

We still need to make a new folder to contain our annotation results.

~~~
$ cd ~/112018_Secretome
$ mkdir annotation
~~~
{: .bash}

Now we are all set to annotate our contigs with PROKKA. Again, this will run for a while.
The parameter --outdir tells PROKKA which output directory to write to. This needs to be a new directory. 
The parameter --prefix assigns the sample name as a prefix to all files. If we ommit this, all output files would have the same name. The parameter --cpus 1 tells it to use 1 cpu.

~~~
$ cd 
prokka --outdir ~/112018_Secretome/annotation/ERR022075 --prefix $sample ~/112018_Secretome/assembly/ERR022075.fasta --cpus 1
~~~
{: .bash}

Let's check the output:

~~~
$ cd ~/annotation/
$ cat */*.txt
~~~
{: .bash}

The .txt files contain some statistics on how many annotated genes are found etc. 

> ## Challenge: How many coding regions did PROKKA find in the contigs??
>
> Find out how many coding regions there are in the *E. coli* isolate. 
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
> > $ cd ~/112018_Secretome/annotation/
> > $ grep CDS */*.txt
> >  
> > ERR326690/ERR326690.txt:CDS: 2047
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Is your solution the same or do you get other numbers of coding regions? What could be possible explanations 
if the solution differs? 

> ## Challenge: Annotation of *S.aureus* 
>
> In addition to *E.coli*, there is an assembled, but unannotated genome of *S.aureus* on your remote machine. Can you annotate this genome as well?

> Hint:
> ~~~
> $ cd ~/assembly/S_aureus
> ~~~
> > ## Solution
> >
> > 
> > ~~~
> > $ cd ~/assembly/S_aureus 
> > prokka --outdir /112018_Secretome/annotation/S_aureus/... --prefix $sample ~/assembly/S_aureus/.....fasta --cpus 1
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

{% include links.md %}
