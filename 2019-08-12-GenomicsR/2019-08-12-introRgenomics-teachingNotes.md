# 2019-08-12 Intro to R for Genomics Teaching Notes

This Markdown document contains teaching notes for the Introduction to R for Genomics lessons for working with the RStudio server using Cyverse. 

### Creating projects

Can save data, variables, packages, scripts for a specific analysis project in a neat place, restart work where you started off, and can collaborate > connect with github, share environment with others.

To create a project, select new directory, create a directory name called dc_genomics_r, default puts it in the home directory.

Make directories for scripts and data, within data raw_data and cleaned_data. copy over important data files to the respective directories. 

If on the Cyverse server, can copy it over from the r_data folder, if using your local computer, have to download it from figshare

Cyverse instructions: 
1. From the home directory, ls
2. Create a directory called dc_genomics_r in your home directory, ~

Locally: 
1. Create a proejct called dc_genomics_r that is on your desktop

For both, cd into the dc_genomics_r direcotry, and make directories called scripts and data. Wherever your combined_tidy_vcf.csv file is, copy it into the data folder of your new directory/project. 

Put green flags up when have this done. 

You can navigate direcotories or set directories within R with `getwd()` and `setwd()`.

### 4 panels of RStudio

1. Environment - variables, history of code run
2. Files/plots/packages/can also find help on a function or package
3. Console/terminal - the console runs your code, shows error messages, terminal connects to your shell terminal of the working directory, can run bash
4. create a new script, file > new script, top left panel is the source panel

Create a new script called `genomics_r_basics.R`. 

### Comments

```
"Your closest collaborator is you six months ago, 
	but you don’t reply to emails.
		- Mark Holder
			- Karl Broman"
```

It's nice to leave yourself helpful tips for why you wrote your code a certain way - you won't remember in 6 months to a year when you go to rerun the script and prepare your final analyses/figures for a publication. 

Leave yourself comments starting with the pound/hash/hashtag symbol `#`. Anything after that symbol will be ignored by R and won't run it as code. 

### Creating Objects in R

An object is given a name, is assigned a value with the assignment operator. Such as the object "a" is given the value "1" with `a <- 1`. Run this line by either hitting the "Run" button at the top of your source panel, or press on a Windows Ctrl + Enter or on a Mac Cmd + Enter. In the environment, you'll have a new object named a, with the value of 1. 

### Exercise 1: 

1. Create an object with the value of number of pairs of human chromosomes (23)
2. Create an object that has a value of your favorite gene name (ppk1)
3. Create an object that has this URL as the value: "ftp://ftp.ensemblgenomes.org/pub/bacteria/release-39/fasta/bacteria_5_collection/escherichia_coli_b_str_rel606/" (quotes are essential for a character string)
4. Object with the value of the number of chromosomes in a diploid human cell, try to incorporate the object you made in task 1 (2 * human_chr_number, or total of 46)

### Tips for naming objects

Avoid spaces and special characters, use short and easy to understand names, but avoid commonly names, don't use the names of functions if you can try, use the assignment operator

### Object data types

In R, every object has two properties: 

1. Length - how many distinct values are held in that object
2. Type - what is the classification/type of the object

Numeric - 1,2, double, int- integer
Character - sequence of letters/numbers in single or double quotes
Logical - boolean values such as TRUE or FALSE or T or F

### Mathematical operations

Can perform simple mathematical operations such as addition, subtraction, multiplication

```
human_chr_number <- 26
human_chr_number * 2
```

### Functions

Such as mathemetical operations sqrt(), round() of numbers, can also find help on specific functions for base R and functions in the help panel

### Vectors

A collection of values that are all of the ame type - numbers, characters. Create a vector with the `c()` function - which means concatenate or combine. Inside the function you enter one or more values, for multiple values separate with a comma

```
snp_genes <- c("OXTR", "ACTN3", "AR", "OPRM1")
mode(snp_genes)
length(snp_genes)
str(snp_genes)
```

Create a couple more vectors and practice subsetting, to retrieve one or more specific values from the vector, using bracket notation (squares)

```
snps <- c('rs53576', 'rs1815739', 'rs1799971')
snp_chromosomes <- c('3','11','X','6')
snp_positions <- c(8762685,66506235,154039662)
# get the 3rd value in the snp_genes vector
snp_genes[3]
# get 1st through 3rd value of snp_genes to retrieve a range of the vector
snp_genes[1:3]
# retrieve several but not a sequential range of items from your vector, pass a vector of the indices with the numbered positions you want to retrieve
snp_genes[c(1,3,4)]
```

