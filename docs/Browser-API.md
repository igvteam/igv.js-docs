After initialization, the browser can be controlled using the functions described below. 


## loadGenome


__async__

Returns a promise to load a reference genome. 

```js
browser.loadGenome(config)
```

See the [reference object](Reference-Genome.md) description for details on configuration options.

**Example**

```
browser.loadGenome(
{
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

## loadSessionObject

__async__

Load a session object, also referred to as a browser configuration object.  See [Browser Creation](Browser-Creation) for details.   
Loading a session will clear the current reference genome and all tracks.

```js
browser.loadSessionObject(config)
```

## loadSession

__async__

Load a session by url or a javascript File blob, which should point to a valid session json object.  The json 
is a representation of a browser configuration object described in the [Browser Creation](Browser-Creation) section, but does not 
support parameter values not representable as json such as functions and promises.

The method takes an object with a single property, `url`, which can be a URL or a local File blob.

```js
browser.loadSession({url})
```

## loadTrack

__async__

Load a track and return a promise for the track object. See the [Tracks page](tracks/Tracks.md) for more detail on configuration options.

```js
const track = await browser.loadTrack(config)
```

**Example**

```js
browser.loadTrack({
  url: 'http://data.broadinstitute.org/igvdata/1KG/b37/data/HG02450/alignment/HG02450.mapped.ILLUMINA.bwa.ACB.low_coverage.20120522.bam',
  label: 'HG02450'
    })

```

## loadSampleInfo

__async__

Load a [sample information](SampleInfo.md) file.  

The method takes an object with a single property, `url`, which can be a URL or a local File blob.

```js
browser.loadSampleInfo({url})
```


## findTracks

Returns an array of tracks matching input critera.  Criteria can be specified as either

1. A property value pair
2. A function taking the track object as an argument and returning true or false

```js
const tracks = browser.findTracks(propertyOrFunction, value)
```

**Examples**
```js
            console.log("Find tracks by property 'id' with value 'T2':");
            const tracksById = browser.findTracks("id", "T2");
            for(let t of tracksById) {
                console.log(`  id=${t.id}   name=${t.name}`);
            }

            console.log("Find tracks by type 'wig'");
            const tracksByType = browser.findTracks("type", "wig");
            for(let t of tracksByType) {
                console.log(`  id=${t.id}   name=${t.name}`);
            }

            console.log("Find tracks by function function(track) {return track.name && track.name.startsWith('GM128')}");
            const tracksByFunction = browser.findTracks(function(track) {
                return track.name && track.name.startsWith('GM128');
            });
```

## removeTrack

Remove a track object from the browser.  

```js
browser.removeTrack(track)
```

## removeTrackByName

Remove track(s) whose "name" property matches the given name.

```js
browser.removeTrackByName(name)
```

## loadROI

__async__

Load an annotation file or array of annotation files to define regions of interest (ROIs).  Regions of interest are
overlaid on the genome view across all tracks.  See [Regions of Interest](Regions-of-Interest.md) for more details.

```js
browser.loadROI(trackConfig | arrayOfTrackConfigs )
```

**Examples**
```js
browser.loadROI([
{
    name: 'ROI set 1',
    url: 'https://s3.amazonaws.com/igv.org.test/data/roi/roi_bed_1.bed',
    indexed: false,
    color: "rgba(68, 134, 247, 0.25)"
},
{
    name: 'ROI set 2',
    url: 'https://s3.amazonaws.com/igv.org.test/data/roi/roi_bed_2.bed',
    indexed: false,
    color: "rgba(0, 150, 50, 0.25)"
})
```
## clearROIs

Remove all regions of interest.

```js
browser.clearROIs()
```

## getUserDefinedROIs

__async__

Returns a promise for an array containing all user-defined ROIs (regions of interest created interactively during
an igv.js session).

```js
const rois = await browser.getUserDefinedROIs()
```

## search

__async__
       
Search by a locus, and change browser locus to corresponding region.

By default the search function uses a webservice to query positions of RefSeq genes for genome "hg19".  
This can be overriden during initialization by supplying a url to a custom webservice.  
For details see [Browser initialization](https://github.com/igvteam/igv.js/wiki/Browser).

```js
browser.search(locus)
```

**Examples**
```js
browser.search('EGFR')
browser.search('chr10:1000-2000')
```

Multiple loci can be passed as space delimited list.  This will result in a multi-locus view.

```js
browser.search('chr10:1000-2000 EGFR')
```

This function returns a promise which resolves to ```true``` if the symbol was found, ```false``` otherwise.

## zoomIn

Zoom in by a factor of 2 

```js
browser.zoomIn()
```

## zoomOut

Zoom out by a factor of 2 

```js
browser.zoomOut()
```

### currentLoci

Return the current genomic region as a locus string, or an array of locus strings if in multi-locus view

```js
const locusStringOrArray = browser.currentLoci()
```

## visibilityChange

Signal a change in visibility, typically from hidden  (e.g. display:none) to shown  (e.g. display:block).  
This is necessary because changes in display attribute do not trigger events.

```js
browser.visibilityChange()
```

If multiple igv browsers are on a page, the function ```igv.visibilityChange``` can be used to signal all.

```js
    igv.visibilityChange();
```


## toJSON

Return the current state of the browser as a JSON style object.  This object can be loaded with loadSessionObject.
Note the returned value is a jsonifiable object, not a json string.

```js
   const json = browser.toJSON();
```

## compressedSession

Return a compressed, encoded, string representing the current browser state.   

```js
const sessionString = browser.compressedSession()
```

This string can be used to load the session on any page hosting an igv.js instance with a url of the form

```
https://myhost/mypage?sessionURL=blob:<compressed session string>
```

Note to reinstate the session ```queryParametersSupported``` must be set to true in the igv.js configuration.

## toSVG

Convert the browser contents to SVG format. This includes ideogram, ruler, and all tracks. The value returned by this 
function is a serialized SVG object.


```js
const svg = browser.toSVG()
```


## setCustomCursorGuideMouseHandler

Pass genomic location and mouse location with respect to trackContainer to the provided handler.
The data is transmitted as the cursor guide is manipulated across the track container.

The handler function will receive an object with the following properties
* bp - cursor-guide location. base-pair units.
* start - track start location. base-pair units.
* end - track end location. base-pair units.
* interpolant - cursor-guide location. 0 - 1 units.

**Example**

```js
browser.setCustomCursorGuideMouseHandler(({ bp, start, end, interpolant }) => {  });
```