# Elevation

The Elevation API provides elevation data for specific geographic coordinates. Use this to get altitude information for points or along paths, useful for hiking apps, route profiling, and terrain analysis.

## Quick Links

- Textual documentation: https://developer.mapy.com/en/rest-api-mapy-cz/function/elevation-api/
- Swagger UI: https://api.mapy.com/v1/docs/elevation/
- OpenAPI (YAML): https://api.mapy.com/v1/docs/elevation/openapi.yaml

## Typical Endpoints

- `GET /v1/elevation` - Get elevation for one or more points

## Key Parameters (Selection)

- `apikey` (string) — Your API key for authentication
- `lon` (number) — Longitude coordinate (single point query)
- `lat` (number) — Latitude coordinate (single point query)
- `points` (string) — Multiple points in format: `lon1,lat1;lon2,lat2;...`
- `samples` (integer) — Number of samples to interpolate along path (for multiple points)

> Complete parameter list available in Swagger / YAML above.

## Examples

### Single Point Elevation

#### cURL

```bash
# Get elevation for a single point (Prague Castle area)
curl "https://api.mapy.com/v1/elevation?apikey=YOUR_API_KEY&lon=14.4378&lat=50.0755"
```

#### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const lon = 14.4378, lat = 50.0755;

fetch(`https://api.mapy.com/v1/elevation?apikey=${apiKey}&lon=${lon}&lat=${lat}`)
  .then(response => response.json())
  .then(data => {
    console.log(`Elevation: ${data.elevation} meters`);
  })
  .catch(error => console.error('Error:', error));
```

#### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var url = $"https://api.mapy.com/v1/elevation?apikey={apiKey}&lon=14.4378&lat=50.0755";

using var client = new HttpClient();
var json = await client.GetStringAsync(url);
Console.WriteLine(json);
```

### Multiple Points (Path Profile)

Get elevation profile along a path by specifying multiple points:

#### cURL

```bash
# Get elevation profile for a path
curl "https://api.mapy.com/v1/elevation?apikey=YOUR_API_KEY&points=14.4,50.07;14.45,50.08;14.5,50.09"
```

#### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const points = '14.4,50.07;14.45,50.08;14.5,50.09';

fetch(`https://api.mapy.com/v1/elevation?apikey=${apiKey}&points=${points}`)
  .then(response => response.json())
  .then(data => {
    console.log('Elevation profile:');
    data.points.forEach((point, i) => {
      console.log(`Point ${i}: ${point.elevation} m`);
    });
  })
  .catch(error => console.error('Error:', error));
```

#### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var points = "14.4,50.07;14.45,50.08;14.5,50.09";
var url = $"https://api.mapy.com/v1/elevation?apikey={apiKey}&points={points}";

using var client = new HttpClient();
var json = await client.GetStringAsync(url);
Console.WriteLine(json);
```

## Use Cases

- **Hiking Applications**: Display elevation profiles for hiking trails
- **Route Planning**: Calculate elevation gain/loss for routes
- **Fitness Apps**: Track altitude changes during activities
- **Terrain Analysis**: Analyze topography for construction or planning
- **Drone Flight Planning**: Ensure safe flight paths considering terrain

## Response Format

The API returns elevation in meters above sea level. For multiple points, the response includes:
- Individual elevation for each point
- Optional interpolated samples between points
- Distance information along the path

## Common Errors and Limits

- **401 Unauthorized**: Invalid or missing API key
- **400 Bad Request**: Invalid coordinates or parameters
- **403 Forbidden**: API key doesn't have access to this resource
- **429 Too Many Requests**: Rate limit exceeded
- **404 Not Found**: Elevation data not available for specified location

**Limitations:**
- Points must be within service coverage area (primarily Czech Republic and surrounding regions)
- Maximum number of points per request depends on your API plan

For detailed error responses and limits, see the [OpenAPI specification](https://api.mapy.com/v1/docs/elevation/openapi.yaml).

## Related

- [Getting Access](getting-access.md)
- [Route Planning](route-planning.md)
- [REST API Documentation](README.md)

Last verified: 2025-10-13
