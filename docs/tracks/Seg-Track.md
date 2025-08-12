<p class="page-title">Segmented Copy Number Track</p>

The segmented copy number (`type="seg"`) track displays segmented copy number values as a heatmap, 
red = amplifications, blue = deletions. There are 2 common conventions for values in segmented copy number
files,  the copy number itself, and a log score computed from

    score = 2 * log2( copyNumber / 2)

The value type is indicated by the isLog property.  If no value is set  for this property it is inferred by the values 
in the file, all positive values => isLog = false.  The presence of any negative values => isLog = true.

## File Formats

### seg

A seg file (file extension .seg) is a tab-delimited text file that lists loci and associated numeric values. 
The first row contains column headings, and each subsequent row contains a locus and an associated numeric value. 
IGV ignores the column headings. It reads the first four columns as track name, chromosome, start location, and end location. 
It reads the last column as the numeric value for that locus (if the value is non-numeric, IGV ignores the row). 
IGV ignores all other columns.

The segmented data file format is the output of the Circular Binary Segmentation algorithm (Olshen et al., 2004).

## Example

```
{
  type: "seg",
  format: "seg",
  url: "https://s3.amazonaws.com/igv.org.demo/GBM-TP.seg.gz",
  indexed: false,
  isLog: true,
  name: "GBM Copy # (TCGA Broad GDAC)",
  sort: {
     direction: "DESC",     // ASC | DESC
     chr : "chr7",
     start: 55174641,
     end: 55175252
  }
}

```

## Configuration Options

[General options](Tracks.md#options-for-all-track-types)

| Property    | Type    | Description                                                                                                                                                                  | Default    |
|-------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------|
| isLog       | boolean | True if values are  2 * log2(copyNumber / 2).                                                                                                                                | _computed_ |
| displayMode | string  | One of "EXPANDED", "SQUISHED", or "FILL".  The value affects the sample height (height of each row).  The "FILL" value will result in all samples visible in the track view. | "EXPANDED" |
| sort        | object  | An object specifying initial sort order(options).  See example below.                                                                                                        |            |
| groupBy     | string  | A sample attribute name to group samples by.  If specified the samples will be grouped by the property value.  Applicable if sample information has been loaded.             |            |