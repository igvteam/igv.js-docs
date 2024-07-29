<p class="page-title">Tracks</p>


Tracks in igv.js are categorized by type.  Each type is designed to display a
particular class of genomic data, such as read alignments or annotations.

## Track Types

Current track types include:

Track Type | Description | Associated File Formats
---------|-------------|----------
[annotation](Annotation-Track.md) | Non-quantitative genome annotations such as genes. This is the most generic track type. | bed, gff, gff3, gtf, bedpe, and others
[wig](Wig-Track.md) | Quantitative genomic data, such as ChIP peaks and alignment coverage. | wig, bigWig, bedGraph
[alignment](Alignment-Track.md) | Sequencing alignments. | bam, cram
[variant](Variant-Track.md) | Genomic variants | vcf
[seg](Seg-Track.md) | Segmented copy number data. | seg
[mut](Mutation-Track.md) | Mutation data, primarily from cancer studies. | maf, mut
[interact](Interact.md) | Arcs representing associations or interactions between 2 genomic loci. | bedpe, interact, bigInteract
[gwas](GWAS.md) | Genome wide association data (manhattan plots) | gwas, bed
[arc](Arc-Track.md) | RNA secondary structure | [bp](https://software.broadinstitute.org/software/igv/RNAsecStructure), [bed](https://software.broadinstitute.org/software/igv/node/284)
[junction](Splice-Junctions.md) | RNA splice junctions | bed
[qtl](QTL-Track.md) | Quantitative trait locus | qtl
[pytor](CNVPytor.md) | Copy number analysis | pytor, vcf
[merged](Merged.md) | Overlayed wig tracks |


## Configuring Tracks

Tracks can be added during initial browser configuration, or via the browser
API with the `loadTrack` function. In both cases a track is configured
with an options object.  For example, the following object creates a
gene annotation track from an indexed BED file. The track will
open initially in "expanded" mode.

```JavaScript 
{
      name: "Genes",
      type: "annotation",
      format: "bed",
      url: "<url to your bed file>",
      displayMode: "EXPANDED"
    }
``` 


### Configuration Options for all track types

| Property         | Description                                                                                                                                                                                                                                                                              | Default                                                                         |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| type             | Track type                                                                                                                                                                                                                                                                               | No default. If not specified, type is inferred from file format                 |
| sourceType       | Type of data source.  Valid values are "file", "htsget", and "custom"                                                                                                                                                                                                                    | file                                                                            |
| format           | File format                                                                                                                                                                                                                                                                              | No default. If not specified format is inferred from file name extension        |
| name             | Display name (label).  **Required**                                                                                                                                                                                                                                                      |                                                                                 |
| url              | URL to the track data resource, such as a file or webservice, or a [data URI](Data-URIs).  As of release 2.5.2 the url property can be a function or promise that returns or resolves to a url string.                                                                                   |                                                                                 |
| indexURL         | URL to a file index, such as a BAM .bai, tabix .tbi, or tribble .idx file.  **Notes:** For indexed file access the index URL is required, if absent the entire file will be read.  As of release 2.5.2 the indexURL property can be a function or promise that resolves to a URL string. |                                                                                 |
| indexed          | Flag provided to explicitly indicate the resource is not indexed. If a resource is indexed the indexURL should be provided in which case this flag is redundant.  This flag can be used to load small BAM files without an index by setting to false                                     |                                                                                 |
| order            | Integer value specifying relative order of track position on the screen.  To pin a track to the bottom use Number.MAX_VALUE.  If no order is specified, tracks appear in order of their addition.                                                                                        |                                                                                 |
| color            | CSS color value for track features, e.g. "#ff0000" or "rgb(100,0,100)".                                                                                                                                                                                                                  |                                                                                 |
| height           | Initial height of track viewport in pixels                                                                                                                                                                                                                                               | 50                                                                              |
| autoHeight       | If true, then track height is adjusted dynamically, within the bounds set by minHeight and maxHeight, to accomdodate features in view                                                                                                                                                    | false                                                                           |
| minHeight        | Minimum height of track in pixels                                                                                                                                                                                                                                                        | 50                                                                              |
| maxHeight        | Maximum height of track in pixels                                                                                                                                                                                                                                                        | 500                                                                             |
| visibilityWindow | Maximum window size in base pairs for which indexed annotations or variants are displayed                                                                                                                                                                                                | 1 MB for variants, 30 KB for alignments, whole chromosome for other track types |
| removable        | If true a "remove" item is included in the track menu.                                                                                                                                                                                                                                   | true                                                                            |
| headers          | http headers to include with each request.  For example {"foo": "fooValue", "bar": "barValue"}                                                                                                                                                                                           |                                                                                 |
| oauthToken       | OAuth token, or function returning an OAuth token.  The value will be included as a Bearer token with each request.  See [Oauth Support](/OAuth)                                                                                                                                         |                                                                                 |
