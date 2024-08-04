# ndm-global

This repository contains an exploratory analysis on New Delhi metallobetalactamases (NDM) genes from annotated genomes at NCBI. The goal of the analysis is to:

- Evaluate the species diversity carrying different versions of the gene
- Look at correlation with other antibiotic and/or metal resistance genes
- Evaluate trends by country and date

## Introduction and data collection

New Delhi metallo-becta-lactamases are enzymes that provide resistant to bacteria to a broad range of beta-lactam [antibiotics](https://en.wikipedia.org/wiki/New_Delhi_metallo-beta-lactamase_1).

These genes are present in several microbial groups, in particular _Escherichia coli_ and _Klebsiella pneumoniae_ among others. Using publicly available datasets, we can look at global trends in the presence of these genes in microbial isolates from all over the world.

To obtain the data, I used the [MicroBIGG-E](https://www.ncbi.nlm.nih.gov/pathogens/docs/microbigge_gcp/) platform from NCBI, accesing throught the Google Cloud platform. This required and account to get the data using BigQuery.

The data was collected on Agust 2, 2024, using the following query:

```
SELECT
    mb.scientific_name,
    mb.contig_acc,
    mb.element_symbol,
    mb.protein_acc,
    mb.type,
    mb.class,
    mb.subclass,
    mb.taxgroup_name,
    mb.strain,
    mb.serovar,
    mb.isolation_source,
    mb.geo_loc_name,
    mb.epi_type,
    mb.host,
    mb.collection_date
FROM
    `ncbi-pathogen-detect.pdbrowser.microbigge` mb
    JOIN ( SELECT DISTINCT
            mb1.contig_acc
        FROM
            `ncbi-pathogen-detect.pdbrowser.microbigge` mb1
        WHERE
            mb1.element_symbol LIKE 'blaNDM-%') contigs 
        ON contigs.contig_acc = mb.contig_acc
ORDER BY
    mb.contig_acc,
    mb.start_on_contig;

```


