# OverlappingMarkerSpiderfier

[![npm version](https://img.shields.io/npm/v/OverlappingMarkerSpiderfier.svg)](https://www.npmjs.com/package/OverlappingMarkerSpiderfier)
[![GitHub stars](https://img.shields.io/github/stars/nmccready/OverlappingMarkerSpiderfier.svg?style=social)](https://github.com/nmccready/OverlappingMarkerSpiderfier)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Google Earth-style spiderfying for overlapping markers on Google Maps API v3.**

Ever noticed how in Google Earth, marker pins that overlap each other spring apart gracefully when you click them? This library brings that behavior to the Google Maps JavaScript API v3. Small clusters (up to 8) fan into a circle; larger groups spiral outward for efficient use of space.

The compiled code has no dependencies beyond Google Maps. Under 3KB minified and gzipped.

> Originally created by [George MacKerron](https://github.com/jawj) for [Mappiness](http://www.mappiness.org.uk/maps/). This fork is maintained for npm/bower compatibility.

## Install

```bash
npm install OverlappingMarkerSpiderfier
# or
bower install OverlappingMarkerSpiderfier
```

Or include directly:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/OverlappingMarkerSpiderfier/1.0.3/oms.min.js"></script>
```

## Demo

See the [live demo](http://jawj.github.com/OverlappingMarkerSpiderfier/demo.html) (reload to reposition random markers).

## Quick Start

```js
const map = new google.maps.Map(document.getElementById('map'), {
  center: { lat: 50, lng: 0 },
  zoom: 6,
});

// Create the spiderfier
const oms = new OverlappingMarkerSpiderfier(map, {
  markersWontMove: true,
  markersWontHide: true,
  keepSpiderfied: true,
});

// Add a click listener
const infoWindow = new google.maps.InfoWindow();
oms.addListener('click', (marker) => {
  infoWindow.setContent(marker.desc);
  infoWindow.open(map, marker);
});

// Close info window on spiderfy
oms.addListener('spiderfy', () => infoWindow.close());

// Add markers
markers.forEach((m) => oms.addMarker(m));
```

## Clustering vs Spiderfying

The [marker clustering library](https://github.com/googlemaps/js-markerclusterer) groups nearby markers at lower zoom levels. Spiderfying complements clustering by handling markers at the **same location** or very close together at max zoom — where clustering can't help users distinguish individual markers.

They work great together: clustering at low zoom, spiderfying at high zoom.

## API

### Constructor

```js
new OverlappingMarkerSpiderfier(map, options?)
```

#### Options

| Option | Default | Description |
|--------|---------|-------------|
| `markersWontMove` | `false` | Skip position change tracking (saves memory) |
| `markersWontHide` | `false` | Skip visibility change tracking (saves memory) |
| `keepSpiderfied` | `false` | Keep markers spiderfied after clicking one |
| `nearbyDistance` | `20` | Pixel radius for overlap detection |
| `circleSpiralSwitchover` | `9` | Marker count threshold for spiral vs circle layout |
| `legWeight` | `1.5` | Thickness of connecting lines |

### Marker Management

| Method | Description |
|--------|-------------|
| `addMarker(marker)` | Track a marker |
| `removeMarker(marker)` | Stop tracking a marker (doesn't remove from map) |
| `clearMarkers()` | Stop tracking all markers |
| `getMarkers()` | Get array of all tracked markers (copy) |

### Event Listeners

| Method | Description |
|--------|-------------|
| `addListener(event, fn)` | Listen for `'click'`, `'spiderfy'`, or `'unspiderfy'` |
| `removeListener(event, fn)` | Remove a specific listener |
| `clearListeners(event)` | Remove all listeners for an event |
| `unspiderfy()` | Manually unspiderfy all markers |

### Advanced

| Method | Description |
|--------|-------------|
| `markersNearMarker(marker, firstOnly?)` | Find markers within `nearbyDistance` of a marker |
| `markersNearAnyOtherMarker()` | Find all markers that overlap with at least one other |

### Properties

Customize leg colors per map type:

```js
oms.legColors.usual[google.maps.MapTypeId.SATELLITE] = '#fff';
oms.legColors.highlighted[google.maps.MapTypeId.SATELLITE] = '#f00';
```

## Build from Source

```bash
npm install
bower install
gulp
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

## Sponsor

If you find this project useful, consider [sponsoring @nmccready](https://github.com/sponsors/nmccready) to support ongoing maintenance and development. ❤️

## License

[MIT](http://www.opensource.org/licenses/mit-license.php)
