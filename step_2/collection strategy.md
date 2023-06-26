!! I included TSV files gathering the information below with more details in step_2 folder!!
### Strategy to collect data

We looked on different databases using < Osteoarthritis ATAC-Seq >, < Osteoarthritis Dnase-Seq >, < Osteoarthritis H3K27ac Chip-Seq >, < Osteoarthritis Chip-Seq >, < Osteoarthritis Hi-C > and < Osteoarthritis RNA-Seq >.
We did the same to look for healthy joint tissue data by replacing " Osteoarthritis " with " Cartilage ", " Synovium ", " Bone " and " Muscles ". 

We included Chip-Seq data, but only H3K27ac, H3K27me3, H3K4me3, H3K4me1 which are markers linked to OA and/or active/inactive enhancer and promoter.

For each dataset, we will create a table with information on the data, including: 

    - the article doi reference
    - the GEO data set reference 
    - Kind of sample: human adults, cell culture, embryonic 
    - Cell type: chondrocytes, osteoblasts, osteoclasts
    - Number of samples
    - Sequencing depth
    - Raw or processed data
    - Category of data: ATAC-Seq, Dnase-Seq, Chip-Seq, HiC or gene expression
    - OA or healthy 
    - Year of publication
    - Population

Next, we controlled the quality of the data: how many samples did they use? did they do quality control? what's the sequencing depth?

**1. PUBMED**

On PubMed, we found: 

    - 13 for < Osteoarthritis ATAC-Seq >
    - 0 for < Osteoarthritis Dnase-Seq >
    - 0 for < Osteoarthritis Hi-C >
    - 1 for < Osteoarthritis H3K27ac Chip-Seq >
    - 16 for < Osteoarthritis Chip-Seq >
    - 17 for < Cartilage ATAC-Seq >
    - 0 for < Cartilage Dnase-Seq >
    - 0 for < Cartilage Hi-C >
    - 3 for < Cartilage H3K27ac Chip-Seq >
    - 29 for < Cartilage Chip-Seq >
    - 7 for < Synovium ATAC-Seq >
    - 0 for < Synovium Dnase-Seq >
    - 0 for < Synovium Hi-C >
    - 2 for < Synovium H3K27ac Chip-Seq >
    - 8 for < Synovium Chip-Seq >

To find additional OA sets, we also looked at Capellini's publication (prolific researcher on OA) and also checked the references in his articles.

For muscle, bone, and RNA-Seq, we adapted our methodology because we got too many results on PubMed. Therefore, we switched to the NCBI GEO browser. 
Here are the results:

    - 14 for "homo sapiens"[Organism] AND "bone"[All Fields]) AND "atac seq"[All Fields]) AND "healthy"[All Fields]
    - 2 for "homo sapiens"[Organism] AND "bone"[All Fields]) AND "atac seq"[All Fields]) AND "osteoarthritis"[All Fields]
    - 0 for "homo sapiens"[Organism] AND "bone"[All Fields]) AND "dnase seq"[All Fields]) AND "healthy"[All Fields]
    - 0 for "homo sapiens"[Organism] AND "bone"[All Fields]) AND "dnase seq"[All Fields]) AND "osteoarthritis"[All Fields]
    - 3 for "homo sapiens"[Organism] AND "bone"[All Fields]) AND "hi c"[All Fields]) AND "healthy"[All Fields]
    - 0 for "homo sapiens"[Organism] AND "bone"[All Fields]) AND "hi c"[All Fields]) AND "osteoarthritis"[All Fields]
    - 87 for "homo sapiens"[Organism] AND "bone"[All Fields]) AND "chip seq"[All Fields]) AND "healthy"[All Fields]
    - 0 for "homo sapiens"[Organism] AND "bone"[All Fields]) AND "chip seq"[All Fields]) AND "osteoarthritis"[All Fields]
    - 7 for "homo sapiens"[Organism] AND "muscle"[All Fields]) AND "atac seq"[All Fields]) AND "healthy"[All Fields] 
    - 0 for "homo sapiens"[Organism] AND "muscle"[All Fields]) AND "atac seq"[All Fields]) AND "osteoarthritis"[All Fields]
    - 3 for "homo sapiens"[Organism] AND "muscle"[All Fields]) AND "dnase seq"[All Fields]) AND "healthy"[All Fields]
    - 0 for "homo sapiens"[Organism] AND "muscle"[All Fields]) AND "dnase seq"[All Fields]) AND "osteoarthritis"[All Fields]
    - 0 for "homo sapiens"[Organism] AND "muscle"[All Fields]) AND "hi c"[All Fields]) AND "healthy"[All Fields]
    - 0 for "homo sapiens"[Organism] AND "muscle"[All Fields]) AND "hi c"[All Fields]) AND "osteoarthritis"[All Fields]
    - 81 for "homo sapiens"[Organism] AND "muscle"[All Fields]) AND "chip seq"[All Fields]) AND "healthy"[All Fields]
    - 0 for "homo sapiens"[Organism] AND "muscle"[All Fields]) AND "chip seq"[All Fields]) AND "osteoarthritis"[All Fields]
    
    
In total, we found 
   
    - 3 sets of ATAC-Seq
    - 0 set of Dnase-Seq
    - 1 set of Hi-C
    - 7 sets of Chip-Seq
    - 20 sets of RNA-Seq

**2. Musculoskeletal knowledge browser**

On the Musculoskeletal knowledge browser, we found:

    - 3 sets of ATAC-Sq
    - 1 set of DNase-Seq
    - 0 set of Hi-C
    - 0 set of Chip-Seq
    - 0 sets of RNA-Seq

**3. ENCODE**

On Encode we found:

    - 2 sets of ATAC-Sq
    - 12 set of DNase-Seq
    - 0 set of Hi-C
    - 0 set of Chip-Seq
    - 21 sets of RNA-Seq

**4. ROADMAP**

On Roadmap we found:

    - 0 sets of ATAC-Sq
    - 1 set of DNase-Seq
    - 0 set of Hi-C
    - 24 set of Chip-Seq
    - 1 sets of RNA-Seq


**5. QC**

We did a first check for the QC using the following 

1) Replicate 
2) Plateform
3) Reads/Coverage/Sequencing depth
4) Origin (is it fresh samples?)
5) did they do the QC themself too
6) Check what kind of sequencing used exactly (for example scRNAsq, whole genome or targeted sequencing, ...)
7) Cohort size
8) Additional QC




