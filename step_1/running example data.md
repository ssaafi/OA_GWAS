**1. Define the candidate elements**

The first step is creating a conda environment (to have all the programs with the correct versions). However, Marijn (from IT) suggested not downloading conda because we will have a duplicate of many programs. He checked all the dependencies and made sure that the right version of every program was available. So for the first try, we didn't use the conda environment and we had the first error in step2.

Therefore we decided to use the two conda environments: macs conda and abc conda. We activated the macs conda environment to call peaks via:

```Python
conda activate macs.yml 
```

Call peaks

```Python
macs2 callpeak
-t ~/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/input_data/Chromatin/wgEncodeUwDnaseK562AlnRep1.chr22.bam
-n wgEncodeUwDnaseK562AlnRep1.chr22.macs2
-f BAM
-g hs
-p .1
--call-summits
--outdir example_chr22/ABC_output/Peaks/ 
```

Sort narrowPeak file

```Python
bedtools sort -faidx example_chr22/reference/chr22 -i example_chr22/ABC_output/Peaks/wgEncodeUwDnaseK562AlnRep1.chr22.macs2_peaks.narrowPeak > example_chr22/ABC_output/Peaks/wgEncodeUwDnaseK562AlnRep1.chr22.macs2_peaks.narrowPeak.sorted
```

Worked with the conda environment the very first time. We activated the abc conda environment to call candidate regions via:

```Python
conda activate abcenv.yml 
```

Call candidate regions

```Python
python ~/ABC_model/ABC-Enhancer-Gene-Prediction-master/src/makeCandidateRegions.py
--narrowPeak ~/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/ABC_output/Peaks/wgEncodeUwDnaseK562AlnRep1.chr22.macs2_peaks.narrowPeak.sorted
--bam ~/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/input_data/Chromatin/wgEncodeUwDnaseK562AlnRep1.chr22.bam
--outDir ~/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/ABC_output/Peaks/
--chrom_sizes ~/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/reference/chr22
--regions_blocklist ~/ABC_model/ABC-Enhancer-Gene-Prediction-master/reference/wgEncodeHg19ConsensusSignalArtifactRegions.bed
--regions_includelist ~/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/reference/RefSeqCurated.170308.bed.CollapsedGeneBounds.TSS500bp.chr22.bed
--peakExtendFromSummit 250
--nStrongestPeaks 3000 
```

Worked as well

**2. Quantifying Enhancer Activity**

I create shell scripts (.sh) so I don't have to type everything over and over again using the command nano (also use to modify the script + command cat to read it).

PROBLEM Here's the ~/home/.../abc2.sh

```Python
#!bin/bash

LOC=/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22 OUT=/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/ABC_output/Neighborhoods

python3.10 /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/src/run.neighborhoods.py --candidate_enhancer_regions $LOC/ABC_output/Peaks/wgEncodeUwDnaseK562AlnRep1.chr22.macs2_peaks.narrowPeak.sorted --genes $LOC/reference/RefSeqCurated.170308.bed.CollapsedGeneBounds.chr22.bed --H3K27ac $LOC/input_data/Chromatin/ENCFF384ZZM.chr22.bam --DHS $LOC/input_data/Chromatin/wgEncodeUwDnaseK562AlnRep1.chr22.bam,$LOC/input_data/Chromatin/wgEncodeUwDnaseK562AlnRep2.chr22.bam --expression_table $LOC/input_data/Expression/K562.ENCFF934YBO.TPM.txt --chrom_sizes $LOC/ABC_output/Peaks/wgEncodeUwDnaseK562AlnRep1.chr22.macs2_peaks.narrowPeak.sorted
--ubiquitously_expressed_genes $LOC/reference/UbiquitouslyExpressedGenesHG19.txt --cellType K562 --outdir $OUT exit 
```

ERROR: 
```Python
 pandas.errors.ParserError: Too many columns specified: expected 12 and found 2
 ```

RESOLVE WITH the following ~/home/.../abc.sh 

```Python
#!bin/bash

LOC=/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22 OUT=/ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/ABC_output/Neighborhoods

python /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/src/run.neighborhoods.py --candidate_enhancer_regions $LOC/ABC_output/Peaks/wgEncodeUwDnaseK562AlnRep1.chr22.macs2_peaks.narrowPeak.sorted.candidateRegions.bed --genes $LOC/reference/RefSeqCurated.170308.bed.CollapsedGeneBounds.chr22.bed --H3K27ac $LOC/input_data/Chromatin/ENCFF384ZZM.chr22.bam --DHS $LOC/input_data/Chromatin/wgEncodeUwDnaseK562AlnRep1.chr22.bam,$LOC/input_data/Chromatin/wgEncodeUwDnaseK562AlnRep2.chr22.bam --expression_table $LOC/input_data/Expression/K562.ENCFF934YBO.TPM.txt --chrom_sizes $LOC/reference/chr22 --ubiquitously_expressed_genes $LOC/reference/UbiquitouslyExpressedGenesHG19.txt --cellType K562 --outdir $OUT exit 
```

It took a lot of time to find the issue (wrong version of python). Next time check the dependencies first! Might also be because we didn't use the conda environment at first.

**3. Computing the ABC Score**

```Python
python /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/src/predict.py
--enhancers /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/ABC_output/Neighborhoods/EnhancerList.txt
--genes /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/ABC_output/Neighborhoods/GeneList.txt
--HiCdir /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/input_data/HiC/raw/
--chrom_sizes /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/reference/chr22
--hic_resolution 5000
--scale_hic_using_powerlaw
--threshold .02
--cellType K562
--outdir /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/ABC_output/Predictions/
--make_all_putative 
```

It went smoothly.

**4. Get Prediction Files for Variant Overlap**

```Python
python /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/src/getVariantOverlap.py
--all_putative /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/ABC_output/Predictions/EnhancerPredictionsAllPutative.txt.gz
--chrom_sizes /home/.../ABC_model/ABC-Enhancer-Gene-Prediction-master/example_chr22/reference/chr22
--outdir . 
```

It went smoothly.

*Note*: I had to write the whole path most of the time, or else it couldn't find the correct file. Maybe next time, create a folder with all the files needed.
