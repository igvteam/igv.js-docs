# Interact Track  

The interact track (`type = 'interact'`) visualizes pairwise interactions between genome regions as arcs.

### File formats

* [bedpe](https://bedtools.readthedocs.io/en/latest/content/general-usage.html)
* [interact](https://genome.ucsc.edu/goldenPath/help/interact.html) 
* [bigInteract](https://genome.ucsc.edu/goldenPath/help/interact.html)

### Options

[General options](Tracks.md#options-for-all-track-types)

Property  | Description | Default
------ | ------- | ------------
arcOrientation | Direction of arcs (boolean, true ->up) | true
thickness | Line thickness | 2


#### Example

```javascript
{
   url: "https://s3.amazonaws.com/igv.org.test/data/gm12878_loops.bedpe.gz",
   type: "interaction",
   format: "bedpe",
   arcOrientation: true,
   thickness: 2,     
   name: "GM12878 Loops (bedpe)"
}
```
