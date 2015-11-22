an approach from Gordon Smyth

(Professor, Bioinformatics Division, 
Walter and Eliza Hall Institute of Medical Research,
1G Royal Parade, Parkville, Vic 3052, Australia.)


> "Of the gene set tests in limma, the only ones that can be used as part of a negative binomial based analysis of
count data are wilcoxGST and geneSetTest.  The others, roast and romer, make multivariate normal
assumptions that are incompatible with count distributions.

>To make a gene set test using geneSetTest, you would do something like

	index <- lrtFilter$genes$ID %in% MyGeneSetSymbols
	p.value <- geneSetTest(index, lrtFiltered$table$LR.statistic, type="f")

>The approach does treat the genes as statistically independent, so gives p-values that are bit
optimistic, but otherwise it is a productive approach.

>To use romer, you would have to make some normal approximations:

	effective.lib.size <- d$samples$lib.size * d$samples$norm.factors
	y <- log2( t(t(d$counts+0.5) / (effective.lib.size+0.5)) )

> Then you can use

	test <- romer(geneIndex, y, design, contrast=7)

> Actually, you don't even need to set the contrast argument, as the last coefficient is the default.  The
contrast argument of romer is like a combination of the coef and contrast arguments of glmLRT.  If it is of
length one, it is taken to be a coef.  If it's a vector, it's taken to be a contrast."

>I hope this advice is useful to others wishing to undertake GSEA with RNA-Seq data (it is for me!).