Add, remove, or replace values to a vector
Add the gene CYP1A1 and APOA5 to our list of snp genes
```
snp_genes <- c(snp_genes, "CYP1A1", "APOA5")
```
To remove an item from the vector, can use the minus sign and the indece/position of that item in the vector
```
snp_genes[-6]
snp_genes <- snp_genes[-6]
# explicitly rename or add a value to the index with double bracket notation
snp_genes[7] <- "APOA5"
```

### Logical Subsetting

Retrieve items in a vector based on logical evaluation/numerical comparison. Want to get all SNPs in our vector that of SNP positions that are greater than 100,000,000. Use the greater than sign
```
snp_positions[snp_positions > 1000000000]
```

### Missing values

Check if any values of your vector are missing = NA, can use the is.NA() function with `is.na(snp_genes)`. 

### Checking values

Want to find out if a specific value is present in a vector, with the comparison operator `%in%`, will return TRUE for any value in the collection that is in the vector you are searching

Test if ACTN3 or APO5A is in the snp_genes vector
```
c("ACTN3", "APOA5") %in% snp_genes
```

### Exercise 2
Create a version of snp_genes vector with the following changes: 
1. Does not contain CYP1A1
2. Add 2 NA values ot the end of snp_genes
```
snp_genes <- snp_genes[-5]
snp_genes <- c(snp_genes, NA,NA)
```

### Exercise 3 
Create a new vector named combined that contains: 
1. The first value in snp_genes
2. The first values in snps
3. The first value in snp_chromosomes
4. The first value in snp_positions
5. What type of data is combined?
```
combined <- c(snp_genes[1], snps[1], snp_chromosomes[1], snp_positions[1])
typeof(combined)
# character
```

*lists are bonus material and not sure really essential*

### Working with data and loading data in to R

Make sure you are in your directory dc_genomics_r, check with `getwd()`, and that the combined_tidy_vcf.csv file is within a folder called data in your project repository. Now we are going to work with our SNP data that we know the basic building blocks of R. 

Read in a CSV file and save it as an object called variants. Tab completion also works here. Look at data with view, see number of observations and columns in the environment panel. 

### Summarzing the structure of a data frame
```
variants <- read.csv("data/combined_tidy_vcf.csv")
summary(variants)
```
29 variables, get 29 fields that summarize the data. str for structure of the data
```
str(variants)
```

Objet data.frame is displayed along with dimensions, 801 observations, and 29 variables/columns, each variable or column has a name, fololowed by an object of a certain type - numberic, factor, string etc. Before each variable name there is a $, and this is how you can access variable/column names later on. 


### Factors
Create an object named REF which is only the REF column of the variants data frame
```
REF <- variants$REF
head(REF)
str(REF)
```
Items of a factor are levels, are different categories contained in a factor. R organizes the levels of a factor in alphabetical order. 

### Subsetting and exploring data frames

To extract information from a data frame we need to provide the "coordinates", where row numbers come first, and columns second

```
variants[1,1]
variants[2,4]
variants[801,29]
variants[2,]
variants[-1,]
variants[1:4,1]
variants[,c("sample_id")]
head(variants)
tail(variants)
variants$sample_id
```
Rename columns
colnames(variatns)
colnames(variants)[1] <- "strain"

### Exercise 4
Create a new data frame containing only observations from SRR2584863: (Hint use the == sign to get specific part of a column)

```
subset_variants <- variants[variants$sample_id=="SRR2584863",]
dim(subset_variants)
summary(subset_variants)
# save to a file
write.csv(subset_variants, file="data/SRR2584862-subset-variants.csv")
```

BREAK, put up stickies of what you liked/disliked about the first part of the morning lesson

### Manipulating Data with dplyr

Dplyr contains a lot of handy functions for manipulating and exploring dataframes that keep in mind with tidy data, reduce errors, and make more sense with the verbiage than some of base R, especially with classes of data and coercing data to certain types. Package for common data manipulation tasks. Sometimes want to select certain columns or filter through specific rows of a larger dataset to do a specific analysis on. Want to perform summary statistics, string together multiple operations to make code readable, pipe operator like pipes | in bash to string together multiple programs. Tidyverse is an umbrella of a suite of packages for doing data analysis - dplyr and ggplot2 is what we will be using. 

```
library(tidyverse)
```

On the server this should already be installed, raise the red side of your flag if this isn't installed on your local computer, or if warning signs come up with this. Usually put all the package you will be using at the top of your script, so they laod all at once at the start, and if you share your script with somebody else they know what dependencies they have to have. 

Select is used on columns. 
Filter rows by specific criteria. 

