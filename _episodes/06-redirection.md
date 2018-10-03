---
title: "Redirection"
teaching: 30
exercises: 15
questions:
- "How can I search within files?"
- "How can I combine existing commands to do new things?"
- "How can I get rid of sequence data that doesn’t meet my quality standards?"
objectives:
- "Employ the `grep` command to search for information within files."
- "Print the results of a command to a file."
- "Construct command pipelines with two or more stages."
- "Clean FASTQ reads using seqtk"
keypoints:
- "`grep` is a powerful search tool with many options for customization."
- "`>`, `>>`, and `|` are different ways of redirecting output."
- "`command > file` redirects a command's output to a file."
- "`command >> file` redirects a command's output to a file without overwriting the existing contents of the file."
- "`command_1 | command_2` redirects the output of the first command as input to the second command."
---

## Searching files

We discussed in a previous episode how to search within a file using `less`. We can also
search within files without even opening them, using `grep`. `grep` is a command-line
utility for searching plain-text files for lines matching a specific set of 
characters (sometimes called a string) or a particular pattern 
(which can be specified using something called regular expressions). We're not going to work with 
regular expressions in this lesson, and are instead going to specify the strings 
we are searching for.
Let's give it a try!

> ## Nucleotide abbreviations
> 
> The four nucleotides that appear in DNA are abbreviated `A`, `C`, `T` and `G`. 
> Unknown nucleotides are represented with the letter `N`. An `N` appearing
> in a sequencing file represents a position where the sequencing machine was not able to 
> confidently determine the nucleotide in that position. You can think of an `N` as a `NULL` value
> within a DNA sequence. 
> 
{: .callout}

Suppose we want to see how many reads in our file have really bad segments containing 10 consecutive unknown nucleoties (Ns). Let's search for the string NNNNNNNNNN in the SRR098026 file.

> ## Determining quality
> 
> In this lesson, we're going to be manually searching for strings of `N`s within our sequence
> results to illustrate some principles of file searching. It can be really useful to do this
> type of searching to get a feel for the quality of your sequencing results, however, in you 
> research you will most likely use a bioinformatics tool that has a built-in program for
> filtering out low-quality reads. You'll learn how to use one such tool in 
> [a later lesson](http://www.datacarpentry.org/wrangling-genomics/00-readQC/).
> 
{: .callout}

~~~
$ grep NNNNNNNNNN SRR098026.fastq
~~~
{: .bash}

This command returns a lot of output to the terminal. Each line in the SRR098026 
file which contains at least 10 consecutive Ns is printed to the terminal. We may be 
interested not only in the actual sequence which contains this string, but 
in the name (or identifier) of that sequence. We discussed in a previous lesson 
that the identifier line immediately precedes the nucleotide sequence for each read
in a FASTQ file. We may also want to inspect the quality scores associated with
each of these reads. To get all of this information, we will return the line 
immediately before each match and the two lines immediately after each match.

We can use the `-B` argument for grep to return a specific number of lines before
each match and the `-A` argument to return a specific number of lines after each matching line. Here we want the line before and the two lines after each 
matching line so we add `-B1 -A2` to our grep command.

~~~
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq
~~~
{: .bash}

One of the sets of lines returned by this command is: 

~~~
@SRR098026.177 HWUSI-EAS1599_1:2:1:1:2025 length=35
CNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
+SRR098026.177 HWUSI-EAS1599_1:2:1:1:2025 length=35
#!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
~~~
{: .output}

> ## Exercise
>
> 1) Search for the sequence `GNATNACCACTTCC` in the `SRR098026.fastq` file.
> Have your search return all matching lines and the name (or identifier) for each sequence
> that contains a match.
> 
> 2) Search for the sequence `AAGTT` in both FASTQ files.
> Have your search return all matching lines and the name (or identifier) for each sequence
> that contains a match.
> 
> > ## Solution  
> > 1) `grep -B1 GNATNACCACTTCC SRR098026.fastq`  
> > 2) `grep -B1 AAGTT *.fastq`
> >
> {: .solution}
{: .challenge}

## Redirecting output

`grep` allowed us to identify sequences in our FASTQ files that match a particular pattern. 
But all of these sequences were printed to our terminal screen. In order to work with these 
sequences and perform other opperations on them, we will need to capture that output in some
way. 

