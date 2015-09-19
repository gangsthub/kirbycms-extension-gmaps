# KirbyText Extension - GMaps

This is a Kirbytag to integrate Google Maps API in your website

## Requirements

- Plugin: [Webhelper](https://github.com/fanningert/kirbycms-extension-webhelper)
- jQuery
- Google Maps Javascript API

## Installation

First you need to install the plugin. In the second part copy the gmaps.js in to your `assets/js` directory and append this javascript file and the Google Maps Javascript API.

```php
...
<script src="//code.jquery.com/jquery-2.1.1.min.js"></script>
<script src="//maps.googleapis.com/maps/api/js?v=3.exp&signed_in=true&libraries=places&callback=initialize"></script>
<?php echo js('assets/js/gmaps.js'); ?>
...
```

### Asynchron Loading of jQuery and Google Maps Javascript API

For the asynchron loading of Javascript, you need a helper class. In my example I am using [script.js](https://github.com/ded/script.js).

```php
...
  <?php echo js("assets/js/script.min.js") ?>
  <script type="text/javascript">
    $script.path('<?php echo $kirby->urls->index; ?>/assets/js/');
    $script(['//code.jquery.com/jquery-2.1.1.min.js', 'gmaps'], 'pagejs');
    $script.ready('pagejs', function() {
      // I add the protcol, because without the protocol the script is searching in the local directory
      $script('https://maps.googleapis.com/maps/api/js?v=3.exp&signed_in=true&libraries=places&callback=initialize', 'googlemaps');
    });
  </script>
  </body>
</html>
```

### GIT

Go into the `{kirby_installation}/site/plugins` directory and execute following command.

```bash
$ git clone https://github.com/fanningert/kirbycms-extension-webhelper.git
$ git clone https://github.com/fanningert/kirbycms-extension-gmaps.git
```

### GIT submodule

Go in the root directory of your git repository and execute following command to add the repository as submodule to your GIT repository.

```bash
$ git submodule add https://github.com/fanningert/kirbycms-extension-webhelper.git ./site/plugins/kirbycms-extension-webhelper
$ git submodule add https://github.com/fanningert/kirbycms-extension-gmaps.git ./site/plugins/kirbycms-extension-gmaps
$ git submodule update --init --recursive
$ git submodule foreach --recursive git pull
```

### Manuell

## Update

### GIT

Go into the `{kirby_installation}/site/plugins/kirbycms-extension-gmaps` directory and execute following command.

```bash
$ git pull
```
Don't forget to update the requirement `kirbycms-extension-webhelper`.

### GIT submodule

Go in the root directory of your git repository and execute following command to update the submodule of this extension.

```bash
$ git submodule foreach --recursive git pull
```

## Documentation

### Kirby configuration values

| Kirby option | Default | Values | Description |
| ------------ | ------- | ------ | ----------- |
| `kirby.extension.gmaps.debug` | false | true/false | |
| `kirby.extension.gmaps.class` | 'googlemaps' | {string} | Class of the canvas element |
| `kirby.extension.gmaps.lat` | 0.0 | {number} | Initial LAT-Value for map |
| `kirby.extension.gmaps.lng` | 0.0 | {number} | Initial LNG-Value for map |
| `kirby.extension.gmaps.zoom` | 7 | {number 0-19} | Default zoom level |
| `kirby.extension.gmaps.controls.scale` | true | true/false | Activate/Deactivate scale control |
| `kirby.extension.gmaps.controls.maptypes` | 'roadmap,satellite,hybrid,terrain' | {string} | |
| `kirby.extension.gmaps.controls.maptype_selectable` | true | true/false | |
| `kirby.extension.gmaps.controls.maptype.default` | 'roadmap' | {string} | |
| `kirby.extension.gmaps.controls.draggable` | true | true/false | |
| `kirby.extension.gmaps.controls.zoomable` | true | true/false | |
| `kirby.extension.gmaps.controls.streetview` | true | true/false | |
| `kirby.extension.gmaps.controls.fitbounds.marker` | true | true/false | |
| `kirby.extension.gmaps.controls.fitbounds.kml` | true | true/false | |

### KirbyTag attributes

#### Root element (googlemaps)

| Option | Default | Values | Description |
| ------ | ------- | ------ | ----------- |
| class | | | see `kirby.extension.gmaps.class` |
| lat | | | see `kirby.extension.gmaps.lat` |
| lng | | | see `kirby.extension.gmaps.lng` |
| place | | {string} | alternative location definition over Place API |
| zoom | | | see `kirby.extension.gmaps.zoom` |
| scale | | | see `kirby.extension.gmaps.controls.scale` |
| dragable | | | see `kirby.extension.gmaps.controls.draggable` |
| zoomable | | | see `kirby.extension.gmaps.controls.zoomable` |
| maptypes | | | see `kirby.extension.gmaps.controls.maptypes` |
| maptype | | | see `kirby.extension.gmaps.controls.maptype.default` |
| maptype_selectable | | | see `kirby.extension.gmaps.controls.maptype_selectable` |
| streetview | | | see `kirby.extension.gmaps.controls.streetview` |
| fitbounds_marker | | | see `kirby.extension.gmaps.controls.fitbounds.marker` |
| fitbounds_kml | | | see `kirby.extension.gmaps.controls.fitbounds.kml` |

#### KML (kml)

| Option | Default | Values | Description |
| ------ | ------- | ------ | ----------- |
| title | | {string} | Title of KML, not useable at the moment |
| file | | {string} | relative or absolute url |

#### Marker (marker)

| Option | Default | Values | Description |
| ------ | ------- | ------ | ----------- |
| title | | {string} | |
| lat | 0.0 | {number} | |
| lng | 0.0 | {number} | |

### Examples

#### Simple example

##### Kirby

```kirby
(googlemaps lat: 35.7152128 lng: 139.7981552 zoom: 4)
```

##### PHP

```php
use at\fanninger\kirby\extension\gmaps\GMaps;

$attr = array();
$attr[GMaps::ATTR_LAT] = 35.7152128;
$attr[GMaps::ATTR_LNG] = 139.7981552;
$attr[GMaps::ATTR_ZOOM] = 4;
echo GMaps::getGMap($page, $attr);
```

#### Use Place API for map focus

##### Kirby

```kirby
(googlemaps place: Vienna)
```

##### PHP

```php
use at\fanninger\kirby\extension\gmaps\GMaps;

$attr = array();
$attr[ATTR_PLACE] = 'Vienna';
echo GMaps::getGMap($page, $attr);
```

#### KML

##### Kirby

```kirby
(googlemaps)
  (kml file: cta.kml)
(/googlemaps)
```

##### PHP

```php
use at\fanninger\kirby\extension\gmaps\GMaps;

$attr = array();
$attr[GMaps::OBJECT_KML] = array();
$attr[GMaps::OBJECT_KML][0] = array();
$attr[GMaps::OBJECT_KML][0][GMaps::ATTR_KML_FILE] = '.../cta.kml';
echo GMaps::getGMap($page, $attr);
```

#### Marker

##### Kirby

```kirby
(googlemaps)
  (marker lat: 41.875696 lng: -87.624207)
  (marker lat: -87.624207 lng: 41.875696)
(/googlemaps)
```

##### PHP

```php
use at\fanninger\kirby\extension\gmaps\GMaps;

$attr = array();
$attr[GMaps::OBJECT_MARKER] = array();
$attr[GMaps::OBJECT_MARKER][0] = array();
$attr[GMaps::OBJECT_MARKER][0][GMaps::ATTR_MARKER_LAT] = 41.875696;
$attr[GMaps::OBJECT_MARKER][0][GMaps::ATTR_MARKER_LNG] = -87.624207;
$attr[GMaps::OBJECT_MARKER][1] = array();
$attr[GMaps::OBJECT_MARKER][1][GMaps::ATTR_MARKER_LAT] = -87.624207;
$attr[GMaps::OBJECT_MARKER][1][GMaps::ATTR_MARKER_LNG] = 41.875696;
echo GMaps::getGMap($page, $attr);
```