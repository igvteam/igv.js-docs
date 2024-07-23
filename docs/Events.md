# Browser events

IGV provides a simple event system to listen for user interactions.  Event handlers can be attached to the "Browser" object with the ```on``` function

```
browser.on(event, handler)
browser.off(event)
```
* event (String) - the event type, see below
* handler (function) - handler function for the event.  The function may be passed data as described below


Event type | Description | Data
-----      | ---- | -----
trackremoved | Called when one or more tracks are removed, either by the user or through the API.  See [track-reorder.html](https://github.com/igvteam/igv.js/blob/master/examples/events/track-reorder.html). | Array of track objects removed
trackdrag | Called repeatedly while the user pans the genome view.  Warning this event can trigger many calls | 
trackdragend | Called upon completion of of a drag (horizontal pan).
locuschange | Called when the genome locus is changed.  Note, as with trackdrag this event can trigger many calls.  See [locus-change.html](https://github.com/igvteam/igv.js/blob/master/examples/events/locus-change.html)  | Array of ReferenceFrame objects, or for versions < 2.10.0 a single ReferenceFrame (see below).  In multi-locus view the array contains a ReferenceFrame for each viewport.
trackclick | Called when the user clicks on a feature in a track.  This event can be used to customized the igv popover window that displays information about the feature, or to take some other custom action.  To customize the popover window the callback function should return a string of valid html to be displayed (see [custom-track-popover.html](https://github.com/igvteam/igv.js/blob/master/examples/events/custom-track-popover.html)).  To prevent the igv popover window from displaying return ```false``` (see [custom-track-click.html](https://github.com/igvteam/igv.js/blob/master/examples/events/custom-track-click.html))  | popupData (see below)
trackorderchanged | Called when the track order changes | ordered array of track names




### ReferenceFrame

Object specifying the genome range of the current view.

```
{
   chr : string
   start: number  ("0" based coordinates)
   end: number
   getLocusString():  function - return a representation of the form ```chr:start-end```,  where start is in "1" based coordinates
}
```

### popupData 

Array of properties for feature clicked as name/value pairs ```[{name, value}, {name, value}, ...]```



## Example

```
browser.on('locuschange', function (referenceFrame) {
                        window.location.replace(HASH_PREFIX + referenceFrame.label);
                    });
```


# Track events

## _Experimental -- not released and subject to change or removal_

```
track.on(event, handler)
```

* event (String) - the event type, see below
* handler (function) - handler function for the event.  The function may be passed data as described below



Event type | Description | Data
-----      | ---- | -----
action | Triggered on an explicit user action, usually from a menu.  Only a subset of actions are supported, see table below.  | See user action table below |




#### User actions

Action | Description | Data
-----      | ---- | -----
sort | Sort track data.  Supported by _alignment_ and _seg_ tracks, no-op for other track types | sortObject
setcolor | Set the track color | color-string (e.g. ```rgb(0,0,150)```
setAltColor | Set the "alt" color.  Interpretation of alt color depends on the track type.  | color-string

