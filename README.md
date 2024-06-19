# TBWes
##TUMOR Mutation Burden from Whole Exome Sequencing.
Tumor Mutation burden is used to decide the potential success of immunotherapy. In principle, it is a straightforward ratio of the count of somatic mutations
divided by the number of bases that could be mutated. In practice, there are a number of factors that can bias the computation.
- artifactuals Somatic mutations
    - sequencing artifacts from mononucleotide repeats
    - artifactual repeats from mapping of repeats or pseudeogenes
- Tumor Subclones : Fortunately, in many cases subclones are related and they share a lot of historical mutations. However, for tumors that have diverged a long time ago, this increases the apparent tumor burden.
    - Increasing minimal sequencing depth allows to better call subclone mutations, but it also increases the odds of of getting artifactual events. 
- Sequencing Depth: If the sequencing depth is too low, we may miss somatic mutations in the tumors. For a pure tumor (not mixed with normal tissue), 7X reads should capture at least 1 mutant read 99 percent of the time, but this depth increases as the tumor purity drops.
- Normal contamination: Variants coming from the normal tissue can contaminate the somatic variant calls from tumor-only.

A common approach to removing artifactual variants is to require a minimum mutant allele frequency (MAF) above a certain cutoff (typical cutoff is 5%). 
For example, for a tumor with 20 percent purity, and two subclones population, should produce on average of 5 percent mutant reads. For 100X coverage, 5 percent MAF is 5 reads. However, if the a subclone is really present at the 10 percent rate and being
sequenced, we could end up with 0,1,2,3,4 (or more than 5) reads. 5 or more reads cutoff (5 percent MAF) only captures 56 percent of the somatic variants. In fact, a 5 percent MAF cutoff as 100X only captures 95 percent of variants if the underlying frequency is
9 percent (instead of 5 percent). So unless the sequencing depth is sufficient to capture all subclones, using the MAF cutoff approach to variant filtering will underestimate the Tumor Burden.

Increasing the minimal depth could work to capture subclone mutations, but even though exome sequencing has uneven sequencing depth, increasing the cutoffs too much would 
severely limit the number of sites being used for somatic mutations. Multiple publications show that 1-6 MB of sites are required for good accuracy. However, the cutoff used for deciding therapy is usually around 10 mutations per megabases and 1 MB is
can lead to many patients having measure mutation counts below the cutoff.

TMBWes's approach to variant contamination is to
- mask out regions of known calling difficulty (bed file)
- Rely on variant caller artifactual mutations detection (especially mutect2). Do not rely on an MAF cutoff, but do compute an option.
- Bootstrap resample sites to estimate Variance in TMB estimate.
- Fit a variance weighted linear model to TMB as a function of depth and take the linear model estimate at the 1MB depth.
  




