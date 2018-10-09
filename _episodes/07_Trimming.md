---
title: "Trimming"
teaching: 10
exercises: 10
questions:
- "How can I get rid of sequence data that doesn’t meet my quality standards?"
- "How can I organize a file system for a bioinformatics project?"
objectives:
- "Clean FASTQ reads using seqtk"
- "Start a a directory structure for your project" 
keypoints:
- "Sequencing data needs to be trimmed or corrected before use"
- "Spend the time to organize your file system. Your future self will thank you!"
---

Let's start to work on the *E.coli* genome. We are going to follow this workflow:

../fig/MMBI_schemes.png



## Trimming and more practice with pipes

Let's use the tools we've added to our tool kit so far, along with a few new ones, to trim our short read sequences. 
First, let's navigate to the correct directory.

~~~
$ cd
$ cd assembly
$ ls
~~~
{: .bash}

These files contain the short read data of *E.coli*. Before we start the assemble the short reads into contigs, we have to trim the low quality sequences away. 


# Cleaning Reads

It's very common to have some reads within a sample,
or some positions (near the beginning or end of reads) across all
reads that are low quality and should be discarded. We will use a program called
[seqtk](https://github.com/lh3/seqtk) to
filter poor quality reads and trim poor quality bases from our samples.

## Seqtk Options

Seqtk is a program written C and aims to be a Swiss army knife for sequencing reads. 
You don't need to learn C to use Seqtk, but the fact that it's a C program helps
explain the syntax that is used to run Seqtk. The basic
command to run Seqtk starts like this:

~~~
$ seqtk
~~~
{: .bash}


That's just the basic command, however. Seqtk has a variety of
options and parameters. We will need to specify what options we want
to use for our analysis. Here are some of the options:


| option    | meaning |
| ------- | ---------- |
| `seq` | common transformation of FASTA/Q |
|  `comp`   | get the nucleotide composition of FASTA/Q |
|  `trimfq` | trim FASTQ using the Phred algorithm |

In addition to these options, there are a number if  trimming options
available:

~~~
$ seqtk trimfq
~~~
{: .bash}

| step   | meaning |
| ------- | ---------- |
| `-q` | error rate threshold (disabled by -b/-e) [0.05] |
| `-l`  | maximally trim down to INT bp (disabled by -b/-e) [30]  |
|  `-b` |  trim INT bp from left (non-zero to disable -q/-l) [0] |
| `-e`  |  trim INT bp from right (non-zero to disable -q/-l) [0] |

We will use only a few of these options in our
analysis. It is important to understand the steps you are using to
clean your data.

A complete command for trimming with seqtk will look something like this:

~~~


$ seqtk trimfq -q 0.01 ERR022075_1.fastq.gz > ERR022075_1.trimmed.fastq
~~~
{: .bash}

Seqtk conveniently compresses our fastq file while trimming, which means we do not need the .gz ending anymore.

## Trimming

Now we will run seqtk trimfq on our data. To begin, create the folder that will hold our trimmed data and then navigate to your `untrimmed` data directory:

~~~
$ cd ~/112018_Secretome
$ mkdir trimmed
$ mkdir trimmed
$ cd ~/112018_Secretome/reads
~~~
{: .bash}

We are going to run seqtk on one sample giving it an error rate threshold of 0.01 which indicates the base call accuracy. We request that, after trimming, the chances that a base is called incorrectly are only 1 in 10000.

~~~
$ seqtk trimfq -q 0.01 ERR022075_1.fastq> ~/112018_Secretome/trimmed/ERR022075_1.trimmed.fastq
~~~
{: .bash}

Notice that we needed to redirect the output to a file. If we don't do that, the trimmed fastq data will be displayed in the console.


> ## Exercise
> Trim the second read pair with the same parameters and write into a new file
> 
> 
> > ## Solution
> > 
> > `$ cd ~/112018_Secretome/reads`
> > `$ seqtk trimfq -q 0.01 ERR022075_2.fastq > ~/112018_Secretome/ERR022075_2.trimmed.fastq`
> >
> {: .solution}
{: .challenge}
