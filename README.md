Bioinformatics One Liners
=========================

This repository collects and explains the `bash` one-liners I tweeted using the [#BioinformaticsOneLiner hash tag](https://twitter.com/search?f=realtime&q=%23bioinformaticsoneliner&src=typd). The idea behind this project is to demonstrate how to perform many basic bioinformatics tasks using widely available shell utilities like `awk` and `sed`.

**Note**: Although many (most?) bioinformaticians use some flavor of Linux for their work, I've tried to make these one-liners as POSIX-ly correct as possible, which basically means avoiding GNU-specific extensions like the ~ "operator" in GNU `sed`. POSIX correctness makes some of these one-liners slightly more verbose than their GNU counterparts, but doing so makes them usable on systems like OS X, where the default versions of utilities like `awk` and `sed` lack the GNU extensions.


One Liners
----------

#### Get records identifiers from a FASTQ file
**Warning** A naïve approach would be using `grep` to find the `@` character that starts each FASTQ record; i.e.,
```bash
grep -v '^@' foo.fq
```
However, this does *not* work because the `@` symbol can also appear as the first character in a quality string. A better approach would be:
```bash
awk 'NR%4==1' foo.fq
```

#### Reverse complement a sequence
Given the sequence string `seq`, print its reverse complement:
```bash
echo <seq> | tr AGCT TCGA | rev
```
*Note*: this only works if `seq` is contained entirely on one line (i.e., does not have any line breaks).


#### Number the header fields in a TSV file
Given a tab-separated file that has the header:
<pre>
col1&lt;tab&gt;col2&lt;tab&gt;...&lt;tab&gt;col&lt;N&gt;
</pre>
number its column names like so:
<pre>
1 col2
2 col2
...
N col&lt;N&gt;
</pre>
```bash
head -n1 <foo> | tr $'\t' '\n' | cat -n
```
*Note*: [Yaniv Erlich's tweet](https://twitter.com/erlichya/status/460911423166885888) provided the inspiration for this one-liner, although I have modified it to be more POSIX compliant.


#### Directly untar onto a remote system
```bash
ssh host 'tar -C dest_path -xzf -' < foo.tar.gz
```


#### Join consecutive lines in a file:
Given a file:
<pre>
A1
B1
A2
B2
...
A&lt;N&gt;
B&lt;N&gt;
</pre>
print
<pre>
A1&lt;tab&gt;B1
A1&lt;tab&gt;B1
...
A&lt;N&gt;&lt;tab&gt;B&lt;N&gt;
</pre>
```bash
paste - - < foo
```


#### Split FASTQ file into 5 files
Useful for perform basic data-level parallelization (e.g., when mapping reads)
```bash
awk 'NR%4==1{ f=(FILENAME)"."(++i%5); }{ print >> f }' foo.fq
```
