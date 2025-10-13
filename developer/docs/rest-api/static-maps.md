# Static Maps

The Static Maps API generates static map images with customizable markers, paths, and layers. Use this to embed maps in emails, PDFs, reports, or anywhere a static image is needed instead of an interactive map.

## Quick Links

- Textual documentation: https://developer.mapy.com/en/rest-api-mapy-cz/function/static-maps/
- Swagger UI: https://api.mapy.com/v1/docs/static/
- OpenAPI (YAML): https://api.mapy.com/v1/docs/static/openapi.yaml

## Typical Endpoints

- `GET /v1/static/map` - Generate a static map image with optional markers and overlays

## Key Parameters (Selection)

- `apikey` (string) — Your API key for authentication
- `center` (string) — Center coordinates in format `lon,lat` (e.g., `14.4378,50.0755`)
- `zoom` (integer) — Zoom level (1-19)
- `width` (integer) — Image width in pixels (default: 600, max: 2048)
- `height` (integer) — Image height in pixels (default: 400, max: 2048)
- `mapset` (string) — Map style: `basic`, `outdoor`, `winter`, `aerial`, `traffic`
- `markers` (string) — Marker definitions: `lon,lat|color:red|size:small`
- `path` (string) — Path/polyline coordinates: `lon1,lat1;lon2,lat2;...`
- `format` (string) — Output format: `png` (default), `jpeg`

> Complete parameter list available in Swagger / YAML above.

## Examples

### cURL

```bash
# Generate a static map with a marker in Prague
curl "https://api.mapy.com/v1/static/map?apikey=YOUR_API_KEY&center=14.4378,50.0755&zoom=13&width=800&height=600&markers=14.4378,50.0755" \
  --output map.png
```

### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const params = new URLSearchParams({
  apikey: apiKey,
  center: '14.4378,50.0755',
  zoom: 13,
  width: 800,
  height: 600,
  mapset: 'basic',
  markers: '14.4378,50.0755|color:red'
});

fetch(`https://api.mapy.com/v1/static/map?${params}`)
  .then(response => response.blob())
  .then(blob => {
    const imageUrl = URL.createObjectURL(blob);
    document.getElementById('static-map').src = imageUrl;
  })
  .catch(error => console.error('Error:', error));
```

### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var url = $"https://api.mapy.com/v1/static/map?apikey={apiKey}" +
          "&center=14.4378,50.0755&zoom=13&width=800&height=600" +
          "&markers=14.4378,50.0755|color:red";

using var client = new HttpClient();
var imageBytes = await client.GetByteArrayAsync(url);
await File.WriteAllBytesAsync("map.png", imageBytes);
```

## Advanced Features

### Multiple Markers with Custom Colors

```bash
curl "https://api.mapy.com/v1/static/map?apikey=YOUR_API_KEY&center=14.4378,50.0755&zoom=12&width=800&height=600&markers=14.4378,50.0755|color:red&markers=14.5,50.08|color:blue"
```

### Path/Route Overlay

```bash
curl "https://api.mapy.com/v1/static/map?apikey=YOUR_API_KEY&center=14.4378,50.0755&zoom=12&width=800&height=600&path=14.4,50.07;14.45,50.08;14.5,50.075|color:blue|width:3"
```

## Common Errors and Limits

- **401 Unauthorized**: Invalid or missing API key
- **400 Bad Request**: Invalid parameter values (coordinates, dimensions)
- **403 Forbidden**: API key doesn't have access to this resource
- **429 Too Many Requests**: Rate limit exceeded

Maximum image dimensions: 2048x2048 pixels. For detailed error responses, see the [OpenAPI specification](https://api.mapy.com/v1/docs/static/openapi.yaml).

## Related

- [Getting Access](getting-access.md)
- [Map Tiles](map-tiles.md)
- [Static Panorama](static-panorama.md)
- [REST API Documentation](README.md)

Last verified: 2025-10-13