```
select(variatns, sample_id, REF, ALT, DP)
select(variants, -CHROM)
filter(variants, sample_id = "SRR2584863")
# equivalent to :
variants[variants$sample_id=="SRR2584863"]
# can also use the %in% function
filter(variants, REF %in% c("T", "G"))
filter(variants, QUAL >=100)
filter(variants, INDEL)
filter(variants, !is.na(IDV)) # to get rid of missing data in the IDV column, ! means everything except
# can also combine multiple conditions
filter(variants, sample_id == "SRR2584863", QUAL >=100)
```

### Exercise 5

Selet all mutations that occur between the positions 1e6 (1 million) and 2e6 (include) that are not indels and have QUAL greater than 200 
```
filter(variants, POS >= 1e6 & POS <=2e6, !INDEL, QUAL > 200)
```

### Pipes

Want to combine select and filter? Can do this with pipes a bit easier than have separate commands. Pipes let you take the output of one function and send it directly to the next function, which is nice if you need to do many seqeuential steps to the same dataset, or want to filter through your pipe select parts of your data. Pipes look like `%>%`, and can be automatically added in Mac with Cmd + shift +m or in windows, Ctrl + shift + m. 

A nice practice of pipes is that you can define the dataset first and not as the first argument of the select or filter function, so that it's more readible and obvious where you are starting out. 

```
variants %>% filter(sample_id == "SRR2584863") %>% select(REF, ALT, DP)
subset_variants <- variants %>% filter(sample_id == "SRR2584863") %>% select(REF, ALT, DP)
```

### Exercise 6

Starting with the variants dataframe, use pipes to subset data to include only observations from the SRR2584863 sample, where filtered DP is tat least 10, and only keep columns REF, ALT, POS

```
variants %>% filter(sample_id == "SRR2584863" & DP >=10) %>% select(REF, ALT, POS)
```

### Mutate

Create new columns based on the values already existing in columns, do unit conversions or find the ratio of values in two columns. Use the mutate function for this. 

Have a column named QUAL. This is a Phred-scaled confidence score that a polymorphism exists at this position given the sequencing data. Lower QUAL scores indicate low probability of a polymorphism existing at that site. Convert the confidence value QUAL to a probability with the formula: 

Probability - 1 - 10 ^ -(QUAL/10)

Let's add a probability column called POLPROB to our variants dataframe that shows the probabliilty of a polymorphism at that site given.

```
variants %>% mutate(POLPROB = 1 - (10 ^ -(QUAL/10)))
```

### Exercise 7

Using the above code, only look at the columns sample_id, POS, QUAL, and POLPROB

```
variants %>% mutate(POLPROB = 1 - (10 ^ -(QUAL/10))) %>% select(sample_id, POS, QUAL, POLPROB)
```

Get the most common size for indels. Create a new column called indel_size that contains the size difference between our sequencees and the referenc genome. The function nchar() will return the number of letters in a string

```
variants %>% mutate(indel_size = nchar(ALT) - nchar(REF))
```

Can capture missing data
```
variants_indel %>%
  filter(is.na(mutation_type))
```

Many data tasks can be approaching using the split-apply-combine approach: where you split the data into groups, apply some analysis to each group, and then combine the results. You can do this with the group_by function, which splits the data into groups. Summarize can then be used to collapse each group into a single row summary, by applying an aggregate or summary function to each group. Want to group by the mutation_type and find the average size for insertions and deletions:

```
variants_indel %>% 
  group_by(mutation_type) %>% 
  summarize(
    mean_size = mean(indel_size)
  )
```

Additional columns by adding arguments to the summarize function, want to know the median indel size: 

```
variants_indel %>%
  group_by(mutation_type) %>%
  summarize(
    mean_size = mean(indel_size),
    median_size = median(indel_size)
  )
```

View the highest filtered depth:
```
variants_indel %>%
  group_by(sample_id) %>%
  summarize(max(DP))
```

### Exercise 8
What are the largest insertions and deletions?
```
variants_indel %>%
  group_by(mutation_type) %>%
  summarize(
    max_size = max(abs(indel_size))
  )
```

Look at how many observations are present in a group. Function n() can do that:
```
variants_indel %>% 
  group_by(mutation_type) %>% 
  summarize(
    n= n()
  )
```

Can also use the dplyr verb count to combine these two commands: 
```
variants_indel %>%
  count(mutation_type)
```

### Exercise 9
1. How many mutations are found in each sample?
2. How many mutation types are found in each sample? 

```
variants_indel %>%
  count(sample_id)

variants_indel %>%
  count(sample_id, mutation_type)
```
*probably won't get to reshaping data frames, because never do *
