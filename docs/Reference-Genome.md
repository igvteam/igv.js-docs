<p class="page-title">Genome</p>

In igv.js a _genome_ refers to a reference sequence and, optionally, one or more associated tracks and metadata such
as chromosome aliases and chromosome cytobands. 

The reference genome can be defined with either the __genome__ or __reference__  property.   One of these options is required.   


## _genome_ property

The _genome_ property specifies a reference genome by  an identifier for either (1) an IGV hosted genome definition,
or (2) a UCSC GenArk assembly.   

Example - IGV hosted genome

``` javascript
{
  ...
  genome: "hg38",
  ...
}
```

Example - UCSC GenArk assembly

``` javascript
{
  ...
  genome: "GCA_000002305.1"
  ...
}

```

IGV hosts a limited number of genome assemblies, which are listed in the table below.   The UCSC GenArk site hosts
an extensive list of assembly hubs, currenlty numbering 4,247.  The complete listing can be 
found [here](https://hgdownload.soe.ucsc.edu/hubs/UCSC_GI.assemblyHubList.txt).


**IGV Hosted Genomes**

| id              | name                                                                |
|-----------------|---------------------------------------------------------------------|
| hs1             | Human (T2T CHM13-v2.0/hs1)                                          |
| chm13v1.1       | Human (T2T CHM13-v1.1)                                              |
| hg38            | Human (GRCh38/hg38)                                                 |
| hg38_1kg        | Human (hg38 1kg/GATK)                                               |
| hg19            | Human (GRCh37/hg19)                                                 |
| hg18            | Human (hg18)                                                        |
| mm39            | Mouse (GRCm39/mm39)                                                 |
| mm10            | Mouse (GRCm38/mm10)                                                 |
| mm9             | Mouse (NCBI37/mm9)                                                  |
| rn7             | Rat (rn7)                                                           |
| rn6             | Rat (RGCS 6.0/rn6)                                                  |
| gorGor6         | Gorilla (Kamilah_GGO_v0/gorGor6)                                    |
| gorGor4         | Gorilla (gorGor4.1/gorGor4)                                         |
| panTro6         | Chimp (panTro6) (panTro6)                                           |
| panTro5         | Chimp (panTro5) (panTro5)                                           |
| panTro4         | Chimp (SAC 2.1.4/panTro4)                                           |
| macFas5         | Macaca fascicularis (macFas5)                                       |
| GCA_011100615.1 | Macaca fascicularis 6.0 (GCA_011100615.1)                           |
| panPan2         | Bonobo (MPI-EVA panpan1.1/panPan2)                                  |
| canFam3         | Dog (Broad CanFam3.1/canFam3)                                       |
| canFam4         | Dog (UU_Cfam_GSD_1.0/canFam4)                                       |
| canFam5         | Dog (canFam5)                                                       |
| bosTau9         | Cow (ARS-UCD1.2/bosTau9)                                            |
| bosTau8         | Cow (UMD_3.1.1/bosTau8)                                             |
| susScr11        | Pig (SGSC Sscrofa11.1/susScr11)                                     |
| galGal6         | Chicken (galGal6)                                                   |
| GCF_016699485.2 | Gallus gallus (GCF_016699485.2)                                     |
| danRer11        | Zebrafish (GRCZ11/danRer11)                                         |
| danRer10        | Zebrafish (GRCZ10/danRer10)                                         |
| ce11            | C. elegans (ce11)                                                   |
| dm6             | D. melanogaster (dm6)                                               |
| dm3             | D. melanogaster (dm3)                                               |
| dmel_r5.9       | D. melanogaster (dmel_r5.9)                                         |
| sacCer3         | S. cerevisiae (sacCer3)                                             |
| ASM294v2        | S. pombe (ASM294v2)                                                 |
| ASM985889v3     | Sars-CoV-2 (ASM985889v3)                                            |
| tair10          | A. thaliana (TAIR 10)                                               |
| GCA_003086295.2 | Peanut (GCA_003086295.2)                                            |
| GCF_001433935.1 | O. sativa IRGSP-1.0 (GCF_001433935.1)                               |
| NC_016856.1     | Salmonella enterica subsp. enterica serovar Typhimurium str. 14028S |
| GCA_000182895.1 | Coprinopsis cinerea okayama7#130 (GCA_000182895.1)                  |

## _reference_ object 

To define or customize a reference genome the reference property can be used.  

### example

```javascript

{
  reference: {
    "id": "hg38",
    "name": "Human (GRCh38/hg38)",
    "fastaURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg38/hg38.fa",
    "indexURL": "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg38/hg38.fa.fai",
    "cytobandURL": "https://s3.amazonaws.com/igv.broadinstitute.org/annotations/hg38/cytoBandIdeo.txt",
    "tracks": [
      {
        "name": "Refseq Genes",
        "url": "https://s3.amazonaws.com/igv.org.genomes/hg38/refGene.txt.gz",
        "order": 1000000,
        "indexed": false
      }
    ]
  }
}
```

### details

All properties except a reference sequence URL (fastaURL or twoBitURL) are optional.   An index is highly recommended 
for fasta files if the sequence is > a few mb in size.   Loading a large fasta file without an index is likely to 
freeze the browser.  In total there are 4 options for defining the reference sequence:

1. Fasta file without index, useful for small (< 10 MB) sequences.  __fastaURL__
2. Indexed fasta file.  __fastaURL, indexURL__
3. BGZIPPED compressed fasta file.  __fastaURL, indexURL, compressedIndexURL__
4. UCSC two bit sequence file (see [twobit]). (https://genome.ucsc.edu/goldenPath/help/twoBit.html).  __twoBitURL__

| Option             | Description                                                                                                                                                                                                                               | Default |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| id                 | UCSC or other id string.  _Optional_                                                                                                                                                                                                      |         |
| name               | A descriptive name.  _Optional_                                                                                                                                                                                                           |         |
| fastaURL           | URL to a FASTA file. **Either fastaURL or twoBitURL is required**.                                                                                                                                                                        |         |
| indexURL           | URL to a FASTA index (.fai file).  An index file is optional, but if not supplied the entire fasta is read.                                                                                                                               |         |
| compressedIndexURL | URL to a BGZIPPED compressed fasta file (.gzi index).  Note this is in addition, not in place, of the .fai index.                                                                                                                         |         |
| twoBitURL          | URL to a UCSC two-bit sequence file (Version 3.0).  Required if fastaURL is not defined.  If both fastaURL and twoBitURL are defined twoBitURL will be used for igv.js version >= 3.0, fastaURL for earlier igv.js versions.              |         |
| cytobandURL        | URL to a cytoband ideogram file in UCSC format.  _Optional_                                                                                                                                                                               |         |
| aliasURL           | URL to a tab-delimited file defining aliases for chromosome names.  Theformat is tab delimited with  1 line per chromosome.  Each line begins with the canonical chromosome followed by aliases.  See the example below.   _Optional_     |         |
| chromSizesURL      | URL to a UCSC .chrom.sizes file.   This can be used in combination with twoBitURL to support the whole genome view.                                                                                                                       |         |
| indexed            | Flag indicating if the FASTA is indexed.  Ignored if indexURL is supplied.  The primary purpose of this property is to indicate that the fasta is not indexed.  _**Deprecated as of Version 3.0**_                                        |         |
| tracks             | A list of tracks to be loaded with the genome.  See the [tracks](tracks/Tracks.md) description for details.  _Optional_                                                                                                                   |         |
| chromosomeOrder    | An array of chromosome names defining the order in the whole genome view and chromosome pulldown selector, if used.   _Optional_                                                                                                          |         |
| headers            | HTTP headers to include with each request. For example {"authorization": "bearer: token"}.  _Optional_                                                                                                                                    |         |
| wholeGenomeView    | Construct a "whole genome" view from the individual sequences.  This is useful for finished assemblies with a few (< 50) large chromosomes.  Its not useful for assemblies with a single or conversely thousands of sequences. _Optional_ | true    |

#### Chromosome alias example

Below is the contents of a chromosome alias file.

```
chr1	1	CM000663.2	NC_000001.11
chr2	2	CM000664.2	NC_000002.12
chr3	3	CM000665.2	NC_000003.12
chr4	4	CM000666.2	NC_000004.12
chr5	5	CM000667.2	NC_000005.10
chr6	6	CM000668.2	NC_000006.12
chr7	7	CM000669.2	NC_000007.14
chr8	8	CM000670.2	NC_000008.11
chr9	9	CM000671.2	NC_000009.12
chr10	10	CM000672.2	NC_000010.11
chr11	11	CM000673.2	NC_000011.10
chr12	12	CM000674.2	NC_000012.12
chr13	13	CM000675.2	NC_000013.11
chr14	14	CM000676.2	NC_000014.9
chr15	15	CM000677.2	NC_000015.10
chr16	16	CM000678.2	NC_000016.10
chr17	17	CM000679.2	NC_000017.11
chr18	18	CM000680.2	NC_000018.10
chr19	19	CM000681.2	NC_000019.10
chr20	20	CM000682.2	NC_000020.11
chr21	21	CM000683.2	NC_000021.9
chr22	22	CM000684.2	NC_000022.11
chrX	X	CM000685.2	NC_000023.11	23
chrY	Y	CM000686.2	NC_000024.10	24
chrM	MT	J01415.2	NC_012920.1	chrMT
```



