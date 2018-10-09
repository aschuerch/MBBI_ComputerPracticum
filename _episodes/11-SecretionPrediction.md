---
title: "Prediction of secreted proteins"
teaching: 10
exercises: 30
questions:
- "How can I predict proteins that are secreted?"
objectives:
- "Have a list of potential secreted proteins."
keypoints:
- "Q"
- " "
---

# Bioinformatics workflows

When working with high-throughput sequencing data, the raw reads you get off of the sequencer will need to pass
through a number of  different tools in order to generate your final desired output. The execution of this set of
tools in a specified order is commonly referred to as a *workflow* or a *pipeline*. 

An example of the workflow we will be using for our secreted protein prediction is provided below with a brief
description of each step. 

![workflow](../fig/MMBI_scheme_1.png)


1. Quality control - Trimming and/or filtering reads
2. Assembly of the trimmed reads into contigs
3. Annotation of genes, translation of genes
5. Prediction of secreted proteins

These workflows in bioinformatics adopt a plug-and-play approach in that the output of one tool can be easily
used as input to another tool without any extensive configuration. Having standards for data formats is what 
makes this feasible. Standards ensure that data is stored in a way that is generally accepted and agreed upon 
within the community. The tools that are used to analyze data at different stages of the workflow are therefore 
built under the assumption that the data will be provided in a specific format.  


# Prediction of secreted proteins


> ## Exercise
> 
> Which samples failed at least one of FastQC's quality tests? What
> test(s) did those samples fail?
>
>> ## Solution
>> 
>> We can get the list of all failed tests using `grep`. 
>> 
>> ~~~ 
>> $ cd ~/dc_workshop/docs
>> $ grep FAIL fastqc_summaries.txt
>> ~~~
>> {: .bash}
>> 
>> ~~~
>> FAIL	Per tile sequence quality	SRR097977.fastq
>> FAIL	Per tile sequence quality	SRR098026.fastq
>> FAIL	Overrepresented sequences	SRR098026.fastq
>> FAIL	Kmer Content	SRR098026.fastq
>> FAIL	Per base sequence quality	SRR098027.fastq
>> FAIL	Per tile sequence quality	SRR098027.fastq
>> FAIL	Kmer Content	SRR098027.fastq
>> FAIL	Per tile sequence quality	SRR098028.fastq
>> FAIL	Overrepresented sequences	SRR098028.fastq
>> FAIL	Kmer Content	SRR098028.fastq
>> FAIL	Overrepresented sequences	SRR098281.fastq
>> FAIL	Kmer Content	SRR098281.fastq
>> FAIL	Overrepresented sequences	SRR098283.fastq
>> FAIL	Kmer Content	SRR098283.fastq
>> ~~~
>> {: .output}
>> 
>> If we want to see all the files that failed at least one test, we
>> can use a combination of `grep`, `cut`, `sort` and `uniq`. 
>> 
>> ~~~
>> $ grep FAIL fastqc_summaries.txt | cut -f 3 | sort | uniq
>> ~~~
>> {: .bash}
>>
>> ~~~
>> SRR097977.fastq
>> SRR098026.fastq
>> SRR098027.fastq
>> SRR098028.fastq
>> SRR098281.fastq
>> SRR098283.fastq
>> ~~~
>> {: .output}
>> 
>> All of our samples failed at least one test. If we want to see a table showing which tests failed, we can 
>> use the same command we used above, but this time extract the 
>> second field with `cut` (instead of the third) and add the `-c` 
>> option to `uniq` to count the number of times each unique value
>> appears.
>>
>> ~~~
>> $ grep FAIL fastqc_summaries.txt | cut -f 2 | sort | uniq -c
>> ~~~
>> {: .bash}
>> 
>> ~~~
>> 5 Kmer Content
>> 4 Overrepresented sequences
>> 1 Per base sequence quality
>> 4 Per tile sequence quality
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}





