<p class="page-title">QTL Track</p>


The qtl track (`type="qtl"`) displays xQTL data.  


## File Formats

### qtl

Suggested file extension: '.qtl' or '.qtl.tsv'

This is a flexible tab-delimited format.  Any number of columns are supported but must include chromosome,
position (1- based), and value (p-value or probability).  A column header is required.  Position of the required
columns can be specified in the file column header, as described below,
or in the track configuration (see example).   Remaining columns are used for the popup text display.


To specify column positions for the required fields include the following case-insensitive column headers in the header line.

*   **CHR**: chromosome (aliases: chromosome, chrom, chr_id)
*   **BP**: nucleotide location (aliases: bp, pos, position, chr_pos, chromEnd)
*   **SNP**: SNP identifier (aliases: rsid, variant)
*   **P**: p-value for the association (aliases: p, pval, p-value, pvalue, p.value)
*   **Phenotype** feature name for the associated phenotype.  For example, a gene name for an eqtl.  (aliases: gene, gene_id, molecular_trait_id)


```text
CHR     SNP     BP      P       Phenotype
1       rs61769351      758443  0.000426        TTLL10
1       rs1323158546    791100  0.000419        TTLL10
1       rs58276399      796338  0.000485        TTLL10
...
```

## Configuration Options

[General options](Tracks.md#options-for-all-track-types)

| Property            | Description                                                                                                        | Default |
|---------------------|--------------------------------------------------------------------------------------------------------------------|---------|
| min                 | Minimum value of y-axis in -log10 units.                                                                           | 3.5     |
| max                 | Maximum value of y-axis in -log10 units.  Optional, if not specified max is set as a percentile of values in view. |         |
| autoscalePercentile | Upper percentile for setting max value when autoscaling.  Number between 0 and 100.                                | 98      |

## Example

```js
{
    type: "qtl", 
    format: "qtl", 
    name: "B cell eQTL",
    url: "https://igv-genepattern-org.s3.amazonaws.com/test/qtl/B.cell_eQTL.tsv.gz", 
    indexURL: "https://igv-genepattern-org.s3.amazonaws.com/test/qtl/B.cell_eQTL.tsv.gz.tbi", 
    visibilityWindow: 4000000
}

```
