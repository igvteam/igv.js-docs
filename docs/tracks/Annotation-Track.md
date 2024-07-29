<p class="page-title">Annotation Track</p>

The annotation track (`type = 'annotation'`) display views of genomic annotations.

## File Formats

* [bed](https://genome.ucsc.edu/FAQ/FAQformat.html#format1)
* [gff3](https://gmod.org/wiki/GFF3)
* [gtf](mod.org/wiki/GFF3#GTF)
* [genePred](https://genome.ucsc.edu/FAQ/FAQformat.html#format9)
* [genePredExt](https://genome.ucsc.edu/FAQ/FAQformat.html#format9)
* [peaks](https://genome.ucsc.edu/FAQ/FAQformat.html)
* [narrowPeak](https://genome.ucsc.edu/FAQ/FAQformat.html)
* [broadPeak](https://genome.ucsc.edu/FAQ/FAQformat.html)
* [bigBed](https://genome.ucsc.edu/goldenPath/help/bigBed.html)

## Configuration Options

Configuration options specific to annotation tracks.  All properties below are optional.

Property  | Description | Default
------ | ------- | ------------
displayMode | Annotation display mode, one of "COLLAPSED", "EXPANDED", "SQUISHED" | "COLLAPSED"
expandedRowHeight | Height of each row of features in "EXPANDED" mode | 30
squishedRowHeight | Height of each row of features in "SQUISHED" mode | 15
nameField | For GFF/GTF file formats.  Name of column 9 property to be used for feature label. | 
maxRows | Maximum number of rows of features to display | 500
searchable | If true, feature names for this track can be searched for.  Use this option with caution, it is memory intensive.  This option **will not work** with indexed tracks. | false
searchableFields | For use with the ```searchable``` option in conjunction with GFF files.  An array of field (column 9) names to be included in feature searches.  When searching for feature attributes spaces need to be escaped with a "+" sign or percent encoded ("%20). |
filterTypes | Array of gff feature types to filter from display. | ['chromosome, 'gene']
color | CSS color value for track features, e.g. "#ff0000" or "rgb(100,0,100)".  |  rgb(0,0,150)
altColor | If supplied, used for features on negative strand | 
colorBy | Used with GFF/GTF files.  Name of column 9 attribute to color features by. | |
colorTable | Used in conjunction with colorBy property.  Maps attribute values to CSS colors.  See example below. |


## Example

```json
{
  "type": "annotation",
  "format": "bed",
  "name": "Gencode v18",
  "url": "https://s3.amazonaws.com/igv.broadinstitute.org/annotations/hg19/genes/gencode.v18.collapsed.bed.gz", 
  "indexURL": "https://s3.amazonaws.com/igv.broadinstitute.org/annotations/hg19/genes/gencode.v18.collapsed.bed.gz.tbi"
}
```

#### colorBy example

For GFF and GTF files the ```coloryBy``` property can be used ot specify a column 9 property to color by.  To specify
specific colors for property values use the ```colorTable``` property.  If no ```colorTable``` is specified colors
are assigned randomly.

```
{
    name: "Color by attribute biotype",
    type: "annotation",
    format: "gff3",
    displayMode: "expanded",
    height: 300,
    url: "https://s3.amazonaws.com/igv.org.genomes/hg38/Homo_sapiens.GRCh38.94.chr.gff3.gz",
    indexURL: "https://s3.amazonaws.com/igv.org.genomes/hg38/Homo_sapiens.GRCh38.94.chr.gff3.gz.tbi",
    visibilityWindow: 1000000,
    colorBy: "biotype",
    colorTable: {
       "antisense": "blueviolet",
       "protein_coding": "blue",
       "retained_intron": "rgb(0, 150, 150)",
       "processed_transcript": "purple",
       "processed_pseudogene": "#7fff00",
       "unprocessed_pseudogene": "#d2691e",
       "*": "black"
    }
},
```


#### color function

The color and altColor properties can be specified with a function that takes a feature object
as an argument and returns a CSS color string (e.g. "blue", "rgb(0,150,150)",  "#d2691e").   

Example

```
{
    name: "Color by function",
    format: "gff3",
    displayMode: "expanded",
    height: 300,
    url: "https://s3.amazonaws.com/igv.org.genomes/hg38/Homo_sapiens.GRCh38.94.chr.gff3.gz",
    indexURL: "https://s3.amazonaws.com/igv.org.genomes/hg38/Homo_sapiens.GRCh38.94.chr.gff3.gz.tbi",
    visibilityWindow: 1000000,
    color: (feature) => {
        switch (feature.getAttributeValue("biotype")) {
            case "antisense":
                return "blueviolet"
            case "protein_coding":
                return "blue"
            case "retained_intron":
                return "rgb(0, 150, 150)"
            case "processed_transcript":
                return "purple"
            case "processed_pseudogene":
                return "#7fff00"
            case "unprocessed_pseudogene":
                return "#d2691e"
            default:
                return "black"
        }
    }
}
```

The feature object passed to the color function is described below


```typescript

interface Feature {
  chr: string;
  start: integer;   
  end: integer;
  name: string;
  score: float;
  strand: string;
  cdStart: integer;
  cdEnd: integer;
  color: string;
  exons: Exon [];
  getAttributeValue: (property: string) => value;
}

interface Exon {
  start: integer;
  end: integer;
  cdStart: integer;
  cdEnd: integer;
  utr: boolean;
}

```


