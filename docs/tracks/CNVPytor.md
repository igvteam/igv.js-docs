<p class="page-title">CNVpytor Track</p>

The CNVpytor track (type = `'cnvpytor'`) is a tool for copy number analysis from read depth and B-allele frequency 
(BAF) of variants. As input, it takes BAM and/or VCF files. The analyzed data is stored in pytor file and can be viewed 
using `cnvpytor` track in igv.  One can also use a whole genome VCF file as an input to the `cnvpytor` track. In such a 
case  read depth and BAF information are calculated on-fly.

For details: https://github.com/abyzovlab/CNVpytor

## File formats
`cnvpytor` track can be loaded from either `pytor` or VCF files.

### pytor
  *    [CNVpytor](https://github.com/abyzovlab/CNVpytor)-produced pytor file.
  *    Option to switch among available bin sizes.
  *    Option to switch among available CNV callers.
  *    Signals: 
    * Raw Read Depth
    * GC Corrected Read depth
    * CNV call 
    * Maximum of BAF likelihood

### vcf

Whole genome [VCF](https://samtools.github.io/hts-specs/VCFv4.2.pdf) file with constraints.

* Indexed VCF files are not supported as data for the entire genome is required for the calculations. 
* Both  AD (Read Depth information) and GT (Genotype information) fields are required . 
* Currently only a single genotype field is supported. Multi-sample VCFs are not supported. 
* Currently, only ReadDepth CNV caller is available. 
* Signals:
    * Raw Read Depth
    * Partition
    * CNV call 
    * Maximum of BAF likelihood

Loading the data may take time, as it reads the entire VCF file and calculates the signals.



## Configuration Options

[General options](Tracks.md#options-for-all-track-types)

| Property    | Description                                                                                                                                                                                   | Default                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| signal_name | Available Signal names <ul><li>rd_snp : Read Depth and BAF Likelihood</li><li>rd : Read depth</li><li> snp : BAF likelihood</li></ul>                                                         | rd_snp                             |
| cnv_caller  | Name of CNV caller <br>Options<ul><li> ReadDepth: Uses Read depth information only</li><li>2D: Uses both Read depth and BAF information</li></ul>  Shows data based on available caller data. | 2D                                 |
| bin_size    | Bin size <ul> <li>pytor file: Bin size should be avialable in the pytor file</li><li>vcf: Bin size should be multiple of 10,000</li></ul>                                                     | 100000                             |
| colors      | Color of the signals. Signal details are in file format section.                                                                                                                              | ['gray', 'black', 'green', 'blue'] |

#### Examples

##### pytor file as input

```javascript
{
    id: "pytor_track",
    type: "cnvpytor",
    name: "HepG2 pytor",
    url: "https://storage.googleapis.com/cnvpytor_data/HepG2_WGS.pytor",
}
```

######  View


![](https://github.com/arpanda/igv.js/assets/21196859/8a18f9c0-6dc9-4e21-af66-77d2b2de7e29)


##### VCF file as input

```javascript
{
    id: "cnvpytor_track_vcf",
    type: "cnvpytor",
    name: "HepG2 VCF",
    url: "https://storage.googleapis.com/cnvpytor_data/HepG2.vcf.gz",
}
```

######  View
![](https://github.com/arpanda/igv.js/assets/21196859/b90ebd48-eebd-4d98-af9d-888c365d6e4d)
