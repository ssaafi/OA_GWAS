**Strategy to implement the ABC model**


*Tissue specific*: running the ABC model per tissue allows us to identify tissue-specific chromatin interactions and gene regulation patterns. This is important because different tissues have distinct functional requirements and can have different regulatory landscapes.

*Dividing healthy and OA*: to make a comparison and see what differ between healthy and non-healthy in order to get better insight into the pathophysiology and find a potential drug target.

*Per chromosome*: help reduce computational requirement while allowing for more targeted analysis (more power). 
Genome-wide: to study the enhancer-promotor inter-chromosomal interactions. 

*Sample size/Replicate*: there is no minimal limit to run the model, however larger samples sizes are prefer to get a better representation of the underlying biological process. You also get more complexity and statistical power. Replicates are important for measuring the variability!

*HiC*: We are using the average HiC data generate in this [study](doi: 10.1038/s41586-021-03446-x) for the first attempts and will use tissue specific data afterwards.
