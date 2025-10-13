# Map Tiles

The Map Tiles API provides access to raster map tiles in various formats and mapsets. Use this API to create interactive maps with custom tile layers in your applications using libraries like Leaflet or MapLibre.

## Quick Links

- Textual documentation: https://developer.mapy.com/en/rest-api-mapy-cz/function/map-tiles/
- Swagger UI: https://api.mapy.com/v1/docs/maptiles/
- OpenAPI (YAML): https://api.mapy.com/v1/docs/maptiles/openapi.yaml

## Typical Endpoints

- `GET /v1/maptiles/basic/{z}/{x}/{y}` - Basic map tiles
- `GET /v1/maptiles/outdoor/{z}/{x}/{y}` - Outdoor/hiking map tiles
- `GET /v1/maptiles/winter/{z}/{x}/{y}` - Winter sports map tiles
- `GET /v1/maptiles/aerial/{z}/{x}/{y}` - Aerial/satellite imagery

## Key Parameters (Selection)

- `z` (integer) — Zoom level (0-19), where higher values show more detail
- `x` (integer) — Tile X coordinate in the tile grid
- `y` (integer) — Tile Y coordinate in the tile grid
- `apikey` (string) — Your API key for authentication
- `format` (string) — Output format: `png` (default) or `jpeg`
- `scale` (integer) — Resolution scale: 1 (standard), 2 (retina/high-DPI)

> Complete parameter list available in Swagger / YAML above.

## Examples

### cURL

```bash
# Get a basic map tile for Prague area
curl "https://api.mapy.com/v1/maptiles/basic/14/8956/5513?apikey=YOUR_API_KEY" \
  --output tile.png
```

### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const z = 14, x = 8956, y = 5513;
const mapset = 'basic';

fetch(`https://api.mapy.com/v1/maptiles/${mapset}/${z}/${x}/${y}?apikey=${apiKey}`)
  .then(response => response.blob())
  .then(blob => {
    const imageUrl = URL.createObjectURL(blob);
    document.getElementById('map-tile').src = imageUrl;
  })
  .catch(error => console.error('Error:', error));
```

### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var url = $"https://api.mapy.com/v1/maptiles/basic/14/8956/5513?apikey={apiKey}";

using var client = new HttpClient();
var imageBytes = await client.GetByteArrayAsync(url);
await File.WriteAllBytesAsync("tile.png", imageBytes);
```

## Integration with Map Libraries

### Leaflet Example

```js
const mapyTilesLayer = L.tileLayer(
  'https://api.mapy.com/v1/maptiles/basic/{z}/{x}/{y}?apikey=YOUR_API_KEY',
  {
    attribution: '&copy; <a href="https://mapy.cz/">Mapy.cz</a>',
    maxZoom: 19
  }
);

const map = L.map('map').setView([50.0755, 14.4378], 13);
mapyTilesLayer.addTo(map);
```

## Common Errors and Limits

- **401 Unauthorized**: Invalid or missing API key
- **403 Forbidden**: API key doesn't have access to this resource
- **404 Not Found**: Invalid tile coordinates or zoom level
- **429 Too Many Requests**: Rate limit exceeded

For detailed error responses and rate limits, see the [OpenAPI specification](https://api.mapy.com/v1/docs/maptiles/openapi.yaml) and [Getting Access](getting-access.md).

## Related

- [Getting Access](getting-access.md)
- [Static Maps](static-maps.md)
- [REST API Documentation](README.md)

Last verified: 2025-10-13
