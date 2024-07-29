<p class="page-title">Merged Track</p>

The merged track (`type = 'merged'`) is used to overlay multiple wig tracks. 



## Configuration Options

[General options](Tracks.md#options-for-all-track-types)

| Property | Description                                                                     | Default |
|----------|---------------------------------------------------------------------------------|---------|
| alpha    | Alpha transparency to apply to individual track colors.  Number between 0 and 1 | 0.5     |



## Example

```javascript
                {
                    name: "Merged",
                    height: 50,
                    type: "merged",
                    tracks: [
                        {
                            "type": "wig",
                            "format": "bigwig",
                            "url": "https://www.encodeproject.org/files/ENCFF000ASJ/@@download/ENCFF000ASJ.bigWig",
                            "color": "red"
                        },
                        {
                            "type": "wig",
                            "format": "bigwig",
                            "url": "https://www.encodeproject.org/files/ENCFF351WPV/@@download/ENCFF351WPV.bigWig",
                            "color": "green"
                        }
                    ]
                }
```