We can do this with something called "redirection". The idea is that
we're redirecting what was output to the terminal to another location. 
In our case, we want to print this information to a file, so that we can look at it later and 
do other analyses with this data.

The command for redirecting output to a file is `>`.

Let's try out this command and copy all the records (including all four lines of each record) 
in our FASTQ files that contain 
'NNNNNNNNNN' to another file called 'bad_reads.txt'.

~~~
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq > bad_reads.txt
~~~
{: .bash}

> ## File extensions
> 
> You might be confused about why we're naming our output file with a `.txt` extension. After all,
> it will be holding FASTQ formatted data that we're extracting from our FASTQ files. Won't it 
> also be a FASTQ file? The answer is, yes - it will be a FASTQ file and it would make sense to 
> name it with a `.fastq` extension. However, using a `.fastq` extension will lead us to problems
> when we move to using wildcards later in this episode. We'll point out where this becomes
> important. For now, it's good that you're thinking about file extensions! 
> 
{: .callout}


The prompt should sit there a little bit, and then it should look like nothing
happened. But type `ls`. You should see a new file called bad_reads.txt. 

We can check the number of lines in our new file using a command called `wc`. 
`wc` stands for `word count`. This command counts the number of words, lines, and characters
in a file. 

~~~
$ wc bad_reads.txt
~~~
{: .bash}

~~~
  537  1073 23217 bad_reads.txt
~~~
{: .output}

This will tell us the number of lines, words and characters in the file. If we
want only the number of lines, we can use the `-l` flag for `lines`.

~~~
$ wc -l bad_reads.txt
~~~
{: .bash}

~~~
537 bad_reads.txt
~~~
{: .output}

Because we asked `grep` for all four lines of each FASTQ record, we need to divide the output by
four to get the number of sequences that match our search pattern.

> ## Exercise
>
> How many sequences in `SRR098026.fastq` contain at least 3 consecutive Ns?
>
>> ## Solution
>> We can do it in one step, using the `|` pipe:  
>>
>> ~~~
>> $ grep NNN SRR098026.fastq | wc -l
>> ~~~
>> {: .bash}
>> 
>> ~~~
>> 249
>> ~~~
>> {: .output}
>>
> {: .solution}
{: .challenge}

We might want to search multiple FASTQ files for sequences that match our search pattern.
However, we need to be careful, because each time we use the `>` command to redirect output
to a file, the new output will replace the output that was already present in the file. 
This is called "overwriting" and, just like you don't want to overwrite your video recording
of your kid's first birthday party, you also want to avoid overwriting your data files.

~~~
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq > bad_reads.txt
$ wc -l bad_reads.txt
~~~
{: .bash}

~~~
537 bad_reads.txt
~~~
{: .output}

~~~
$ grep -B1 -A2 NNNNNNNNNN SRR097977.fastq > bad_reads.txt
$ wc -l bad_reads.txt
~~~
{: .bash}

~~~
0 bad_reads.txt
~~~
{: .output}

Here, the output of our second  call to `wc` shows that we no longer have any lines in our bad_reads.txt file. This is 
because the second file we searched (`SRR097977.fastq`) does not contain any lines that match our
search sequence. So our file was overwritten and is now empty.

We can avoid overwriting our files by using the command `>>`. `>>` is known as the "append redirect" and will 
append new output to the end of a file, rather than overwriting it.

~~~
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq > bad_reads.txt
$ wc -l bad_reads.txt
~~~
{: .bash}

~~~
537 bad_reads.txt
~~~
{: .output}

~~~
$ grep -B1 -A2 NNNNNNNNNN SRR097977.fastq >> bad_reads.txt
$ wc -l bad_reads.txt
~~~
{: .bash}

~~~
537 bad_reads.txt
~~~
{: .output}

The output of our second call to `wc` shows that we have not overwritten our original data. 

We can also do this with a single line of code by using a wildcard. 

~~~
$ grep -B1 -A2 NNNNNNNNNN *.fastq > bad_reads.txt
$ wc -l bad_reads.txt
~~~
{: .bash}

~~~
537 bad_reads.txt
~~~
{: .output}

