### Annotations

We  need gene annotations (bed file) and chromosome annotations (bed file) that we can download from on Fantom, 3D genome browser and UCSC.

Annotations files are already available on the GiHub page of the ABC model. All I had to do is splitting the files per chromosome.
I used the following code:

```Python
#Use  to split the bed files
index=1
while read line; do
  echo "$line" > "outputfile$index.bed"
  index=$((index + 1))
done < inputfile.bed

#Use  to split txt files
index=1
while read line; do
  echo "$line" > "outputfile$index.txt"
  index=$((index + 1))
done < inputfile.txt
```

For our analysis, we need to establish a list of GWAS SNPs linked to OA for the fine mapping step.
We use the GWAS SNPs from the latest [review](https://www.sciencedirect.com/science/article/pii/S1063458421006324?via%3Dihub) on OA genetics to establish this list. You can find the list [here](https://github.com/ssaafi/internship22-23/blob/main/step_2/TSV/GWAS%20SNPs%20OA.txt).
