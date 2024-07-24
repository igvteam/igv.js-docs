<p class="page-title">Regions of Interest</p>


A Region of Interest (ROI) is a specified genomic extent that can be configured to highlight a region displayed in the browser in one of two ways:

1. A **track** is configured with one or more ROI sets to highlight regions in the track.
2. The IGV **browser** is configured with one or more ROI sets  to highlight regions across all tracks.

An ROI appears visually as a translucent color overlay. It is configured using an `roi` property either (1) on the `track` property of the browser configuration, or (2) on the browser configuration. The `roi` property refers to an array of objects, each of which specifies a set of ROIs. Each object (set) is in a format identical to the annotation track configuration object format.

#### Configuration Based ROIs ####

The following example defines both global and track-specific ROI sets.  Full example is available here [examples/roi/roi.html](https://github.com/igvteam/igv.js/blob/master/examples/roi/roi.html)

* An ```roi``` property on the configuration specifies an array of two global ROI sets. The first set derives its ROIs from a URL, the send set from a feature list.

* An ```roi``` property on a track configuration specifies a track-specific ROI set.

* ROI sets can be defined from either 
    * features in an annotation file, such as a BED or GFF file, or
    * features with properties ```{chr, start, end}``` specified explicitly in an array. 


```
    const config = {
  
        genome: "hg19",

        // Global ROI sets
        roi:
            [
                // 1. Global ROI set defined by features in a BED file
                {
                    name: 'ROI set 1',
                    url: 'https://s3.amazonaws.com/igv.org.test/data/roi/roi_bed_1.bed',
                    indexed: false,
                    color: "rgba(94,255,1,0.25)"
                },
                
                // 2. Global ROI set defined by explicitly specifying features in an array
                {
                    name: "ROI set 2",
                    color: "rgba(3,52,249,0.25)",
                    features: [
                        {
                            chr: "chr1",
                            start: 67670000,
                            end: 67671080
                        },
                        {
                            chr: "chr1",
                            start: 67672095,
                            end: 67673993
                        }
                    ]
                }
            ],

        //  Track specific ROI set
        tracks:
            [
                {
                    name: 'Some features',
                    url: 'https://s3.amazonaws.com/igv.org.test/data/roi/some_features.bed',
                    indexed: false,
                    roi:
                        [
                            {
                                name: 'Track Based ROI Set',
                                url: 'https://s3.amazonaws.com/igv.org.test/data/roi/roi_bed_2.bed',
                                indexed: false,
                                color: "rgba(255,1,199,0.25)"
                            },
                        ]
                }
            ]
    }
```

### Dynamically Loaded ROIs ###

A list of global ROI sets can be loaded dynamically at runtime with the browser function ```loadROI(config)``.  As in
the example above, the features can be specified as either a URL to an annotation file, or as an explicit array.

```
     const dynamic_roi_config =
        [
            {
                color: "rgba(237,72,155,0.72)",
                features:
                    [
                        {
                            chr: "chr1",
                            start: 67655415,
                            end: 67655611
                        },
                        {
                            chr: "chr1",
                            start: 67664225,
                            end: 67666281
                        }
                    ]
            }
        ]    
     browser.loadROI(dynamic_roi_config)
```

### User Defined ROIs ###

As of igv.js version 2.13.0, users can interactively create global ROIs by sweeping out extents on the ruler 
area while holding down the Shift key.  Interactively defined ROIs can be removed, or assigned an extension, through
a popup menu accssed by clicking on the region header.

The Browser API function ```getUserDefinedROIs()``` returns a promise to return user defined regions as an array of features.

### clearROIs() ###
All global ROIs can be removed by calling ```browser.clearROIs()```. 
