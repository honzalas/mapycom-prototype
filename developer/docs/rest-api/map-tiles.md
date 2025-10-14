# Map Tiles

The Map Tiles API provides access to raster map tiles in various formats and mapsets. Use this API to create interactive maps with custom tile layers in your applications using libraries like Leaflet or MapLibre.

## Quick Links

- Textual documentation: https://developer.mapy.com/en/rest-api-mapy-cz/function/map-tiles/
- Swagger UI: https://api.mapy.com/v1/docs/maptiles/
- OpenAPI (YAML): https://api.mapy.com/v1/docs/maptiles/openapi.yaml

## Typical Endpoints

- `GET /v1/maptiles/{mapset}/{tileSize}/{z}/{x}/{y}` - Get map tile image
- `GET /v1/maptiles/{mapset}/tiles.json` - Get mapset description as TileJSON

## Key Parameters (Selection)

- `mapset` (string) — Map layer name: `basic`, `outdoor`, `winter`, `aerial`, `names-overlay`
- `tileSize` (string) — Tile image size: `256` (standard) or `256@2x` (retina/high-DPI, only for basic and outdoor)
- `z` (integer) — Zoom level (0-20), where higher values show more detail
- `x` (integer) — Tile X coordinate in xyz TileJSON scheme
- `y` (integer) — Tile Y coordinate in xyz TileJSON scheme
- `apikey` (string) — Your API key for authentication (query parameter or X-Mapy-Api-Key header)
- `lang` (string) — Preferred language for labels (cs, de, el, en, es, fr, it, nl, pl, pt, ru, sk, tr, uk), affects tiles with z ≤ 6

> Complete parameter list available in Swagger / YAML above.

## Examples

### cURL

```bash
# Get a basic map tile for Prague area (standard resolution)
curl "https://api.mapy.com/v1/maptiles/basic/256/14/8956/5513?apikey=YOUR_API_KEY" \
  --output tile.png

# Get a high-DPI/retina tile
curl "https://api.mapy.com/v1/maptiles/basic/256@2x/14/8956/5513?apikey=YOUR_API_KEY" \
  --output tile-retina.png

# Get TileJSON metadata for outdoor mapset
curl "https://api.mapy.com/v1/maptiles/outdoor/tiles.json?apikey=YOUR_API_KEY"
```

## Integration with Map Libraries

Map tiles are typically consumed using JavaScript mapping libraries. These libraries handle tile loading, caching, user interactions, and provide additional features like markers, popups, and custom controls. The most popular open-source libraries compatible with Mapy.com tiles are Leaflet and MapLibre GL JS. When integrating with these libraries, please remember to include proper attribution and the Mapy.com logo as shown in the examples below.

### Leaflet Example

```js
// replace with your own API key
const API_KEY = 'YOUR_API_KEY';

/*
We create the map and set its initial coordinates and zoom.
See https://leafletjs.com/reference.html#map
*/
const map = L.map('map').setView([49.8729317, 14.8981184], 16);

/*
Then we add a raster tile layer with REST API Mapy.cz tiles
See https://leafletjs.com/reference.html#tilelayer
*/
L.tileLayer(`https://api.mapy.com/v1/maptiles/basic/256/{z}/{x}/{y}?apikey=${API_KEY}`, {
  minZoom: 0,
  maxZoom: 20,
  attribution: '<a href="https://api.mapy.com/copyright" target="_blank">&copy; Seznam.cz a.s. a další</a>',
}).addTo(map);

/*
We also require you to include our logo somewhere over the map.
We create our own map control implementing a documented interface,
that shows a clickable logo.
See https://leafletjs.com/reference.html#control
*/
const LogoControl = L.Control.extend({
  options: {
    position: 'bottomleft',
  },

  onAdd: function (map) {
    const container = L.DomUtil.create('div');
    const link = L.DomUtil.create('a', '', container);

    link.setAttribute('href', 'http://mapy.com/');
    link.setAttribute('target', '_blank');
    link.innerHTML = '<img src="https://api.mapy.com/img/api/logo.svg" width="100px" />';
    L.DomEvent.disableClickPropagation(link);

    return container;
  },
});

// finally we add our LogoControl to the map
new LogoControl().addTo(map);
```

### MapLibre GL JS Example

```js
// replace with your own API key
const API_KEY = 'YOUR_API_KEY';

/*
We create a map with initial coordinates zoom, a raster tile source and a layer using that source.
See https://maplibre.org/maplibre-gl-js-docs/example/map-tiles/
See https://maplibre.org/maplibre-gl-js-docs/style-spec/sources/#raster
See https://maplibre.org/maplibre-gl-js-docs/style-spec/layers/
*/
const map = new maplibregl.Map({
  container: 'map',
  center: [14.8981184, 49.8729317],
  zoom: 15,
  style: {
    version: 8,
    sources: {
      'basic-tiles': {
        type: 'raster',
        url: `https://api.mapy.com/v1/maptiles/basic/tiles.json?apikey=${API_KEY}`,
        tileSize: 256,
      },
    },
    layers: [{
      id: 'tiles',
      type: 'raster',
      source: 'basic-tiles',
    }],
  },
});

/*
We also require you to include our logo somewhere over the map.
We create our own map control implementing a documented interface,
that shows a clickable logo.
See https://maplibre.org/maplibre-gl-js-docs/api/markers/#icontrol
*/
class LogoControl {
  onAdd(map) {
    this._map = map;
    this._container = document.createElement('div');
    this._container.className = 'maplibregl-ctrl';
    this._container.innerHTML = '<a href="http://mapy.com/" target="_blank"><img  width="100px" src="https://api.mapy.com/img/api/logo.svg" ></a>';

    return this._container;
  }
 
  onRemove() {
    this._container.parentNode.removeChild(this._container);
    this._map = undefined;
  }
}

// finally we add our LogoControl to the map
map.addControl(new LogoControl(), 'bottom-left');
```

## Common Errors and Limits

- **401 Unauthorized**: apikey parameter was not provided
- **403 Forbidden**: Invalid apikey or not permitted to access given resource
- **404 Not Found**: Invalid parameter value, parameter combination, or mapset name

**Rate Limits:**
- Tile images: Maximum 500 requests per second per API key
- TileJSON: Maximum 100 requests per second per API key

For detailed error responses and rate limits, see the [OpenAPI specification](https://api.mapy.com/v1/docs/maptiles/openapi.yaml) and [Getting Access](getting-access.md).

## Related

- [Getting Access](getting-access.md)
- [Static Maps](static-maps.md)
- [REST API Documentation](README.md)

Last verified: 2025-10-13
