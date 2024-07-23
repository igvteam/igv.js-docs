# Segmented Copy Number Track

```
type = 'seg'
```

Displays segmented copy number values as a heatmap, red = amplifications, blue = deletions.
There are 2 common conventions for values in segmented copy number
files,  the copy number itself, and a log score computed from

    score = 2 * log2( copyNumber / 2)

The value type is indicated by the isLog property.  If no value is set
for this property it is inferred by the values in the file, all positive
values => isLog = false.  The presence of any negative values => isLog = true.



Property | Type | Description | Default
-------- | ---- | ----------- | -------
isLog    | boolean | True if values are  2 * log2(copyNumber / 2). | _computed_
displayMode | string | One of "EXPANDED", "SQUISHED", or "FILL".  The value affects the sample height (height of each row).  The "FILL" value will result in all samples visible in the track view.  | "EXPANDED"
sort | object | An object specifying initial sort order(options).  See example below. |




```javascript
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