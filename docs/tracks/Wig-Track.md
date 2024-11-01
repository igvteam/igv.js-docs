<p class="page-title">Wig Track</p>

The wig track (`type = 'wig'`) displays quantititive data as either a bar chart, line plot, or points.

## File formats

* [wig](https://genome.ucsc.edu/goldenPath/help/wiggle.html)
* [bedGraph](https://genome.ucsc.edu/goldenPath/help/bedgraph.html)
* [bigwig](https://genome.ucsc.edu/goldenPath/help/bigWig.html)


## Configuration Options

[General options](Tracks.md#options-for-all-track-types)

| Property       | Description                                                                                                                                                                                       | Default            |
|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| autoscale      | Autoscale track to maximum value in view                                                                                                                                                          |                    |
| autoscaleGroup | Identifier for an autoscale group.  Tracks with the same identifier are autoscaled together.                                                                                                      |                    |
| min            | Sets the minimum value for the data (y-axis) scale.  Usually zero.                                                                                                                                | 0                  |
| max            | Sets the  maximum value for the data (y-axis) scale. This value is ignored if autoscale = true                                                                                                    | No default         |
| color          | Track color as as an "rgb(,,,)" string, a hex string, or css color name.  Alternatively a function can be supplied which takes value as a parameter and returns a color.                          | "rgb(150,150,150)" |
| altColor       | If supplied, used for negative values.  See description of color field above.                                                                                                                     |                    |
| guideLines     | Draws a horizontal line for each object in the given array: ```guideLines: [ {color: [color], y: [number], dotted: [bool]} ]```  Note: y value should be between min and max or it will not show. |                    |
| graphType      | Type of graph, either "bar", "points"                                                                                                                                                             | bar                |
| flipAxis       | If true, track is drawn "upside down" with zero at top                                                                                                                                            | false              |
| windowFunction | Applicable to tracks created from **bigwig** and **tdf** files.  Governs how data is summarized when zooming out.  Options include **`min`**, **`max`**, **`none`** and **`mean`**.               | mean               |

## Example

```javascript
{
  type: "wig",
  name: "CTCF",
  url: "https://www.encodeproject.org/files/ENCFF356YES/@@download/ENCFF356YES.bigWig",
  min: "0",
  max: "30",
  color: "rgb(0, 0, 150)",
  guideLines: [
    {color: 'green', dotted: true, y: 25}, 
    {color: 'red', dotted: false, y: 5}
  ]
}

```
