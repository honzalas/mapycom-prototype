# Route Planning

The Route Planning API provides route calculation between multiple points with support for different travel modes, optimization options, and matrix routing for calculating distances/times between multiple origins and destinations.

## Quick Links

- Textual documentation: https://developer.mapy.com/en/rest-api-mapy-cz/function/routing/
- Swagger UI: https://api.mapy.com/v1/docs/routing/
- OpenAPI (YAML): https://api.mapy.com/v1/docs/routing/openapi.yaml

## Typical Endpoints

- `POST /v1/routing/route` - Calculate a route between waypoints
- `POST /v1/routing/matrix` - Calculate distance/time matrix between multiple points

## Key Parameters (Selection)

- `apikey` (string) — Your API key for authentication
- `start` (object) — Starting point coordinates: `{lon: number, lat: number}`
- `end` (object) — Destination coordinates: `{lon: number, lat: number}`
- `waypoints` (array) — Optional intermediate points (max 15)
- `routeType` (string) — Route optimization:
  - `car_fast` - Fastest route by car
  - `car_short` - Shortest route by car
  - `car_fast_traffic` - Fastest with real-time traffic
  - `foot_fast` - Walking route
  - `foot_hiking` - Hiking trail
  - `bike_road` - Road cycling
  - `bike_mountain` - Mountain biking
- `lang` (string) — Language for instructions: `cs`, `en`, `sk`, `de`, etc.
- `format` (string) — Response format: `json`, `geojson`
- `avoidToll` (boolean) — Avoid toll roads (for car routes)
- `avoidHighway` (boolean) — Avoid highways (for car routes)

> Complete parameter list available in Swagger / YAML above.

## Examples

### cURL

```bash
# Calculate a car route from Prague to Brno
curl -X POST "https://api.mapy.com/v1/routing/route?apikey=YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "start": {"lon": 14.4378, "lat": 50.0755},
    "end": {"lon": 16.6068, "lat": 49.1951},
    "routeType": "car_fast"
  }'
```

### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;

const routeRequest = {
  start: { lon: 14.4378, lat: 50.0755 },
  end: { lon: 16.6068, lat: 49.1951 },
  routeType: 'car_fast',
  lang: 'en'
};

fetch(`https://api.mapy.com/v1/routing/route?apikey=${apiKey}`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(routeRequest)
})
  .then(response => response.json())
  .then(data => {
    console.log(`Distance: ${data.distance} m`);
    console.log(`Duration: ${data.duration} s`);
    console.log('Geometry:', data.geometry);
  })
  .catch(error => console.error('Error:', error));
```

### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Text.Json;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var url = $"https://api.mapy.com/v1/routing/route?apikey={apiKey}";

var routeRequest = new {
    start = new { lon = 14.4378, lat = 50.0755 },
    end = new { lon = 16.6068, lat = 49.1951 },
    routeType = "car_fast"
};

var json = JsonSerializer.Serialize(routeRequest);
var content = new StringContent(json, Encoding.UTF8, "application/json");

using var client = new HttpClient();
var response = await client.PostAsync(url, content);
var result = await response.Content.ReadAsStringAsync();
Console.WriteLine(result);
```

## Route with Waypoints

```js
const routeRequest = {
  start: { lon: 14.4378, lat: 50.0755 },
  waypoints: [
    { lon: 15.0, lat: 50.0 },
    { lon: 15.5, lat: 49.8 }
  ],
  end: { lon: 16.6068, lat: 49.1951 },
  routeType: 'car_fast'
};
```

## Matrix Routing (Overview)

The matrix routing endpoint calculates distances and travel times between multiple origins and destinations simultaneously. This is useful for:
- Fleet optimization
- Service area analysis
- Multiple delivery planning

### Matrix Example

```bash
curl -X POST "https://api.mapy.com/v1/routing/matrix?apikey=YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "origins": [
      {"lon": 14.4378, "lat": 50.0755},
      {"lon": 14.5, "lat": 50.1}
    ],
    "destinations": [
      {"lon": 16.6068, "lat": 49.1951},
      {"lon": 16.7, "lat": 49.2}
    ],
    "routeType": "car_fast"
  }'
```

The response contains a matrix of distances and durations between each origin-destination pair.

## Common Errors and Limits

- **401 Unauthorized**: Invalid or missing API key
- **400 Bad Request**: Invalid coordinates or parameters
- **403 Forbidden**: API key doesn't have access to this resource
- **422 Unprocessable Entity**: Route cannot be calculated (no road connection, invalid route type)
- **429 Too Many Requests**: Rate limit exceeded

**Limitations:**
- Maximum 15 waypoints per route
- Matrix routing limited to reasonable number of origins/destinations (check API documentation)

For detailed error responses and limits, see the [OpenAPI specification](https://api.mapy.com/v1/docs/routing/openapi.yaml).

## Related

- [Getting Access](getting-access.md)
- [Geocoding](geocoding.md)
- [URL Route](../url-mapy/route.md)
- [REST API Documentation](README.md)

Last verified: 2025-10-13
