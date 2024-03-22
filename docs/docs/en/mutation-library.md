# Mutation library

The current library is hosted on a separate github repository [here](https://github.com/jodyphelan/tbdb). This repository contains a CSV file with all the mutations and associated information. 

### The original mutation list

Originally, the mutations used for TB-Profiler were collated from the literature (detailed [here](https://genomemedicine.biomedcentral.com/articles/10.1186/s13073-015-0164-0)). Since then, the library has been continuously updated and curated with contribution from the community.

### WHO mutation catalogue

In 2021 the WHO published a list of mutations associated with drug resistance in TB. This was followed up in 2023 with a more comprehensive list and can be found [here](https://www.who.int/publications/i/item/9789240082410).

### Which mutations are used by TB-Profiler?

There are two main mutation databases maintained for use with tb-profiler and they are hosted in a [dedicated repository](https://github.com/jodyphelan/tbdb). They are hosted on different github branches are and detailed below:

| Branch name | Description                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------|
| who         | All mutations from the 2nd edition WHO mutation catalogue.                                                            |
| tbdb      | A combinaiton of the original library and the WHO catalogue.  |

In you install tb-profiler, it will come with the tbdb library. If you want to use the WHO catalogue you can will need to run the following command:

```
tb-profiler update_tbdb --branch who
```

You should then be able to see the database by running `tb-profiler list_db`. If you then want to use this database during the profiling process then you can use the `--db who` flag.

### What is the difference between the two libraries?

The WHO catalogue is a comprehensive list of mutations associated with drug resistance in TB. These were defined based on statistical support from a large number of isolates. While this does mean that the list is comprehensive, rare resistance mutations may not have been captured. The tbdb library combines this list with the original library which was curated from the literature. This library may contain mutations which are not in the WHO catalogue but have been reported in the literature. In addition, the tbdb database contains mutations for drugs which were not evaluated by the WHO such as para-aminosalicylic acid (PAS) and cycloserine (CS). To give an idea on the differences between the two libraries, the following table shows the sensitivity and specificity of the two libraries when run on a test set of isoaltes with paired phenotypic and genotypic data. 

!!! danger "Important"
    This dataset is an amalgamation of data from many different published studies which may have different methodologies for performing the DST. The dataset is not perfect and should be taken with a grain of salt. It does however give an idea of the differences between the two libraries.


| Drug                     | Sensitive isolates | Resistant isolates | Sensitivity - tbdb | Specificity - tbdb | Sensitivity - who | Specificity - who |
|--------------------------|--------------------|--------------------|-------------|-------------|-------------|-------------|
| rifampicin               | 23022              | 10544              | 0.97        | 0.97        | 0.97        | 0.97        |
| isoniazid                | 20501              | 12838              | 0.95        | 0.98        | 0.92        | 0.98        |
| ethambutol               | 24688              | 5475               | 0.93        | 0.90        | 0.87        | 0.92        |
| pyrazinamide             | 12340              | 2842               | 0.89        | 0.96        | 0.85        | 0.97        |
| moxifloxacin             | 13446              | 2534               | 0.92        | 0.92        | 0.91        | 0.92        |
| levofloxacin             | 12620              | 3131               | 0.92        | 0.97        | 0.91        | 0.97        |
| bedaquiline              | 11045              | 100                | 0.40        | 0.98        | 0.40        | 0.98        |
| delamanid                | 10829              | 178                | 0.12        | 1.00        | 0.12        | 1.00        |
| linezolid                | 13218              | 174                | 0.40        | 1.00        | 0.40        | 1.00        |
| streptomycin             | 7527               | 3907               | 0.84        | 0.93        | 0.80        | 0.93        |
| amikacin                 | 16107              | 1652               | 0.84        | 0.99        | 0.84        | 0.99        |
| kanamycin                | 16047              | 2407               | 0.85        | 0.97        | 0.85        | 0.98        |
| capreomycin              | 9242               | 1027               | 0.79        | 0.97        | 0.78        | 0.97        |
| clofazimine              | 12934              | 483                | 0.16        | 0.99        | 0.16        | 0.99        |
| ethionamide              | 11783              | 2993               | 0.85        | 0.89        | 0.83        | 0.90        |
| para-aminosalicylic_acid | 1123               | 102                | 0.39        | 0.95        | Not tested        | Not tested        |
| cycloserine              | 960                | 157                | 0.29        | 0.97        | Not tested        | Not tested        |

As can be seen, the tbdb library has a higher sensitivity for most drugs. This is because it contains mutations which are not in the WHO catalogue. However, this comes at the cost of a drop in specificity for some drugs. The largest difference can be seen for isoniazid, ethambutol and pyrazinamide. 

!!! note "Note"
    There is no right choice between the two libraries and the choice of library will depend on the specific use case. 

# Want to contribute?
If you think a mutation should be removed or added please raise and issue [here](https://github.com/jodyphelan/tbdb/issues). If you want to help curate the library, leave a comment [here](https://github.com/jodyphelan/tbdb/issues/4).

## Adding/removing mutations
Mutations can be added by submitting a pull request on a branch modified tbdb.csv file. If that previous sentence made no sense to you then you can suggest a change using an [issue](https://github.com/jodyphelan/tbdb/issues) and we will try help.

## How does it work?

The mutations are listed in [mutations.csv](https://github.com/jodyphelan/tbdb/blob/tbdb/mutations.csv). These are parsed by `tb-profiler create_db` to generate the json formatted database used by TB-Profiler along with a few more files. Mutations can be removed and added from tbdb.csv and a new library can be built using `tb-profiler create_db`.

## tbdb.csv
This is a CSV file which must contain the following column headings:
1. Gene - These can be the gene names (e.g. rpoB) or locus tag (e.g. Rv0667).
2. Mutation - These must follow the [hgvs nomenclature](http://varnomen.hgvs.org/) or be a valid sequence ontology term. More info on this down below.
3. type
4. drug - Name of the drug
5. original_mutation - 
6. confidence - The confidence as given by the WHO mutation catalogue
7. source - A reference to the source of the mutation (e.g. WHO catalogue v2 or tbdb)
8. comment - Any additional information about the mutation

## Mutation format

### HGVS nomenclature

Mutations must follow the HGVS nomenclature. Information on this format can be found [here](http://varnomen.hgvs.org/). The following types of mutations are currently allowed:

* Amino acid substitutions. Example: S450L in rpoB would be p.Ser450Leu
* Deletions in genes. Example: Deletion of nucleotide 758 in tlyA would be c.758del
* Insertion in genes. Example: Insertion of GT between nucleotide 1850 and 1851 in katG would be c.1850_1851insGT
* SNPs in non-coding RNAs. Example: A to G at position 1401 in rrs would be n.1401a>g
* SNPs in gene promoters. Example: A to G 7 bases 5' of the start codon in pncA c.-7A>G

### Sequence ontology terms

If the mutation is not a simple amino acid substitution or indel then it can be described using a sequence ontology term. The sequence ontology is a standardised ontology for describing genomic features. All terms detected by snpEff are valid sequence ontology terms and can be found [here](http://pcingola.github.io/SnpEff/snpeff/inputoutput/#variant-annotaiton-details).

## Epistasis rules

Epistasis rules can be added to the rules.txt file. These are rules that allow tb-profiler to negate the resistance effect of certain mutations if another mutation is present. For example, if a mutation an mmpL5 loss of function mutation is present then it will inactivate the effect of any mmpR5 resistance mutation. The format of the rules.txt file is as follows:

```
Variant(gene_name=mmpL5,type=lof) inactivates_resistance Variant(gene_name=mmpR5)
```

This will inactivate the effect of any mmpR5 resistance mutation if an mmpL5 loss of function mutation is present. The rules.txt file can contain multiple rules.

## Confidence values

The confidence values are taken from the WHO catalogue but this can be changed to whatever you want.

# Generating a new library

Just download the repository using `git clone https://github.com/jodyphelan/tbdb.git`. This will generate a folder with all the required files. Then you can run `tb-profiler create_db`

```
Usage: tb-profiler create_db [-h] --prefix PREFIX [--csv CSV [CSV ...]] [--watchlist WATCHLIST] [--spoligotypes SPOLIGOTYPES] [--spoligotype_annotations SPOLIGOTYPE_ANNOTATIONS] [--barcode BARCODE]
                             [--bedmask BEDMASK] [--rules RULES] [--amplicon_primers AMPLICON_PRIMERS] [--match_ref MATCH_REF] [--custom] [--db_name DB_NAME] [--db_commit DB_COMMIT]
                             [--db_author DB_AUTHOR] [--db_date DB_DATE] [--include_original_mutation] [--load] [--no_overwrite] [--dir DIR] [--temp TEMP] [--version]
                             [--logging {DEBUG,INFO,WARNING,ERROR,CRITICAL}]

Options:
  -h, --help            show this help message and exit
  --prefix, -p PREFIX   The input CSV file containing the mutations (default: None)
  --csv, -c CSV [CSV ...]
                        The prefix for all output files (default: ['mutations.csv'])
  --watchlist, -w WATCHLIST
                        A csv file containing genes to profile but without any specific associated mutations (default: watchlist.csv)
  --spoligotypes SPOLIGOTYPES
                        A file containing a list of spoligotype spacers (default: spoligotype_spacers.txt)
  --spoligotype_annotations SPOLIGOTYPE_ANNOTATIONS
  --barcode BARCODE     A bed file containing lineage barcode SNPs (default: barcode.bed)
  --bedmask BEDMASK     A bed file containing a list of low-complexity regions (default: mask.bed)
  --rules RULES         A file containing python rules (default: rules.txt)
  --amplicon_primers AMPLICON_PRIMERS
                        A file containing a list of amplicon primers (default: None)
  --match_ref MATCH_REF
                        The prefix for all output files (default: None)
  --custom              Tells the script this is a custom database, this is used to alter the generation of the version definition (default: False)
  --db_name DB_NAME     Overrides the name of the database in the version file (default: None)
  --db_commit DB_COMMIT
                        Overrides the commit string of the database in the version file (default: None)
  --db_author DB_AUTHOR
                        Overrides the author of the database in the version file (default: None)
  --db_date DB_DATE     Overrides the date of the database in the version file (default: None)
  --include_original_mutation
                        Include the original mutation (before reformatting) as part of the variant annotaion (default: False)
  --load                Automaticaly load database (default: False)
  --no_overwrite        Don't load if existing database with prefix exists (default: False)
  --dir, -d DIR         Storage directory (default: .)
  --temp TEMP           Temp firectory to process all files (default: .)
  --version             show program's version number and exit
  --logging {DEBUG,INFO,WARNING,ERROR,CRITICAL}
                        Logging level (default: INFO)
```

If you run it without any arguments it will generate the database files with the prefix `tbdb`. This can be changed by using the `--prefix` argument.

## Using alternate reference names

The tbdb database will assume you have mapped your data to a reference with "Chromosome" as the sequence name. If your reference sequence is the same but has a differenct name e.g "NC_000962.3". You can generate a database with an alternate sequence name using the `--match_ref /path/to/your/reference.fasta` flag.

## Watchlist

There are some genes it may be of interest to record mutations even if we do not have any specific associated mutaitons. To allow this funcitonality we have included a "watchlist" file. To include genes just add them and the associated drug(s) to the watchlist.csv file.
