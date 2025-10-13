# Static Panorama

The Static Panorama API retrieves static images from spherical photos (360° panoramas) at specific locations. Use this to display street-level imagery in your applications without implementing a full panorama viewer.

## Quick Links

- Textual documentation: https://developer.mapy.com/en/rest-api-mapy-cz/function/api-for-static-panorama/
- Swagger UI: https://api.mapy.com/v1/docs/static/
- OpenAPI (YAML): https://api.mapy.com/v1/docs/static/openapi.yaml

## Typical Endpoints

- `GET /v1/static/panorama` - Get a static panorama image for specific coordinates and viewing direction

## Key Parameters (Selection)

- `apikey` (string) — Your API key for authentication
- `lon` (number) — Longitude coordinate where panorama is located
- `lat` (number) — Latitude coordinate where panorama is located
- `heading` (number) — Viewing direction in degrees (0-360, 0=North, 90=East, 180=South, 270=West)
- `pitch` (number) — Vertical viewing angle in degrees (-90 to +90, 0=horizontal)
- `fov` (number) — Field of view in degrees (10-120, default: 90)
- `width` (integer) — Image width in pixels (default: 640, max: 2048)
- `height` (integer) — Image height in pixels (default: 480, max: 2048)
- `format` (string) — Output format: `png` (default), `jpeg`

> Complete parameter list available in Swagger / YAML above.

## Examples

### cURL

```bash
# Get a panorama view from Prague Old Town Square
curl "https://api.mapy.com/v1/static/panorama?apikey=YOUR_API_KEY&lon=14.4208&lat=50.0875&heading=180&pitch=0&fov=90&width=800&height=600" \
  --output panorama.jpg
```

### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;

const params = new URLSearchParams({
  apikey: apiKey,
  lon: 14.4208,
  lat: 50.0875,
  heading: 180,      // Looking south
  pitch: 0,          // Horizontal view
  fov: 90,           // 90-degree field of view
  width: 800,
  height: 600,
  format: 'jpeg'
});

fetch(`https://api.mapy.com/v1/static/panorama?${params}`)
  .then(response => response.blob())
  .then(blob => {
    const imageUrl = URL.createObjectURL(blob);
    document.getElementById('panorama').src = imageUrl;
  })
  .catch(error => console.error('Error:', error));
```

### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var url = $"https://api.mapy.com/v1/static/panorama?apikey={apiKey}" +
          "&lon=14.4208&lat=50.0875&heading=180&pitch=0&fov=90" +
          "&width=800&height=600&format=jpeg";

using var client = new HttpClient();
var imageBytes = await client.GetByteArrayAsync(url);
await File.WriteAllBytesAsync("panorama.jpg", imageBytes);
```

## Use Cases

- **Virtual Tours**: Show street-level views without full panorama viewer
- **Location Preview**: Give users a sense of place before visiting
- **Reports**: Include street-level imagery in PDFs or documents
- **Email Marketing**: Embed location previews in emails
- **Real Estate**: Show property surroundings

## Understanding Parameters

### Heading (Direction)
- 0° = North
- 90° = East
- 180° = South
- 270° = West

### Pitch (Vertical Angle)
- -90° = Looking straight down
- 0° = Looking at horizon
- +90° = Looking straight up

### Field of View (FOV)
- Smaller values (10-45°) = telephoto/zoomed in view
- 90° = Normal human field of view
- Larger values (90-120°) = wide angle view

## Common Errors and Limits

- **401 Unauthorized**: Invalid or missing API key
- **400 Bad Request**: Invalid parameters (coordinates, angles, dimensions)
- **403 Forbidden**: API key doesn't have access to this resource
- **404 Not Found**: No panorama available at specified location
- **429 Too Many Requests**: Rate limit exceeded

**Limitations:**
- Panoramas are only available for locations where they have been captured
- Maximum image dimensions: 2048x2048 pixels
- Coverage is primarily in Czech Republic and selected other areas

For detailed error responses, see the [OpenAPI specification](https://api.mapy.com/v1/docs/static/openapi.yaml).

## Related

- [Getting Access](getting-access.md)
- [Static Maps](static-maps.md)
- [REST API Documentation](README.md)

Last verified: 2025-10-13
