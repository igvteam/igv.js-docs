The `igv.createBrowser` function is used to create an igv Browser object and insert it into your dom. The function takes 
two arguments: (1) The parent element; the browser object will be inserted into the DOM as a child of this element, and
(2) A configuration object that defines the browser's initial state. `igv.createBrowser` returns a promise which 
resolves to the browser object upon completion.  The browser object should be saved by the client program for future 
calls to the API.

The example below creates an igv browser initialized with the hg19 reference genome.  

```html

<div id="igv_div"

<script type="module">

    import igv from "https://cdn.jsdelivr.net/npm/igv@3.0.0/dist/igv.esm.min.js"

    const div = document.getElementById("igv_div")
    
    const config = {
            genome: "hg19"
    }

    const browser = await igv.createBrowser(div, config) 
    
</script>    

```

## Browser configuration options ##


The object that configures the browser's initial state includes details for the reference genome, track default settings, 
the initial set of loaded tracks, and the initial view locus. It also controls some aspects of the user interface.  
All fields are optional except one of either **genome** or **reference**.

Option  | Description                                                                                                                                                                                                                                                                                                                                                        | Default
------ |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ------------
genome | String identifier defining genome (e.g. "hg19").  See [Reference Genome](Reference-Genome) for details and list of supported identifiers. **Note: One (but only one) of either _genome_ or _reference_ properties must be set.**                                                                                                                                   
reference | Object defining reference genome.  See [Reference Genome](Reference-Genome) for details. **Note: One (but only one) of either _genome_ or _reference_ properties must be set.**                                                                                                                                                                                    |
flanking  | Distance (in bp) to pad sides of feature  on successful search for gene or other annotation.                                                                                                                                                                                                                                                                       | 1000
genomeList | URL to a json file containing an array of genome reference definitions, _or_ an inline json array.  These genome definitions are appended to the default list unless `loadDefaultGenomes` is false.  Genomes can be specified from the list by ID with the "genome" property.  __NOTE: the genomeList is globally shared by all igv browser instances on a page.__ |
loadDefaultGenomes | Load the default genome list specified in `genomeList` on startup from [https://igv.org/genomes/genomes.json](https://igv.org/genomes/genomes.json)                                                                                                                                                                                                                | true
locus | Initial genomic location(s).  Either a string or an array of strings.  If an array a viewport is created for each location.                                                                                                                                                                                                                                        | 
minimumBases | Minimum window size in base pairs when zooming in                                                                                                                                                                                                                                                                                                                  | 40
queryParametersSupported | If true support initialization by query parameters.                                                                                                                                                                                                                                                                                                                | false
search | Object defining a web service for supporting search by gene or other annotation.  See object details below.  Optional.   Current a default service is provided for human (hg19, hg38) and mouse (mm10) assemblies.                                                                                                                                                 |
showAllChromosomes | Show all chromosome (sequences) in the pulldown control irrespective of size.  Set to false to filter sequences < 1/50 the mean length.                                                                                                                                                                                                                            | true
showChromosomeWidget | Show a chromosome (sequence) pulldown control.                                                                                                                                                                                                                                                                                                                     | true
showNavigation | Show basic navigation controls (search, zoom in, zoom out).                                                                                                                                                                                                                                                                                                        | true
showIdeogram | Show an ideogram of the chromosome in the header of the browser viewport.                                                                                                                                                                                                                                                                                          | true
showSVGButton | Show button that saves browser as SVG file.                                                                                                                                                                                                                                                                                                                        | true
showRuler | Show a genomic ruler track.                                                                                                                                                                                                                                                                                                                                        | true
showCenterGuide | Show a pair of vertical lines, or single line when zoomed out, framing the base position at the center of view                                                                                                                                                                                                                                                     | false
showCursorTrackGuide | Show a vertical line that follows the cursor                                                                                                                                                                                                                                                                                                                       | false
trackDefaults | Embedded object defining default settings for specific track types (see table below).                                                                                                                                                                                                                                                                              |
tracks | Array of configuration objects defining tracks initially displayed when app launches.                                                                                                                                                                                                                                                                              |
roi | Array of track-like configuration objects defining regions of interest.  These regions will be overlaid on all tracks.                                                                                                                                                                                                                                             |
oauthToken | oauth access token                                                                                                                                                                                                                                                                                                                                                 |
apiKey | Google API key.  Optional                                                                                                                                                                                                                                                                                                                                          |
clientId | Google client ID.  Optional.                                                                                                                                                                                                                                                                                                                                       |
genomeList | An array of genome json objects, or url to an array of genome json objects.  If present genomes can be specified by id with the ```genome``` field above.  This list is added to the igv.js default list unless ```loadDefaultGenomes: false```                                                                                                                    |
loadDefaultGenomes | Boolean indicating whether or not the igv.js default genome list should be loaded.   Currently this list is loaded from                                                                                                                                                                                                                                            | true 
nucleotideColors | Color table for nucleotides in sequence an bam tracks.  Object with keys "A", "C", "T", "G", and "N"                                                                                                                                                                                                                                                               | 
showSampleNames | Controls display of sample names for track types that support them (VCF with genotypes, SEG, and MUT)                                                                                                                                                                                                                                                              |

#### _search_ object details

The search object defines a webservice for fetching genomic location given a gene name or other symbol.  The service should return a JSON object with the following structure.  The results array is an array of objects with a chromosome, start, and end field.  The names of these fields are specified in the configuration object.   The end field is optional, if not included end = start + 1.

    {
      <resultsField> : <array of results>
    }

Option  | Description | Default
------ | ------- | ------------
url | URL to search service.  The URL must include the string `$FEATURE$`.  This string will be replaced by the symbol being queried.  if the URL contains the string `$GENOME$` is will be replaced by the current genome ID. |
resultsField | JSON field name for property containing the array of results.  | Treats the response as an array of results.
coords | Indicates genomic coordinate convention used. Possible values are 0 and 1 | 1
chromosomeField | JSON field name for the chromosome property | chromosome
startField | JSON field name for the start position property | start
endField | JSON field name for the end position property | end

The results array contain objects with chromosome, start, and end fields named as specified above.  The type of the chromosome field is string, the type of start and end fields is integer.

E.g. a search for TP53 against hg19 using the defaults should look like this:

    [{"chromosome":"chr17","start":7572927,"end":7579912}]




## Browser removal

To remove an igv browser instance call

```
igv.removeBrowser(browser)
```
To remove all igv browsers

```
igv.removeAllBrowsers()
```
