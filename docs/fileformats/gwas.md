A GWAS file is a space- or tab-delimited result file from genome-wide association study (GWAS) analysis. These files 
include PLINK result files containing integrated map information (i.e., chromosomal location for each association).

The recognized file extension for a GWAS file is  **.gwas**.   

A GWAS file must contain a header line and four required columns (case-insensitive):

*   **CHR**: chromosome (aliases chr, chromosome)
*   **BP**: nucleotide location (aliases bp, pos, position)
*   **SNP**: SNP identifier (aliases snp, rs, rsid, rsnum, id, marker, markername)
*   **P**: p-value for the association (aliases p, pval, p-value, pvalue, p.value)

Columns can be in any order. Other columns besides the required ones are allowed and will be included in popup text.
The p-value will be transformed to -log10 scale for plotting.
