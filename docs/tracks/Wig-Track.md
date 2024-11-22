<p class="page-title">Wig Track</p>

The wig track (`type = 'wig'`) displays quantititive data as either a bar chart, line plot, or points.

## File formats

* [wig](https://genome.ucsc.edu/goldenPath/help/wiggle.html)
* [bedGraph](https://genome.ucsc.edu/goldenPath/help/bedgraph.html)
* [bigwig](https://genome.ucsc.edu/goldenPath/help/bigWig.html)


## Configuration Options

[General options](Tracks.md#options-for-all-track-types)


| Property       | Description                                                                                                                                                                                                           | Default            |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| autoscale      | Autoscale track to maximum value in view                                                                                                                                                                              |                    |
| autoscaleGroup | Identifier for an autoscale group.  Tracks with the same identifier are autoscaled together.                                                                                                                          |                    |
| min            | Sets the minimum value for the data (y-axis) scale.  Usually zero.                                                                                                                                                    | 0                  |
| max            | Sets the  maximum value for the data (y-axis) scale. This value is ignored if autoscale = true                                                                                                                        |                    |
| color          | Track color as as an "rgb(,,,)" string, a hex string, or css color name.  Alternatively a function can be supplied which takes value as a parameter and returns a color.                                              | "rgb(150,150,150)" |
| altColor       | If supplied, used for negative values.  See description of color field above.                                                                                                                                         |                    |
| colorScale     | Color scale for heatmap (graphType = "heatmap" ).  See description below                                                                                                                                              |                    |
| guideLines     | Draws a horizontal line for each object in the given array: ```guideLines: [ {color: [color], y: [number], dotted: [bool]} ]```  Note: y value should be between min and max or it will not show.                     |                    |
| graphType      | Type of graph: "bar", "points", "line", or "heatmap"                                                                                                                                                                  | bar                |
| flipAxis       | If true, track is drawn "upside down" with zero at top                                                                                                                                                                | false              |
| windowFunction | Applicable to tracks created from **bigwig** and **tdf** files.  Governs how data is summarized when zooming out.  Options correspond to data in the file but generally include **`min`**, **`max`**, and **`mean`**. | mean               |


#### Color Scale Objects

A colorScale is described with an object of the form

```js

{
    "type": string,
    "min": number,
    "mid": number,
    "max": number,
    "minColor": string,
    "midColor": string,
    "maxColor": string
}

```

Two types of color scales are supported

1. type = 'gradient' :  A gradient color scale that varies linearlly from minColor to maxColor between min and max values. The mid and midColor properties are ignored.
2. type = 'diverging' : Consist of 2 gradient color scales covering the range min->mid and mid-> max

The color strings can be one of:

* Hex format: #rrggbb
* RGB format: rgb(red, green, blue)
* Name format: name



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
