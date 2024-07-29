<p class="page-title">Variant Track</p>

The variant track (`type="variant"`) displays variant records from "VCF" files or equivalents. 

## File formats

* [vcf](https://samtools.github.io/hts-specs/VCFv4.2.pdf)

## Configuration Options

[General options](Tracks.md#options-for-all-track-types)

| Property           | Description                                                                                   | Default    |
|--------------------|-----------------------------------------------------------------------------------------------|------------|
| displayMode        | Display option.  'COLLAPSED' => show variants only,  'SQUISHED' and 'EXPANDED' => show calls. | 'EXPANDED' |
| squishedCallHeight | Height of genotype call rows in SQUISHED mode.                                                | 1          |
| expandedCallHeight | Height of genotype call rows in EXPANDED mode                                                 | 10         |

### Variant color options

| Property   | Description                                                                                                                                     | Default |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| color      | Specify a variant color, or function that takes a variant object and returns a color.                                                           |         |
| colorBy    | Specify an INFO field to color variants by.  Optional, if specified takes precedence over ```color``` property                                  |         |
| colorTable | Color table mapping INFO field values to colors.  Use in conjunction with colorBy.  Optional, if not specified a color table will be generated. |         |

### Genotype color options
| Property    | Description                                   | Default              |
|-------------|-----------------------------------------------|----------------------|
| noCallColor | Color for no-calls                            | "rgb(250, 250, 250)" |
| homvarColor | CSS color for homozygous non-reference calls. | "rgb(17,248,254)"    |
| hetvarColor | CSS color for heterozygous calls.             | "rgb(34,12,253)"     |
| homrefColor | CSS color for homozygous reference calls.     | "rgb(200, 200, 200)" |

## Examples

### Tabix indexed file

```javascript
{
  type: "variant",
  format: "vcf",
  url: "https://s3.amazonaws.com/1000genomes/release/20130502/ALL.chr22.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz",
  indexURL: "https://s3.amazonaws.com/1000genomes/release/20130502/ALL.chr22.phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz.tbi",
  name: "1KG variants (chr22)",
  squishedCallHeight: 1,
  expandedCallHeight: 4,
  displayMode: "squished",
  visibilityWindow: 1000
}

 ```

### Color-by info field with color table

```javascript
{
    url: "https://s3.amazonaws.com/igv.org.demo/nstd186.GRCh38.variant_call.vcf.gz",
    indexURL: "https://s3.amazonaws.com/igv.org.demo/nstd186.GRCh38.variant_call.vcf.gz.tbi",
    name: "Color by table, SVTYPE",
    visibilityWindow: -1,
    colorBy: "SVTYPE",
    colorTable: {
        "DEL": "#ff2101",
        "INS": "#001888",
        "DUP": "#028401",
        "INV": "#008688",
        "CNV": "#8931ff",
        "BND": "#891100",
        "*": "#002eff"
     }
}

```

### Color-by function.  NOTE: functions cannot be saved in session files.

```javascript
{
    url: "https://s3.amazonaws.com/igv.org.demo/nstd186.GRCh38.variant_call.vcf.gz",
    indexURL: "https://s3.amazonaws.com/igv.org.demo/nstd186.GRCh38.variant_call.vcf.gz.tbi",
    name: "Color by function, SVTYPE",
    visibilityWindow: -1,
    color: function (variant) {
        const svtype = variant.info["SVTYPE"];
        switch (svtype) {
            case 'DEL':
                return "#ff2101";
            case 'INS':
                return "#001888";
            case 'DUP':
                return "#028401";
            case 'INV':
                return "#008688";
            case 'CNV':
                return "#8931ff";
            case 'BND':
                return "#891100";
             default:
                return "#002eff";
         }
     }
}
```