> ## File extensions - part 2
> 
> This is where we would have trouble if we were naming our output file with a `.fastq` extension. 
> If we already had a file called `bad_reads.fastq` (from our previous `grep` practice) 
> and then ran the command above using a `.fastq` extension instead of a `.txt` extension, `grep`
> would give us a warning. 
> 
> ~~~
> grep -B1 -A2 NNNNNNNNNN *.fastq > bad_reads.fastq
> ~~~
> {: .bash}
> 
> ~~~
> grep: input file ‘bad_reads.fastq’ is also the output
> ~~~
> {: .output}
> 
> `grep` is letting you know that the output file `bad_reads.fastq` is also included in your
> `grep` call because it matches the `*.fastq` pattern. Be careful with this as it can lead to
> some surprising output.
> 
{: .callout}

So far we've searched for reads containing a long string of at least 10 unknown nucleotides. 
We might also be interested in finding any reads with at least two shorter strings of 5 unknown 
nucleotides, separated by any number of nucleotides. Reads with more than one region of 
ambiguity like this might be poor enough to not pass our quality filter. We can search for these
reads using a wildcard within our search string for `grep`. 

> ## Exercise
> 
> How many reads in the `SRR098026.fastq` file contain at least two regions of 5 unknown
> nucleotides in a row, separated by any number of nucleotides?
>
>> ## Solution
>> 
>> ~~~
>> $ grep "NNNNN*NNNNN" SRR098026.fastq > bad_reads_2.txt
>> $ wc -l bad_reads_2.txt
>> ~~~
>> {: .bash}
>> 
>> ~~~
>> 186 bad_reads_2.txt
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}


We've now created two separate files to store the results of our search for reads matching 
particular criteria. Since we might have multiple different criteria we want to search for, 
creating a new output file each time has the potential to clutter up our workspace. We also
so far haven't been interested in the actual contents of those files, only in the number of 
reads that we've found. We created the files to store the reads and then counted the lines in 
the file to see how many reads matched our criteria. There's a way to do this, however, that
doesn't require us to create these intermediate files - the pipe command (`|`).

This is probably not a key on
your keyboard you use very much, so let's all take a minute to find that key. 
What `|` does is take the output that is
scrolling by on the terminal and uses that output as input to another command. 
When our output was scrolling by, we might have wished we could slow it down and
look at it, like we can with `less`. Well it turns out that we can! We can redirect our output
from our `grep` call through the `less` command.

~~~
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq | less
~~~
{: .bash}

We can now see the output from our `grep` call within the `less` interface. We can use the up and down arrows 
to scroll through the output and use `q` to exit `less`.

Redirecting output is often not intuitive, and can take some time to get used to. Once you're 
comfortable with redirection, however, you'll be able to combine any number of commands to
do all sorts of exciting things with your data!

None of the command line programs we've been learning
do anything all that impressive on their own, but when you start chaining
them together, you can do some really powerful things very
efficiently. Let's take a few minutes to practice. 

> ## Exercise
>
> Now that we know about the pipe (`|`), write a single command to find the number of reads 
> in the `SRR098026.fastq` file that contain at least two regions of 5 unknown
> nucleotides in a row, separated by any number of known nucleotides. Do this without creating 
> a new file.
>
>> ## Solution
>> 
>> ~~~
>> $ grep "NNNNN*NNNNN" SRR098026.fastq | wc -l
>> ~~~
>> {: .bash}
>>
>> ~~~
>> 186
>> ~~~
>> {: .output}
>> 
> {: .solution}
{: .challenge}

## Trimming and more practice with pipes

Let's use the tools we've added to our tool kit so far, along with a few new ones, to trim our short read sequences. First, let's navigate to the correct directory.

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

Now we will run seqtk trimfq on our data. To begin, navigate to your `untrimmed_fastq` data directory:

~~~
$ cd ~/dc_workshop/data/
~~~
{: .bash}

We are going to run seqtk on one sample giving it an error rate threshold of 0.01 which indicates the base call accuracy. We request that, after trimming, the chances that a base is called incorrectly are only 1 in 10000.

~~~
$ seqtk trimfq -q 0.01 ERR022075_1.fastq.gz > ERR022075_1.trimmed.fastq
~~~
{: .bash}

Notice that we needed to redirect the output to a file. If we don't do that, the trimmed fastq data will be displayed in the console.


> ## Exercise
> Trim the second read pair with the same parameters and write into a new file
> 
> 
> > ## Solution
> > 
> > `$ seqtk trimfq -q 0.01 ERR022075_2.fastq.gz > ERR022075_2.trimmed.fastq`
> >
> {: .solution}
{: .challenge}

{% include links.md %}
