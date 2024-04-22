# Algorithm

The pipeline is designed to predict the resistance profile of a given Mycobacterium tuberculosis sample. The pipeline is divided into a couple of steps:

## Mapping

The pipeline uses [BWA](http://bio-bwa.sourceforge.net/) to map the reads to the reference genome. The pipeline can also use [minimap2](https://github.com/lh3/minimap2) for nanopore reads. The pipeline will also use [samtools](http://www.htslib.org/) to mark duplicate reads in the resulting bam file.

## Variant Calling

Variants are called by one of the following tools:
* [Freebayes](https://github.com/freebayes/freebayes) (default)
* [GATK](https://software.broadinstitute.org/gatk/)
* [bcftools](https://samtools.github.io/bcftools/bcftools.html)
* [LoFreq](https://csb5.github.io/lofreq/)
* [Pilon](https://github.com/broadinstitute/pilon)

Following the variant calling, the pipeline will annotate the variants using [SnpEff](http://snpeff.sourceforge.net/). 

## Variant filtering

The pipeline will filter the variants based on the following parameters:

```
--depth DEPTH         Minimum depth hard and soft cutoff specified as comma separated values (default: 0,10)
--af AF               Minimum allele frequency hard and soft cutoff specified as comma separated values (default: 0,0.1)
--strand STRAND       Minimum read number per strand hard and soft cutoff specified as comma separated values (default: 0,3)
--sv_depth SV_DEPTH   Structural variant minimum depth hard and soft cutoff specified as comma separated values (default: 0,10)
--sv_af SV_AF         Structural variant minimum allele frequency hard cutoff specified as comma separated values (default: 0.5,0.9)
--sv_len SV_LEN       Structural variant maximum size hard and soft cutoff specified as comma separated values (default: 100000,50000)
```

The first three are for short variants and the last three are for large deletions. For each of these parameters there are two values. For a variant if the value is less than (or greater than for --sv_len) the first cutoff value it is removed from the analysis. If the value is in between the first and second cutoff values it is maked as "soft fail" and removed from the final predictions but kept in the report in the "qc_fail_variants" section.

For example, the --af flag specifies the minimum allele frequency hard and soft cutoffs. If the default values are set to 0.1 and 0.4 (--af 0.1,0.4). This means that, tb-profiler will filter out mutations with a frequency less than 0.4. Any variant with less than 0.1 in frequency will not be analysed. Anything between 0.1 and 0.4 will be kept in the report but will be flagged as a failing the soft cutoff. This allows the user to decide if they want to keep these variants or not. By default, none of these "soft fail" variant make it into the final collated reports and this is why we do not see any mutations with a frequency less than 0.4 in the dataset.