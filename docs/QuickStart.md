
## Installation
igv.js consists of a single javascript file with no external dependencies.

Pre-built files for script include, AMD, or CJS module systems (igv.min.js) and an ES6 module (igv.esm.min.js)
can be downloaded from [https://cdn.jsdelivr.net/npm/igv@2.15.13/dist/](https://cdn.jsdelivr.net/npm/igv@2.15.13/dist/).

To import igv as an ES6 module

```javascript
import igv from "https://cdn.jsdelivr.net/npm/igv@2.15.13/dist/igv.esm.min.js"
``` 

Or as a script include (defines the "igv" global)

```html
<script src="https://cdn.jsdelivr.net/npm/igv@2.15.13/dist/igv.min.js"></script>
```   

Alternatively you can install with npm

```npm install igv```

and source the appropriate file for your module system (igv.min.js or igv.esm.min.js)  in node_modules/igv/dist.


## Usage

To create an igv.js ***browser*** supply a container div
and an initial configuration defining the reference genome, initial tracks, and other state to the
function ```igv.createBrowser(div, config)```.

This function returns a promise for an igv.Browser object which can used to control the browser.  For example, to open
an igv.js browser with an alignment track opened at a specific locus:

```
      const igvDiv = document.getElementById("igv-div")
      
      const options =
        {
            genome: "hg38",
            locus: "chr8:127,736,588-127,739,371",
            tracks: [
                {
                    "name": "HG00103",
                    "url": "https://s3.amazonaws.com/1000genomes/data/HG00103/alignment/HG00103.alt_bwamem_GRCh38DH.20150718.GBR.low_coverage.cram",
                    "indexURL": "https://s3.amazonaws.com/1000genomes/data/HG00103/alignment/HG00103.alt_bwamem_GRCh38DH.20150718.GBR.low_coverage.cram.crai",
                    "format": "cram"
                }
            ]
        }

        const browser = await igv.createBrowser(igvDiv, options)
      
```

## More

* [Browser creation](Browser-Creation.md)
* [Browser API](Browser-API.md)
* [Tracks](tracks/Tracks.md)

