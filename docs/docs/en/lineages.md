# Lineages

## How it works 

Lineages are assigned by looking for lineage-specific SNPs. The initial list of SNPs was published by [Coll et al.](https://pubmed.ncbi.nlm.nih.gov/25176035/). The SNP barcode was further refined by [Napier et al.](https://genomemedicine.biomedcentral.com/articles/10.1186/s13073-020-00817-3) and [Zwyer et al](https://open-research-europe.ec.europa.eu/articles/1-100).

For each SNP in the barcode the proportion of lineage specific alleles are found. As each lineage has multiple SNPs, an average proportion is calculated and reported in the "lineage" field of the output. The lineage system is a hirearchical system where higher resolution is indicated by increasing number of digits (e.g. lineage 2.2.1).  

![Alt text](https://media.springernature.com/full/springer-static/image/art%3A10.1186%2Fs13073-020-00817-3/MediaObjects/13073_2020_817_Fig2_HTML.png)

## Multiple lineages found

Sometimes tb-profiler may report the presence of multiple lineages (lineage1.2.1;lineage2.2.1). This could be due to:

* A mixed-strain infection
* Possible contamination 
* Issues with low coverage

Check to see that get a consistant proportion for the sublineages within a major lineage as this will add confidence in the result.