**DATASETS** 

*ATAC dataset*: GSE108301 (5 adult human knee cartilage samples, paired-end, 50bp, 2018)
fresh tissue from lateral tibial plateau from patients that undertook joint replacement, stored in 4 °C DMEM for three hours until they were processed

*Chip dataset*: GSE111850 (3 adult human knee cartilage samples (3 replicate), single-end, 2019, comparative study)
Fetal and embryonic tissue samples were obtained from Novogenix Laboratories, Adult human primary tissue samples were obtained from National Disease Research Interchange (NDRI)

*RNA dataset*: GSE150411
Primary articular chondrocytes were isolated by enzymatic digestion from normal human femoral cartilage obtained from three tissue donors, aged 50–61 years and without a history of arthritis. 

*HiC data*: average 

Genome wide (sample not separated by chromosome)
Quantile normalization, used the one from the model.

**STEP1 - CALL PEAKS**

5 ATAC-Seq samples

```Python
#!/bin/bash

#SBATCH --mem=50Gb
#SBATCH -n1
#SBATCH -c1


sample=( "SRR6391858" "SRR6391862" "SRR6391864" "SRR6391866" "SRR6391870" )

for sample in "${sample[@]}"; do
        macs2 callpeak \
          -t ~/ABC_model_cartilage/cartilage/ATAC/GSE108301_1/"$sample".bam \
          -n "$sample".macs2 \
          -f BAM \
          -g hs \
          -p .1 \
          --call-summits \
          --outdir ~/ABC_model_cartilage/output/Peaks/
done
```

Sort narrowPeak file

```Python
#!/bin/bash

#SBATCH --mem=25Gb
#SBATCH -n1
#SBATCH -c1

sample=( "SRR6391858" "SRR6391862" "SRR6391864" "SRR6391866" "SRR6391870" )

for sample in "${sample[@]}"; do
        bedtools sort -i ~/ABC_model_cartilage/output/Peaks/"$sample".macs2_peaks.narrowPeak > ~/ABC_model_cartilage/output/Peaks/"$sample".macs2_peaks.narrowPeak.sorted
done
```

Call candidate regions


```Python
#!/bin/bash

#SBATCH --mem=50Gb
#SBATCH -n1
#SBATCH -c1

sample=( "SRR6391858" "SRR6391862" "SRR6391864" "SRR6391866" "SRR6391870" )

for sample in "${sample[@]}"; do
        python ~/src/makeCandidateRegions.py \
          --narrowPeak ~/ABC_model_cartilage/output/Peaks/"$sample".macs2_peaks.narrowPeak.sorted \
          --bam ~/ABC_model_cartilage/cartilage/ATAC/GSE108301_1/"$sample".bam \
          --outDir ~/ABC_model_cartilage/output/Peaks/ \
          --chrom_sizes ~/reference/chr_sizes \
          --regions_blocklist ~/reference/wgEncodeHg19ConsensusSignalArtifactRegions.bed \
          --regions_includelist ~/reference/RefSeqCurated.170308.bed.CollapsedGeneBounds.TSS500bp.bed \
          --peakExtendFromSummit 250 \
          --nStrongestPeaks 150000
done
```


**STEP2 - QUANTIFYING ENHANCER ACTIVITY**

```Python
#!/bin/bash

#SBATCH --mem=50Gb
#SBATCH -n1
#SBATCH -c1

# Defining the input variables
    H3K27ac="/home/090186/ABC_model_cartilage/cartilage/Chip/GSE111850"
    expression_table="/home/090186/ABC_model_cartilage/cartilage/RNA/GSE150411"
    atac="/home/090186/ABC_model_cartilage/cartilage/ATAC/GSE108301_1"


# Run the python script
    python ~/src/run.neighborhoods.py \
    --candidate_enhancer_regions /home/090186/ABC_model_cartilage/output/Peaks/SRR6391858.macs2_peaks.narrowPeak.sorted.candidateRegions.bed \
    --genes /home/090186/reference/RefSeqCurated.170308.bed.CollapsedGeneBounds.bed \
    --H3K27ac "$H3K27ac"/SRR6836014.bam,"$H3K27ac"/SRR6836019.bam,"$H3K27ac"/SRR6836024.bam \
    --ATAC  "$atac"/SRR6391858.bam,"$atac"/SRR6391862.bam,"$atac"/SRR6391864.bam,"$atac"/SRR6391866.bam,"$atac"/SRR6391870.bam\
    --expression_table "$expression_table"/GSE150411_Rep1.txt,"$expression_table"/GSE150411_Rep2.txt,"$expression_table"/GSE150411_Rep3.txt \
    --qnorm /home/090186/src/EnhancersQNormRef.K562.txt \
    --chrom_sizes /home/090186/reference/chr_sizes \
    --ubiquitously_expressed_genes /home/090186/reference/UbiquitouslyExpressedGenesHG19.txt \
    --cellType AdKnChondrocytes  \
    --outdir /home/090186/ABC_model_cartilage/output/Neighborhoods/ \ 
```

**STEP3 - CALL PEAKS**

```Python 
#!/bin/bash

#SBATCH --mem=50Gb
#SBATCH -n1
#SBATCH -c1


python ~/src/predict.py \
  --enhancers ~/ABC_model_cartilage/output/Neighborhoods/EnhancerList.txt \
  --genes ~/ABC_model_cartilage/output/Neighborhoods/GeneList.txt \
  --HiCdir ~/ABC_model_cartilage/cartilage/HiC/ \
  --chrom_sizes ~/ABC_model_cartilage/reference/chr_sizes \
  --hic_resolution 5000 \
  --scale_hic_using_powerlaw \
  --threshold .02 \
  --cellType AdKnChondrocytes \
  --outdir ~/ABC_model_cartilage/output/Predictions/  \
  --make_all_putative \
```




