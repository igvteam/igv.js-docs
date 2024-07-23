# Merged

The merged track (`type = 'merged'`) is used to overlay multiple wig tracks.

#### Example

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
