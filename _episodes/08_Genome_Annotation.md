---
title: "Annotation"
start: true
teaching: 10
exercises: 50
questions:
- "How are proteins predicted from a DNA sequence?"
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

Now that we have assembled the data into contigs the next natural step to do is annotation of the data. The contigs we assembled contain different genomic features. The process of identifying and labelling those features is called genome annotation.

Genome annotation includes prediction of protein-coding genes, as well as other functional genome units such as structural RNAs, tRNAs, small RNAs, pseudogenes, control regions, direct and inverted repeats, insertion sequences, transposons and other mobile elements. It starts by identifying open reading frames (ORFs). Predicted sequences are further analysed to search for similarity to known elements, for example with BLAST.


Have a look at the 50 first lines of your genomes using head. In this example the folder with assembly ERR029207 is used. This example assumes your assembly outputs from SPADES are in the folder "assembly".

~~~
$ head -n50 ~/assembly/ERR029207/scaffolds.fasta
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
> How will this ORF be annotated? Is it a gene or something else? What does the gene do? Fill your annotation into the
> [table](https://docs.google.com/spreadsheets/d/1xjiliy_USyMwiyzEgWhpn8_109F7Z3jPM_f7Jp-lOb8/edit?usp=sharing) under the header ERR029207_ORF.
> 
{: .challenge}


## Automated Annotation

Now we will annotate all genomes with an automated approach. Prokka is a pipeline script which coordinates a series of genome feature predictor tools and sequence similarity tools to annotate the genome sequence or contigs. 
A range of programs are available for these tasks but here we will use PROKKA, which is a pipeline comprising several open source bioinformatic tools and databases.

PROKKA automates the process of locating ORFs and RNA regions on contigs, translating ORFs to protein sequences, searching for protein homologs and producing standard output files. For gene finding and translation, PROKKA makes use of the program Prodigal. Homology searching (via BLAST and HMMER) is then performed using the translated protein sequences as queries against a set of public databases (CDD, PFAM, TIGRFAM) as well as custom databases that come with PROKKA.

The names of the contigs produced by SPades are quite long. PROKKA needs name which are shorter than 20 characters. We will therefore shorten these names first by making a copy of the contig files. We will make use of a for loop, which takes does three things, it gets takes the first 2 columns in your fasta file, delimited by an underscore. Then it renames the long word "NODE" to the letter "C" in you r file and then it outputs the resulting fasta file to a file called genome1.fasta or genome2.fasta. Replace genome1 and genome2 by the names of your folders.


~~~
$ cd ~/assembly
$ for sample in genome1 genome2
> do
>  cut -f 1,2 -d "_" "$sample"/scaffolds.fasta | sed s/NODE/C/g > "$sample".fasta
> done
~~~

Let's see what this has done. First let's have a look at the original files.

~~~
$ head -n10 genome1/scaffolds.fasta
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
$ head -n10 genome1.fasta
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
$ grep  ">" genome1.fasta
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
$ cd ~/
$ mkdir annotation
~~~
{: .bash}

Now we are all set to annotate our contigs with PROKKA. Again, this will run for a while.
The parameter --outdir tells PROKKA which output directory to write to. This needs to be a new directory. 
The parameter --prefix assigns the sample name as a prefix to all files. If we ommit this, all output files would have the same name. The parameter --cpus 1 tells it to use 1 cpu, the parameters --usegenus and --genus Streptococcus will tell prokka to use the Streptococcus annotation database.

~~~
$ cd ~/
$ for sample in genome1 genome2 ; do
  prokka --outdir annotation/"$sample" --prefix $sample assembly/"$sample".fasta --usegenus -genus Streptococcus --cpus 1
done
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
> Find out how many coding regions there are in the *S. pneumoniae* isolates. Enter your solution in the
> [table](https://docs.google.com/spreadsheets/d/1b8BPKcSUuW2YzgHdMaJN3MEbdgroRJa1dWnf5gkHr9M/edit#gid=0) under the head 'Number of CDS'
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
> > $ cd ~/annotation/
> > $ grep CDS */*.txt
> >  
> > ERR326690/ERR326690.txt:CDS: 2047
> > ERR326694/ERR326694.txt:CDS: 2003
> > ERR326699/ERR326699.txt:CDS: 2009
> > ERR326705/ERR326705.txt:CDS: 2035
> > ERR326710/ERR326710.txt:CDS: 1959
> > ERR326733/ERR326733.txt:CDS: 1975
> > ERR326735/ERR326735.txt:CDS: 1914
> > ERR326740/ERR326740.txt:CDS: 2022
> > ERR326743/ERR326743.txt:CDS: 1969
> > ERR326749/ERR326749.txt:CDS: 2062
> > ERR326761/ERR326761.txt:CDS: 2010
> > ERR326770/ERR326770.txt:CDS: 2220
> > ERR326777/ERR326777.txt:CDS: 1961
> > ERR331391/ERR331391.txt:CDS: 1985
> > ERR331393/ERR331393.txt:CDS: 2095
> > ERR331398/ERR331398.txt:CDS: 1989
> > ERR331400/ERR331400.txt:CDS: 1971
> > ERR331401/ERR331401.txt:CDS: 2015
> > ERR331402/ERR331402.txt:CDS: 1909
> > ERR331403/ERR331403.txt:CDS: 2168
> > ERR331406/ERR331406.txt:CDS: 1903
> > ERR331408/ERR331408.txt:CDS: 1953
> > ERR331409/ERR331409.txt:CDS: 1946
> > ERR331413/ERR331413.txt:CDS: 1952
> > ERR331414/ERR331414.txt:CDS: 1958
> > ERR331416/ERR331416.txt:CDS: 1904
> > ERR331418/ERR331418.txt:CDS: 1904
> > ERR331421/ERR331421.txt:CDS: 1900
> > ERR331426/ERR331426.txt:CDS: 1969
> > ERR331427/ERR331427.txt:CDS: 2060
> > ERR331428/ERR331428.txt:CDS: 2007
> > ERR331429/ERR331429.txt:CDS: 1907
> > ERR331431/ERR331431.txt:CDS: 2055
> > ERR331433/ERR331433.txt:CDS: 1909
> > ERR331435/ERR331435.txt:CDS: 2086
> > ERR331437/ERR331437.txt:CDS: 1902
> > ERR331438/ERR331438.txt:CDS: 1987
> > ERR331439/ERR331439.txt:CDS: 1914
> > ERR331442/ERR331442.txt:CDS: 2000
> > ERR331448/ERR331448.txt:CDS: 2215
> > ERR331449/ERR331449.txt:CDS: 2062
> > ERR331455/ERR331455.txt:CDS: 1938
> > ERR331462/ERR331462.txt:CDS: 2045
> > ERR331463/ERR331463.txt:CDS: 2041
> > ERR331466/ERR331466.txt:CDS: 2028
> > ERR331467/ERR331467.txt:CDS: 2111
> > ERR331478/ERR331478.txt:CDS: 2077
> > ERR338170/ERR338170.txt:CDS: 1938
> > ERR338196/ERR338196.txt:CDS: 2025
> > ERR338233/ERR338233.txt:CDS: 1977
> > ERR338248/ERR338248.txt:CDS: 2108
> > ERR338250/ERR338250.txt:CDS: 1971
> > ERR340827/ERR340827.txt:CDS: 2049
> > ERR340839/ERR340839.txt:CDS: 1986
> > ERR340845/ERR340845.txt:CDS: 1913
> > ERR340855/ERR340855.txt:CDS: 1951
> > ERR340856/ERR340856.txt:CDS: 1897
> > ERR340875/ERR340875.txt:CDS: 1902
> > ERR340884/ERR340884.txt:CDS: 1961
> > ERR340886/ERR340886.txt:CDS: 1958
> > ERR340887/ERR340887.txt:CDS: 1919
> > 
> > These *S. pneumoniae* genomes contain approx 2000 coding regions.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Is your solution the same or do you get other numbers of coding regions? What could be possible explanations 
if the solution differs? 

Repeat the same exercise with the key word 'tRNA' to see how many tRNAs there are and fill into the [table](https://docs.google.com/spreadsheets/d/1b8BPKcSUuW2YzgHdMaJN3MEbdgroRJa1dWnf5gkHr9M/edit#gid=0) under the head 'tRNA'

{% include links.md %}
