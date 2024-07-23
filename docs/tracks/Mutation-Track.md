
```
type = 'mut'
```

Displays mutation data from  [TCGA mut](https://docs.gdc.cancer.gov/Encyclopedia/pages/Mutation_Annotation_Format_TCGAv2/) or [maf](https://docs.gdc.cancer.gov/Data/File_Formats/MAF_Format/) formats



Property | Type | Description | Default
-------- | ---- | ----------- | -------
format    | string    | Either ```mut``` or ```maf``` | Inferred from file extension
displayMode | string | One of EXPANDED, SQUISHED, COLLAPSED.   | EXPANDED



Example

```javascript

    {
        type: 'mut',
        format: 'maf',
        url: 'https://s3.amazonaws.com/igv.org.demo/TCGA.BRCA.mutect.995c0111-d90b-4140-bee7-3845436c3b42.DR-10.0.somatic.maf.gz',
        indexed: false,
        height: 700,
        displayMode: "EXPANDED",
    }


```