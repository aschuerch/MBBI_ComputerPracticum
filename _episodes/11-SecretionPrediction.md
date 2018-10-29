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

# Protein secretion prediction

SignalP is a neural network–based method which can discriminate signal peptides from transmembrane regions. A signal peptide is the N-terminal part of a protein that is targeted to the secretory pathway in pro- and eukaryotes.  In prokaryotes, translocation takes place across the cytoplasmic membrane (inner membrane in Gram-negative bacteria), and the process can happen during or after translation.

However the presence of a signal peptide does not necessarily mean that the protein is secreted to the extracellular environment—it only means that it enters the secretory pathway.  The protein could have one or more transmembrane helices downstream of the signal peptide and therefore be retained in the membrane. In Gram-negative bacteria, the protein could be retained in the peri
plasm, or be inserted into the outer membrane as a β-barrel transmembrane protein. In Gram-positive bacteria, the protein could be attached to the cell wall. In general signal peptides have three regions: 
 - an N-terminal n-region of variable length characterized by positive charge
 - a central h-region of at least 7 hydrophobic residues\
 - a C-terminal c-region of typically 3-7 polar residues. 
 
Positions –1 and –3 relative to the cleavage site are occupied by small uncharged residues; in bacteria predominantly Alanine. SPs of Gram-positive bacteria tend to be longer than those of Gram-negative bacteria.


# Running signalP

SignalP takes amino acid sequences in fasta format. These are available from the annotation output as .faa files. Note that any letters not corresponding to the twenty standard amino acids, e.g. ‘U’, ‘B’, or ‘Z’, will be converted to ‘X’ and treated as unknown amino acids. Furthermore it is important to choose the correct organism group — Eukaryotes, Gram-negative 
bacteria, or Gram-positive bacteria with the -t option. The -m option will give a fasta output of the mature sequence (after cleavage of the signal peptide).

First, let's make a new folder

~~~
$ cd ~/112018_Secretome
$ mkdir prediction
~~~
{: .bash}

Then, lets run signalP on the *E.coli* proteins

~~~
$ cd ~/112018_Secretome/prediction
$ signalp -t gram- -m Ecoli.fasta  ~/112018_Secretome/annotation/ERR022075/ERR022075.faa
~~~
{: .bash}

> ## Exercise
> 
> Can you find proteins with a signal peptide in the *S.aureus* genome?
>
>
>> ## Solution
>> 
>>  
>> 
>> ~~~ 
>> $ cd ~/112018_Secretome/prediction
>> $ signalp -t gram+ -m Saureus.fasta  ~/112018_Secretome/annotation/S_aureus/S_aureus.faa
>> ~~~
>> {: .bash}
>> {: .output}
> {: .solution}
{: .challenge}





