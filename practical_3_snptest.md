# Practical 3 - GWAS from genotype probabilities

This exercies is about how to base association tests on genotype probabilities instead of SNP array genotypes

It will be conducted from the command line using the program SNPTEST. 

For a detailed description of SNPTEST click [here](https://mathgen.stats.ox.ac.uk/genetics_software/snptest/snptest.html) 

The exercise will be carried out at the linux server.

## Preparation - download and inspect the example data

Then make a folder called gwasEx for this exercise by typing
```
cd ~/
mkdir propEx
```
Go into this folder, download a packed version of the data that we will be analyzing today and unpack it. You can do this by typing:
```
cd propEx
cp /home/line/popgen19/practical3data.tar.gz .
tar -xf practical3data.tar.gz
```
List the content of the "data" folder:
```
ls data/
```
Inspect the file with genptype probabilities (-S option displays one line of the file per line instead of breaking lines to display all content):
```
less -S data/example.gen
```
Note that after the variant info, each line contains 3 entries per sample and these 3 entries sum to 1. Exit the display by pressing q. Then inspect the file with info on each sample
```
less -S data/example.sample
```
There are two header lines. First line name the info in the column, while the second line declare the type of data contained there (e.g. B for binary, D for discrete or C for continuous).

Set a variable to point to the SNPTEST binary
```
SNPTEST="/home/line/popgen19/snptest_v2.5.4-beta3_linux_x86_64_dynamic/snptest_v2.5.4-beta3"
```
and to get an overview of options:
```
$SNPTEST -help
```


## Test the variants for association using the likelihood ratio test

Test the 198 variants for association to the binary phenotype "bin2" assuming additive effects using the full likelihood model
```
$SNPTEST -data ./data/example.gen ./data/example.sample -pheno bin2 -frequentist 1 -method newml -o example_bin2.txt  
```
Inspect the output from snptest to the screen. How many cases and how many controls are present? 

Inspect the results file:
```
less -S example_bin2.txt
```
Column number 46 contains the p-value! Get rid of the header and sort by p-value (the data is fake so never mind that snps seems identical in pairs)
```
grep -v "^#" example_bin2.txt | tail -n+2 | sort -k46g,46 | less -S
```

Now try out other genetic models by altering "-frequentist" option. Also try the '-method expected' option, that use expected genotypes/predictors instead. E.g.
```
$SNPTEST -data ./data/example.gen ./data/example.sample -pheno bin2 -frequentist 1 -method expected -o example_bin2_expected.txt  
```
Note that now column number 42 contains the p-value.
```
grep -v "^#" example_bin2_expected.txt | tail -n+2 | sort -k42g,42 | less -S
```

