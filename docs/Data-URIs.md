Many track types support a data URI in the url fields.   The data uri contains the compressed,  uuencoded, 
content of a supported file type.  

Most file formats are supported, with the exception of bigwig, bigbed, and tdf.

To produce a data URI from a data file

1. gzip the data file,  **unless it is a bam file**.  
2. base64 encode the contents of the gzipped or bam file
3. do the following character replacements on the encoded string  ```encoded.replace(/\+/g, '.').replace(/\//g, '_').replace(/=/g, '-')``
4. construct a data URI as follows

```
data:application/gzip;base64,<encoded string>
```

See https://github.com/igvteam/igv.js-reports for a utility for producing a self-contained html file with data URIs.



