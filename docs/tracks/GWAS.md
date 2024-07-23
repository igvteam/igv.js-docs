# GWAS Track

The GWAS track (`type = 'gwas'`) is used for visualizing genome wide association data as a "manhattan" style plot


## File formats

GWAS tracks can be loaded from either bed or [gwas](/fileformats/gwas) files.

### bed

5 columns (chromosome, start, end, name, value).  Additional columns are ignored.  Note bed format uses 
UCSC "0-based" positions, so the first position in the chromosome has start=0, end=1.

### gwas

This is a flexible tab-delimited format.  Any number of columns are supported but must include chromosome, 
position (1- based), and value (p-value or probability).  A column header is required.  Position of the required 
columns can be specified in the file column header, as described [here](../fileformats/gwas.md), 
or in the track configuration (see example).   Remaining columns are used for the popup text display.

#### Options

| Property             | Description                                                                                                                                                     | Default                                                                      |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| min                  | Sets the minimum value for the data (y-axis) scale.                                                                                                             | 0                                                                            |
| max                  | Sets the  maximum value for the data (y-axis) scale.                                                                                                            | 25 for p-value (-log10(pvalue)),  1 for posterior probability                |
| format               | File format, either "bed" or "gwas"                                                                                                                             |                                                                              |
| posteriorProbability | If true values are interpreted as probabilities with a range 0-1.  By default values are treated as p-values and plotted as -log10(P-value).                    | false                                                                        |
| dotSize              | Diameter of dots in pixels                                                                                                                                      | 3                                                                            |
| columns              | For ```gwas``` format only.  Declaration of column number for chromosome, position, and value. Optional, if specified all must be provided.  See example below. | ``` {chromosome: 2, position: 3, value: 4} ```                               |
| colorTable           | Object mapping chromosome names -> colors.  If supplied all chromosomes in data should be included.  See below for the default color table                      | See [below](https://github.com/igvteam/igv.js/wiki/GWAS#default-color-table) |


#### Example

```javascript
{
   type: "gwas",
   format: "gwas",
   name: "GWAS sample",
   url: "https://s3.amazonaws.com/igv.org.demo/gwas_sample.tsv.gz",
   indexed: false,
   columns: {
      chromosome: 12,
      position: 13,
      value: 28
   }
}
```

#### Default color table

```
{
    "X": "rgb(204, 153, 0)",
    "Y": "rgb(153, 204, 0)",
    "Un": "darkGray)",
    "1": "rgb(80, 80, 255)",
    "2": "rgb(206, 61, 50)",
    "2a": "rgb(210, 65, 55)",
    "2b": "rgb(215, 70, 60)",
    "3": "rgb(116, 155, 88)",
    "4": "rgb(240, 230, 133)",
    "5": "rgb(70, 105, 131)",
    "6": "rgb(186, 99, 56)",
    "7": "rgb(93, 177, 221)",
    "8": "rgb(128, 34, 104)",
    "9": "rgb(107, 215, 107)",
    "10": "rgb(213, 149, 167)",
    "11": "rgb(146, 72, 34)",
    "12": "rgb(131, 123, 141)",
    "13": "rgb(199, 81, 39)",
    "14": "rgb(213, 143, 92)",
    "15": "rgb(122, 101, 165)",
    "16": "rgb(228, 175, 105)",
    "17": "rgb(59, 27, 83)",
    "18": "rgb(205, 222, 183)",
    "19": "rgb(97, 42, 121)",
    "20": "rgb(174, 31, 99)",
    "21": "rgb(231, 199, 111)",
    "22": "rgb(90, 101, 94)",
    "23": "rgb(204, 153, 0)",
    "24": "rgb(153, 204, 0)",
    "25": "rgb(51, 204, 0)",
    "26": "rgb(0, 204, 51)",
    "27": "rgb(0, 204, 153)",
    "28": "rgb(0, 153, 204)",
    "29": "rgb(10, 71, 255)",
    "30": "rgb(71, 117, 255)",
    "31": "rgb(255, 194, 10)",
    "32": "rgb(255, 209, 71)",
    "33": "rgb(153, 0, 51)",
    "34": "rgb(153, 26, 0)",
    "35": "rgb(153, 102, 0)",
    "36": "rgb(128, 153, 0)",
    "37": "rgb(51, 153, 0)",
    "38": "rgb(0, 153, 26)",
    "39": "rgb(0, 153, 102)",
    "40": "rgb(0, 128, 153)",
    "41": "rgb(0, 51, 153)",
    "42": "rgb(26, 0, 153)",
    "43": "rgb(102, 0, 153)",
    "44": "rgb(153, 0, 128)",
    "45": "rgb(214, 0, 71)",
    "46": "rgb(255, 20, 99)",
    "47": "rgb(0, 214, 143)",
    "48": "rgb(20, 255, 177)"
}
```