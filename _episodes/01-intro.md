---
title: "Introduction"
start: true
teaching: 10
exercises: 0
questions:
- "What are the goals of this practical training?"
- "What data do we work on?"
objectives:
- "Describe the goals of this practical training."
keypoints:
- "After the practicum you will have some familiarity with working on the command line and will have a list of secreted proteins of two different species"
---
# Bioinformatics

Many bioinformatics tools can only be used through a command line interface, or have extra capabilities in the command line version that are not available in a graphical user interface. In these lessons we will first introduce the Unix shell and then apply this knowlegde to the assembly of a bacterial genome and make predictions about genes on the genome.

## Introduction to the command line

The lessons aim to introduce basic skills in the Unix shell and are designed for learners with no programming experience. The general topics and skills covered include:

### Interacting with Computers
- Could Computing
- Connecting to remote computing (SSH/PuTTY)

### Data Management and Organization
- Unix Shell (command-line: ls, cd, mkdir, cp, rm, wc, grep, cut, columns, head, tail, less etc,)


## Microbial Genomics

### Predict secreted proteins from sequenced DNA

The sequence of microbial genomes can be used to predict proteins that are secreted by bacteria. However, before we obtain a bacterial genome sequence from DNA sequencing data, we have to clean the data and assemble it.

We will carry out the following steps:

- Trimming of sequencing data
- Assembly of high-throughput sequence data
- Annotation of genomes
- Prediction of secreted proteins

### Overview of our Data Set

 *Escherichia coli* strain K-12 holds a key position as a model organism in studies of molecular biology, biochemistry, genetics and biotechnology. The sequence we will be using was cited by several [publications](https://www.ncbi.nlm.nih.gov/pmc/?term=ERX008638+or+ERR022075), especially in the context of genome assembly. Here, we will trim, assemble and annotate the genome of *E. coli* K-12 and will predict secreted proteins from it. 
 
In addition, we will also be using an already assembled genome of a *Staphylococcus aureus* strain "USA300* which is a [lineage often associated with methicillin resistance (MRSA)](https://www.ncbi.nlm.nih.gov/pmc/?term=Staphylococcus+aureus+%2B+USA300).

{% include links.md %}
