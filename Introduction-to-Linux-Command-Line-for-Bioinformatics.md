# Introduction to Linux command line for bioinforamtics

## Goals

The goal of this tutorial is to provide hands-on training basics of using Linux via the command line. It addresses people who have no previous experience with Unix-like systems, or who know a few commands but would like to know more.  

## Prerequisites  

If you use a PC, please make sure you have [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) and [WinSCP](http://winscp.net/eng/download.php) installed on your computer.  

## Log on to CRI's HPC system

The login procedure varies slightly depending on whether you use a Windows computer or Mac/Unix/Linux computer.

**For Mac users**:

1. Open a terminal session
2. Connect to the login node of randi cluster:

`ssh username@randi.cri.uchicago.edu`

Enter the password when prompted. Type yes if you are prompted to accept a key.

**For Windows/PuTTY users**:

Open a PuTTY session, enter the name of the host: **randi.cri.uchicago.edu,** in the Host Name box, and select SSH as the Connection Type, verify the port number set in Port Box is 22.  Press the Open button at the bottom, then type in your username and password when prompted (note: type yes if you are prompted to accept a key before entering username).

![putty.jpg](https://biocore.cri.uchicago.edu/images/putty.jpg)

## Navigation

1. List files in your home directory

    ```bash
    ls            ### List non-hidden files in your current directory   
    ls -a         ### List all files in your current directory
    ```

2. Change current directory to the root of the file system and explore the directory structure.

    ```bash
    cd /          ### Change current directory to root of the file system
    ls
    ls -l         ### List files in long format
    pwd           ### Show the current directory
    cd /tmp       ### Change current directory to /tmp
    ls
    ls -l
    ```

3. Go back to your home directory

    ```bash

    cd ~          ### ~ is your home directory

    ls

    pwd
    ```

## Directory and file operations

1. Create a new directory

    ```bash

    mkdir mydir  ### Make a new directory

    mkdir folder3

    ls
    ```

2. Create a new file in a directory

    ```bash

    cd mydir

    nano file1.txt                   #### Use nano to create a new file, use Control O to save and Control-X to exit.

    ls
    ```

3. Copy a file on your current directory and copy a folder to your home directory.

    ```bash

    cp file1.txt file1_copy.txt      ### Copy file1.txt to file1_copy.txt

    ls

    cp -r /group/cri-training/linux .   ### . is your current directory

    ls

    mkdir temp

    cp *.txt temp                   ### Copy all .txt file to temp directory

    ls temp                         ### Verify the files were copied successfully
    ```

4. Delete a file or directory

    ```bash

    rm file1_copy.txt  ### Delete file1_copy.txt

    rm -r temp         ### This is the most dangerous command! You will delete the whole folder and there is no way you can recover it unless you have a backup.
    ```

5. Rename a file or folder

    ```bash

    mv file1.txt file12.txt    ### Rename file1.txt to file2.txt

    cd                         ### Back to your home directory. This is equivalent to cd ~

    mv mydir folder2           ### Rename mydir to folder2 if folder2 doesn't exist, otherwise, move mydir under folder2.
    ```

6. Move file from one folder to another

    ```bash

    mv folder2/file12.txt folder3   ### Move file12.txt in folder2 to folder3

    ls -l folder3

    ls *
    ```

7. Compress files

    ```bash
    cp -r /gpfs/data/cri-training/linux ~/folder2  ### copy workshop training material to folder2
    cd ~/folder2/linux

    ls -l SRR*                     ### * is a wildcard to match any strings

    gzip SRR001655.fastq           ### Compress a file

    gunzip SRR001655.fastq.gz      ### Decompress a file
    ```

## File transfer between computers

1. From PC: use WinSCP ([http://winscp.net/eng/index.php](http://winscp.net/eng/index.php))
2. From MAC: use scp in terminal

    ```bash
    scp example1.txt username@randi.cri.uchicago.edu:.
    ```

3. You can use wget to get a file from a website directly on your Linux.

    ```bash
    wget https://ftp.ensembl.org/pub/release-110/gtf/saccharomyces_cerevisiae/Saccharomyces_cerevisiae.R64-1-1.110.gtf.gz
    ```

## I/O redirection and pipe

In Linux a terminal by default contains three streams, one for input, and two output-based streams. The input stream is referred to as **_stdin_**, and is generally mapped to the keyboard (other input devices may be used, or could be piped from another process). The standard output stream is referred to as **_stdout_**, and generally prints to the terminal, or output may be consumed by another process (as **_stdin_**). The other output stream **_stderr_** primarily used for status reporting usually prints to the terminal like stdout. As each of these streams has its own file descriptor, each may be piped or redirected separately from the other, even if they are all connected to the terminal. File descriptors for each of these streams are:

* stdin = 0
* stdout = 1
* stderr = 2

These streams can be piped and redirected to files or other processes. This construct is commonly referred to as building a pipeline.

1. \> file, Output re-direction, overwrite

    ```bash
    nano file1.txt                       ### Create a new file and put some contents.

    cat file1.txt                        ### Print file1.txt to screen

    cat file1.txt > file2.txt            ### Print file1.txt to file2.txt

    nano file2.txt                       ### Use nano to view file2.txt
    ```

2. \>> file, Output re-direction, append

    ```bash
    cat file1.txt >> file2.txt           ### Append file1.txt to file2.txt

    nano file2.txt                       ### View file2.txt
    ```

3. < file, input re-direction

    ```bash
    cat < file1.txt                      ### Print file1.txt to screen
    ```

4. CommandA | command B, pipe output from CommandA to command B

    ```bash
    ls | wc -l           ### List the files and folders in the current directory and count the numbers
    ```

## Text extraction and manipulation

1. Text Editor: vi, vim, nano, emacs, and others.
2. Text Viewers:less (more), head, and tail

    ```bash
    cd ~/folder2/linux                   ### Change current directory to ~/folder2/Linux

    less SRR001655.fastq                 ### View a fastq file: use q to exit, space or f to the next page,

    ### b to the previous page, and / to search a word

    head -20 SRR001655.fastq             ### Show the first 20 line of the file

    tail -20 SRR001655.fastq             ### Show the last 20 lines of the file
    ```

3. Pattern Search: grep  
    The grep command searches specified files or stdin for patterns matching a given expression(s).

    ```bash
    > cat list1.txt                      ### See what's in the list1.txt
    
    apples
    bananas
    plums
    carrots

    > cat list2.txt                      ### See what's in the list2.txt

    Apple Sauce
    wild rice
    black beans
    kidney beans
    dry apples

    > grep apple list1.txt list2.txt     ### Search for "apple" in list1.txt and list2.txt

    list1.txt:apples
    list2.txt:dry apples
    ```

4. Find a file: find

    ```bash
    find . -name "*.txt"            ### Find all the files whose names ended with ".txt" under the current directory

    find . -mtime 0                 ### Find all the files modified today under the current directory

    find . -mtime +3                ### Find all the files modified more than 3 days ago under the current directory

    find . -mmin -30                ### Find all the files modified in the last 30 minutes

    find . -type d -name "ex*"      ### Find all the folders whose name starts with ex
    ```

5. Text replacement and text operation: cat, sed, tr, and rev

    ```bash
    > cat list2.txt                                ### See what's in the list2.txt
    Apple Sauce
    wild rice
    black beans
    kidney beans
    dry apples

    > cat list2.txt | sed 's/bean/button/g'       ### Replace bean with button
    Apple Sauce
    wild rice
    black buttons
    kidney buttons
    dry apples

    > cat list2.txt | tr /a-z/ /A-Z/              ### Change lower case to upper case
    APPLE SAUCE
    WILD RICE
    BLACK BEANS
    KIDNEY BEANS
    DRY APPLES

    > echo 'attgctcgat' | tr /atgc/ /tacg/ | rev  ### Find a complement DNA sequence and then reverse it
    atcgagcaat
    ```

6. Table manipulation: sort, uniq, cut, awk, and paste

    ```bash
    > cat table.txt                                      ### show the contents on table.txt
    CHR	SNP	BP	A1	C_A	C_U	A2	CHISQ	P	OR
    19	rs10401969	19268718	C	222	890	T	0.03462	0.8524	0.9857
    1	rs10873883	76734548	G	934	3811	A	0.5325	0.4656	0.9691
    1	rs11589256	214196749	C	271	1084	T	0.01928	0.8896	0.9902
    15	rs10401369	19268718	C	232	890	T	0.03232	0.2524	0.1157
    11	rs10873487	767334548	G	964	3811	A	0.5525	0.2356	0.2391

    ###If the header line is not aligned well with the contents. Please try this:

    > cat table.txt | column -t
    CHR  SNP         BP         A1  C_A  C_U   A2  CHISQ    P       OR
    19   rs10401969  19268718   C   222  890   T   0.03462  0.8524  0.9857
    1    rs10873883  76734548   G   934  3811  A   0.5325   0.4656  0.9691
    1    rs11589256  214196749  C   271  1084  T   0.01928  0.8896  0.9902
    15   rs10401369  19268718   C   232  890   T   0.03232  0.2524  0.1157
    11   rs10873487  767334548  G   964  3811  A   0.5525   0.2356  0.2391

    > sort -k1n table.txt  > table_sorted1.txt         ### sort the table by the first column in numerical order
                                                       ### and write the results to a new file
    > cat table_sorted1.txt
    CHR	SNP	BP	A1	C_A	C_U	A2	CHISQ	P	OR
    1	rs10873883	76734548	G	934	3811	A	0.5325	0.4656	0.9691
    1	rs11589256	214196749	C	271	1084	T	0.01928	0.8896	0.9902
    11	rs10873487	767334548	G	964	3811	A	0.5525	0.2356	0.2391
    15	rs10401369	19268718	C	232	890	T	0.03232	0.2524	0.1157
    19	rs10401969	19268718	C	222	890	T	0.03462	0.8524	0.9857

    > sort -k2 table.txt                               ### sort the table by the second column
    15	rs10401369	19268718	C	232	890	T	0.03232	0.2524	0.1157
    19	rs10401969	19268718	C	222	890	T	0.03462	0.8524	0.9857
    11	rs10873487	767334548	G	964	3811	A	0.5525	0.2356	0.2391
    1	rs10873883	76734548	G	934	3811	A	0.5325	0.4656	0.9691
    1	rs11589256	214196749	C	271	1084	T	0.01928	0.8896	0.9902
    CHR	SNP	BP	A1	C_A	C_U	A2	CHISQ	P	OR

    ####Note that the header goes to the wrong place, let's fix it.

    > awk 'NR==1;NR>1{print $0 | "sort -k2"}' table.txt
    CHR	SNP	BP	A1	C_A	C_U	A2	CHISQ	P	OR
    15	rs10401369	19268718	C	232	890	T	0.03232	0.2524	0.1157
    19	rs10401969	19268718	C	222	890	T	0.03462	0.8524	0.9857
    11	rs10873487	767334548	G	964	3811	A	0.5525	0.2356	0.2391
    1	rs10873883	76734548	G	934	3811	A	0.5325	0.4656	0.9691
    1	rs11589256	214196749	C	271	1084	T	0.01928	0.8896	0.9902

    > cut -f1,2,3,5 table.txt       ####Extract columns 1, 2, 3, and 5 from table.txt
    CHR	SNP	BP	C_A
    19	rs10401969	19268718	222
    1	rs10873883	76734548	934
    1	rs11589256	214196749	271
    15	rs10401369	19268718	232
    11	rs10873487	767334548	964

    ###The paste command is used to align files side-by-side, merging records from each file respectively.

    > paste table.txt list1.txt | column -t
    CHR  SNP         BP         A1  C_A  C_U   A2  CHISQ    P       OR      apples
    19   rs10401969  19268718   C   222  890   T   0.03462  0.8524  0.9857  bananas
    1    rs10873883  76734548   G   934  3811  A   0.5325   0.4656  0.9691  plums
    1    rs11589256  214196749  C   271  1084  T   0.01928  0.8896  0.9902  carrots
    15   rs10401369  19268718   C   232  890   T   0.03232  0.2524  0.1157
    11   rs10873487  767334548  G   964  3811  A   0.5525   0.2356  0.2391

    > cat p1.txt           ### See the contents in p1.txt
    IBM
    MSFT
    Apple
    SAP
    Yahoo

    > cat p2.txt          ### See the contents in p2.txt
    25.23
    234.02
    23.03
    11.22
    15.8

    > paste p1.txt p2.txt   ### What happens when I paste p1.txt and p2.txt?
    IBM	25.23
    MSFT	234.02
    Apple	23.03
    SAP	11.22
    Yahoo	15.8

    > paste -d":" p1.txt p2.txt  ###Delimited the fields by ":"
    IBM:25.23
    MSFT:234.02
    Apple:23.03
    SAP:11.22
    Yahoo:15.8
    ```

7. Count the number of word, lines and bytes: wc

    > wc -l list1.txt list2.txt              ###Count the number of lines in list1.txt and list2.txt

    4 list1.txt

    5 list2.txt

    9 total

    > wc -w list2.txt                        ###Count the number of words in list2.txt

    10 list2.txt

    > wc list2.txt                           ###There are 5 lines, 10 words, 58 bytes in list2.txt

    5 10 58 list2.txt

## Shell Script

```bash
nano hello.sh

# !/bin/bash  

echo "Hello World!."

echo "I am going to generate 10 files: file1.txt, file2.txt, ..., file10.txt"  
for i in {1..10}  
do 
    echo "this is file $i" > file$i.txt  
done  
echo "Done"

chmod u+x hello.sh         ### Make your shell script executable

./hello.sh                 ### Execute your first shell script

ls file*                   ### List the files generated

```

* * *

## Bioinformatics examples

So, we have seen an introduction to Linux, now how is this relevant to bioinformatics? Here are some brief examples that show the power of using Linux.

### Bioinformatics example 1: use Linux command line to process FASTQ file

1. The FASTQ format  
    The majority of NGS data are now provided in FASTQ format. To check the detail of the format and how quality values are encoded in the file, please visit[http://en.wikipedia.org/wiki/FASTQ\_format](http://en.wikipedia.org/wiki/FASTQ_format). For example:

    @SRR001665.1 071112_SLXA-EAS1_s_4:1:1:672:654/1
    GCTACGGAATAAAACCAGGAACAACAGACCCAGCAC
    +
    IIIIIIIIIIIIIIIIIIIIIIIIEII9IIIEIIII

    **This is just one read**. The four lines are:

    line 1: @SRR001665.1...- a unique identifier for the sequence read

    line 2: GCTACGGAA.... - the sequence read

    line 3: + - a separator between the read and the quality values (this sometimes replicates the sequence ID)

    line 4: IIIIIIIIIIIIIIIIIII.... - the quality values. For more information, see [http://en.wikipedia.org/wiki/FASTQ_format#Encoding]

2. Working with FASTQ on the command line\*  
    Let's view the file using **less**: `<space>` or f to the Next Page; b to the Previous Page; q to Exit;

    ```bash 
    less SRR001655.fastq
    ```

    How many reads are in file SRR001655.fastq? (Hint: use cat, grep and wc)

    ```bash
    cat SRR001655.fastq | grep ^@SRR | wc -l
    ```

    or

    ```bash
    grep ^@SRR SRR001655.fastq | wc -l
    ```

    Show me the first 25 reads in file SRR001655.fastq? (Hint: use head)

    ```bash
    head -n 100 SRR001655.fastq
    ```

    or

    ```bash
    head -100 SRR001655.fastq
    ```

    Show me all the sequences that contains GAGAGAGC in file SRR001655.fastq? (Hint: use grep)

    ```bash
    grep GAGAGAGC SRR001655.fastq
    ```

    Write the last 10000 reads to a new file bottom\_10000.fastq? (Hint: use tail)

    ```bash
    tail -40000 SRR001655.fastq > bottom_10000.fastq
    ```

3. Using paste to get your data into a format you may prefer  
    FASTQ format is the format of data that all bioinformatics tools prefer and understand, however, sometimes it is preferable to look at data as a table i.e. in columns.  We can do this with the **paste** command. The **paste** command writes lines in one file out as columns, separated by a tab character. The command can take "-" as an option, which means read from stdin. So if we give it the option "- - - -", this means "read four lines, and write it them out as four columns). Let's take a look:
    ```bash
    cat bottom_10000.fastq | paste - - - - | head -10`

    cat bottom_10000.fastq | paste - - - - | head -1000 > top_1000_tab.txt  
    ```


### Using awk and working with data in columns  

Now that we have some data in tabular format, we can look at using **awk**.

```bash 
awk '/search pattern/{Actions}' filename
```

Briefly, awk goes through each line in _filename_ and if the line matches the search pattern, the action is performed. Let's see some examples:

```bash
awk '/N/{print}' top_1000_tab.txt
```

This simply prints out all lines in the file that contain an N - could be useful, but we can do that with **grep** anyway.  What happens when you execute:

```bash
awk '/N/{print $1,"\t",$2,"\t",$3,"\t",$4}' top_1000_tab.txt
```

What do you think the $1, $2 etc mean? How about the "\\t"? Instead, try this:

```bash
awk '/N/{print $1,"\t",$3}' top_1000_tab.txt
```

We can use these in the match operation e.g.

```bash
awk '$3 ~ /N/{print $1,"\t",$3}' top_1000_tab.txt
```

Up until now, we were asking the question "Does any part of the line contain an N?".  In this last case, we are asking the question: for each line, does the data in column 3 contain an N? Note column 3 is the sequence, and so you may be interested in which lines have sequences that contain N's:

```bash
awk '$3 ~ /N/{print $1}' top_1000_tab.txt
```

Identifies which reads contains N's.

```bash
awk '$3 ~ /N/ {print $1}' top_1000_tab.txt | wc -l
```

Counts the number of reads that contain N's.  Note we can also do this by piping data into awk e.g.

```bash
cat top_1000_tab.txt | awk '$3 ~ /N/{print $1}' | wc -l
```

Putting it together into something useful  
How about a quick fastq to fasta converter?

```bash
cat SRR001655.fastq | paste - - - - | awk '{print ">"$1,$2,"\n"$3}'
```

This is very crude, but in brief: we print the file to the stream, which we pipe into paste to convert to tabular data. We pipe this into awk. As we don't use a search pattern, awk accepts every line. First we print out the ">", and then columns 1 and 2 to make up the identifier. We then print out a new line ("\\n") and then finally the third column which is the sequence.

## Feedback:


Please provide feedback to us [https://biocore.cri.uchicago.edu/cgi-bin/survey.cgi?id=72](https://biocore.cri.uchicago.edu/cgi-bin/survey.cgi?id=72). 