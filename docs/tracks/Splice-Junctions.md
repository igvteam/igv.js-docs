<p class="page-title">Splice Junction Track</p>

type = 'junction'

## File formats

### bed

The source file for this track is a .bed file.
Each row in the .bed file represents a single splice junction.  The name field (column #4) is used to encode the
following attributes as semicolon separated key-value pairs (GFF3 column 9 format).

Attributes:

* motif  - one of  'GT/AG', 'CT/AC', 'GC/AG', 'CT/GC', 'AT/AC', 'GT/AT', 'non-canonical' (from STAR v2.4)
* uniquely_mapped - number of uniquely mapped reads spanning this junction
* multi_mapped - number of non-uniquely mapped reads spanning this junction
* maximum_spliced_alignment_overhang - used as a filter if minSplicedAlignmentOverhang is defined in the track configuration
* annotationed_juncion

This is an example .bed file row:
```
chr15	92883778	92885514	motif=GT/AG;uniquely_mapped=95;multi_mapped=10;maximum_spliced_alignment_overhang=38;annotated_junction=true	95	+
```
Here the attributes are taken directly from the [STAR v2 aligner](https://github.com/alexdobin/STAR) *.SJ.out.tab file.


## Configuration Options

[General options](Tracks.md#options-for-all-track-types)

config settings for this track fall into 2 categories: **Filtering Options** determine which splice junctions are shown, 
while **Display Options** change how the splice junctions are displayed.


### Display Options

| Property                    | Description                                                                                                                                                                                | Default          |
|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|
| colorBy                     | Splice junction color. Possible values: "numUniqueReads", "numReads", "isAnnotatedJunction", "strand", "motif"                                                                             | "numUniqueReads" |
| colorByNumReadsThreshold    | If colorBy is set to "numUniqueReads" or "numReads", junction color will be darker when number of reads > this threshold                                                                   | 5                |
| thicknessBasedOn            | Splice junction line thickness. Possible values: "numUniqueReads", "numReads", "isAnnotatedJunction"                                                                                       | "numUniqueReads" |
| bounceHeightBasedOn         | Splice junction curve height. Possible values: "random", "distance", "thickness"                                                                                                           | "random"         |
| labelUniqueReadCount        | Add unique read counts to splice junction label                                                                                                                                            | true             |
| labelMultiMappedReadCount   | Add multi-mapped read counts to splice junction label                                                                                                                                      | true             |
| labelTotalReadCount         | Add total read counts to splice junction label                                                                                                                                             | false            |
| labelMotif                  | Add splice junction motif to its label                                                                                                                                                     | false            |
| labelAnnotatedJunction      | If defined, this string will be appended to the labels of splice junctions that exist in known gene models (eg. for which annotated_junction=true in the .bed file). Example value: " [A]" | null             |
| minSplicedAlignmentOverhang |                                                                                                                                                                                            | null             |

### Filtering Options

| Property                    | Description                                                                                                                                                   | Default |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| minUniquelyMappedReads      | Junction must be supported by at least this many uniquely-mapped reads.                                                                                       | 0       |
| minTotalReads               | Junction must be supported by at least this many uniquely-mapped + multi-mapped reads.                                                                        | 0       |
| maxFractionMultiMappedReads | (uniquely-mapped reads)/(total reads) must be <= this threshold                                                                                               | 1       |
| minSplicedAlignmentOverhang | Mininum spliced alignment overhang in base pairs. See [STAR aligner docs](https://github.com/alexdobin/STAR/blob/master/doc/STARmanual.pdf) for more details. | 0       |
| hideStrand                  | Set to "+" or "-" to hide junctions on the plus or minus strand.                                                                                              | null    |
| hideAnnotatedJunctions      | If true, only novel junctions will be shown (eg. those not found in gene models passed to the aligner)                                                        | false   |
| hideUnannotatedJunctions    | If true, only annotated junctions will be shown (eg. those found in gene models passed to the aligner)                                                        | false   |
| hideMotifs                  | A list of strings for motif values to hide. For example: ['GT/AT', 'non-canonical']                                                                           | []      |

## Example

```javascript
{
    type: 'spliceJunctions',
    format: 'bed',
    url: `${rootUrl}/${prefix}.SJ.out.bed.gz`,
    indexURL: `${rootUrl}/${prefix}.SJ.out.bed.gz.tbi`,
    minUniquelyMappedReads: 1,
    minTotalReads: 1,
    maxFractionMultiMappedReads: 1,
    minSplicedAlignmentOverhang: 0,
    thicknessBasedOn: 'numUniqueReads', //options: numUniqueReads (default), numReads, isAnnotatedJunction
    bounceHeightBasedOn: 'random', //options: random (default), distance, thickness
    colorBy: 'isAnnotatedJunction', //options: numUniqueReads (default), numReads, isAnnotatedJunction, strand, motif
    labelUniqueReadCount: true,
    labelMultiMappedReadCount: true,
    labelTotalReadCount: false,
    labelMotif: false,
    labelAnnotatedJunction: " [A]",
    hideStrand: '-',
    hideAnnotatedJunctions: false,
    hideUnannotatedJunctions: false,
    hideMotifs: ['GT/AT', 'non-canonical'], //options: 'GT/AG', 'CT/AC', 'GC/AG', 'CT/GC', 'AT/AC', 'GT/AT', 'non-canonical',
},
```

#### Sashimi track: combine 'spliceJunctions' track with 'wig' track

The 'merged' track type allows a 'spliceJunctions' track to be rendered on top of a 'wig' track to create a full [sashimi-like](https://miso.readthedocs.io/en/fastmiso/sashimi.html) track.

Besides the .bed.gz file described above, this also requires a separate .bigWig file.

```
{
        type: 'merged',
        name: prefix,
        height: 150,
        tracks: [
          {
            type: 'wig',
            format: "bigwig",
            url: `${rootUrl}/${prefix}.bigWig`,
          },
          {
            type: 'spliceJunctions',
            format: 'bed',
            url: `${rootUrl}/${prefix}.SJ.out.bed.gz`,
            indexURL: `${rootUrl}/${prefix}.SJ.out.bed.gz.tbi`,
	    minUniquelyMappedReads: 1,
	    ...
          },
        ],
      }
```
