<p class="page-title">Alignment Track</p>

The alignment track (`type = 'alignment'`) is used to display views of read alignments from BAM or CRAM files.

## File formats

* [BAM](https://samtools.github.io/hts-specs/SAMv1.pdf)
* [CRAM](https://samtools.github.io/hts-specs/CRAMv3.pdf)


## Options

Property  | Description                                                                                                                                                                                                                                              | Default                  
------ |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------
showCoverage | Show coverage depth track.                                                                                                                                                                                                                               | true                     
showAlignments | Show individual alignments.                                                                                                                                                                                                                              | true                     
viewAsPairs | If true, paired reads are drawn connected with a line.                                                                                                                                                                                                   | false                    
pairsSupported | If false, mate information in paired reads is ignored during downsampling and the 'View as Pairs' option is removed from the alignment track menu.                                                                                                       | true                     
coverageColor | Color of coverage track                                                                                                                                                                                                                                  | rgb(150, 150, 150)       
color | Default color of alignment blocks                                                                                                                                                                                                                        | rgb(170, 170, 170)       
deletionColor | Color of line representing a deletion                                                                                                                                                                                                                    | black                    
skippedColor | Color of line representing a skipped region (e.g. splice junction)                                                                                                                                                                                       | rgb(150, 170, 170)       
insertionColor | Color of marker for insertions                                                                                                                                                                                                                           | rgb(138, 94, 161)        
negStrandColor | Color of alignment on negative strand.  Applicable if colorBy = "strand"                                                                                                                                                                                 | rgba(150, 150, 230, 0.75) 
posStrandColor  | Color of alignment or position strand.  Applicable if colorBy = "strand"                                                                                                                                                                                 | rgba(230, 150, 150, 0.75) 
pairConnectorColor | Color of connector line between read pairs ("view as pairs" mode).  Optional.                                                                                                                                                                            | alignment color          
colorBy | Alignment color option:  one of "none", "strand", "firstOfPairStrand", "pairOrientation", "tlen", "unexpectedPair", or "tag".  If colorBy = "tag" specify tag with colorByTag.  ***NOTE*** As of release 3.0 color by tag is specified as 'tag:<tag name>' | "unexpectedPair".        
colorByTag | Specific tag to color alignment by.  ***Deprecated as of release 3.0***                                                                                                                                                                                  |
bamColorTag | Specifies a special tag that explicitly encodes an r,g,b color value.  Default value is "YC".  If "YC" does not encode an r,g,b color value set bamColorTag to null.  Must also set "colorBy" to "tag" to enable this option.                            | "YC"                     
samplingWindowSize | Window (bucket) size for alignment downsampling in base pairs                                                                                                                                                                                            | 100                      
samplingDepth | Number of alignments to keep per bucket.  WARNING:  Setting this sampling depth to a high value can freeze the browser when viewing areas of deep coverage.                                                                                              | 100.                     
alignmentRowHeight | Height in pixels of an alignment row when in expanded mode                                                                                                                                                                                               | 14                       
readgroup | Readgroup ID value (tag 'RG').                                                                                                                                                                                                                           |
sort | Initial sort option.  See below.                                                                                                                                                                                                                         |                          |
filter | Alignment filter options.  See below                           |
showSoftClips | Show soft-clipped regions                                                                                                                                                                                                                                | false                    
showMismatches | Highlight alignment bases which do not match the reference.                                                                                                                                                                                              | true                     
showInsertionText | Show number of bases for insertions inline when zoomed in.                                                                                                                                                                                               | false                    
insertionTextColor | Color for insertion count text.                                                                                                                                                                                                                          | white                    
showDeletionText | Show number of bases deleted inline when zoomed in.                                                                                                                                                                                                      | false                    
deletionTextColor | Color for deletion count text.                                                                                                                                                                                                                           | black                    


#### Paired-end and mate-pair coloring options.

Property | Description | Default
-------- | ----------- | -------
pairOrientation | Expected orientation of pairs, one of ff, fr, or rf.  | Computed
minTLEN | Minimum expected absolute "TLEN" value.  Pairs below this value are colored blue if colorBy == "tlen" or "unexpectedPair" | 
maxTLEN | Maximum expected absolute "TLEN" value.   Pairs above this value are colored red if colorBy == "tlen" or "unexpectedPair" | Computed
minTLENPercentile | The percentile threshold for expected insert size.  If minTLEN is not specified this value is used to compute one. See notes below. | 0.1
maxTLENPercentile | The percentile maximum for expected insert size.  If maxTLEN is not specified this value is used to compute one. See notes below.| 99.9

**Note on TLEN:** This refers to column 9 of an alignment record from the SAM specification. It has variously been called "template length", "insert size", and "fragment length". There is no agreement on a precise definition, and aligner interpretations differ, but generally it can be thought of as the distance between start and end of an aligned pair.   Pairs with TLEN outside the expected range can be indicative of a deletion, or more rarely and insertion.

**Note on TLEN computation:** if minTLEN or maxTLEN are not specified a value is computed from a sampling of the first data loaded for the track.  Specifically, the values are taken from a percentile of the sample data as specified by minPercentile and maxPercentile.  It is possible to specify an explicit value for one.

#### Sort option

The `sort` object defines initial sort order of packed alignment rows based on an alignment property at a specified position

Property | Description | Default
-------- | ----------- | -------
chr  | Sequence (chromosome) name  |
position    | Genomic position (integer) |
option   | Parameter to sort by.  One of 'BASE', 'STRAND', 'INSERT_SIZE', 'MATE_CHR', 'MQ', 'TAG' | 
tag      | Tag name to sort by.  Include only if option = 'TAG |   
direction | Sort directions.  ASC = ascending, DESC = descending | "ASC"

#### Filter options

The `filter` object defines an alignment filter  based on sam flags.  If not supplied default filters are set 
as indicated in the table below.

Property | Description | Default
------- | ------------ | --------
vendorFailed | filter alignments marked as failing vendor quality checks (bit 0x200) |  true
duplicates | filter alignments marked as a duplicate (bit 0x400) | true 
secondary | filter alignments marked secondary (bit 0x100) | false
supplementary | filter alignments marked as supplmentary (bit 0x800) | false
mqThreshold | filter alignments with mapping quality < supplied value (a number) | 0
readgroups | array of read group names ('RG' tag).  If present filter alignments not matching this set |


## Functions

An alignment track object exports 2 functions that can be called after track creation. 

* setHighlightedReads(arrayOfReadNames, color)

Highlights alignments with specified read names by drawing an outline in the specified color.

```
bamTrack.setHighlightedReads(["SRR099953.99059361", "SRR099953.79101554"], "#0000ff")
```

* sort(sortOptions)

Sort the alignment track.  See above for the `sortOptions` object definition.

```
                bamTrack.sort({
                    chr: "chr1",
                    position: 155155361,
                    option: "BASE",
                    direction: "ASC"
                })
```


