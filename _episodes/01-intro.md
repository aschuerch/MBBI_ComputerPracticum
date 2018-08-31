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
- "Describe the characteristics of the isolates."
keypoints:
- "After the first two days you will have some familiarity with working on the command line, data management, cleaning and visualization, automation and scripting"
---

The lessons of the **first two days** introduce and reinforce basic skills in the Unix shell and R, and are designed for learners with no programming experience. The general topics and skills covered include:

### Interacting with Computers
- Could Computing
- Connecting to remote computing (SSH/PuTTY)
- File Transfer (FileZilla, other command-line tools: scp, rsynch, wget, etc. )

### Data Management and Organization
- Unix Shell (command-line: ls, cd, mkdir, cp, rm, wc, grep, cut, columns, head, tail, less etc,)

### Data Cleaning and visualization
- FastQC - quality control of high-throughput sequence data
- Seqtk - trimming of low quality data

### Microbial Genomics
- Assembly
- Annotation
- Prediction of secreted proteins

## Overview of our Data Set and Narrative

TBD

Microbes are ideal organisms for exploring 'Long-term Evolution Experiments' (LTEEs) - thousands of generations can be generated and stored in a way that would be virtually impossible for more complex eukaryotic systems. In [Lenski et.al.](http://www.nature.com/nature/journal/v489/n7417/full/nature11514.html), 12 populations of *Escherichia coli* were propagated for more than 40,000 generations in a glucose-limited minimal medium. This medium was supplemented with citrate which *E. coli* cannot metabolize in the aerobic conditions of the experiment. Sequencing of the populations at regular time points reveals that spontaneous citrate-using mutants (Cit+) appeared in a population of *E.coli* (designated Ara-3) at around 31,000 generations. It should be noted that spontaneous Cit+ mutants are extraordinarily rare - inability to metabolize citrate is one of the defining characters of the *E. coli* species. Eventually, Cit+ mutants became the dominate population as the experimental growth medium contained a high concentration of citrate relative to glucose. 

{% include links.md %}
