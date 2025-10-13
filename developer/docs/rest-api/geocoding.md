# Geocoding

The Geocoding API provides forward geocoding (address to coordinates), reverse geocoding (coordinates to address), and autocomplete/suggestion functionality for address search.

## Quick Links

- Textual documentation: https://developer.mapy.com/en/rest-api-mapy-cz/function/geocoding/
- Swagger UI: https://api.mapy.com/v1/docs/geocode/
- OpenAPI (YAML): https://api.mapy.com/v1/docs/geocode/openapi.yaml

## Typical Endpoints

- `GET /v1/geocode` - Forward geocoding (address → coordinates)
- `GET /v1/rgeocode` - Reverse geocoding (coordinates → address)
- `GET /v1/suggest` - Autocomplete suggestions for address search

## Forward Geocoding

Convert addresses or place names to geographic coordinates.

### Key Parameters

- `apikey` (string) — Your API key for authentication
- `query` (string) — Address or place name to geocode
- `limit` (integer) — Maximum number of results to return (default: 10)
- `lang` (string) — Language for results: `cs`, `en`, `sk`, `de`, etc.
- `locality` (string) — Limit search to specific locality/region
- `type` (string) — Filter by type: `regional`, `street`, `address`, `poi`

> Complete parameter list available in Swagger / YAML above.

### Examples

#### cURL

```bash
# Find coordinates for an address
curl "https://api.mapy.com/v1/geocode?apikey=YOUR_API_KEY&query=Prague+Castle&limit=5"
```

#### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const query = 'Prague Castle';

fetch(`https://api.mapy.com/v1/geocode?apikey=${apiKey}&query=${encodeURIComponent(query)}&limit=5`)
  .then(response => response.json())
  .then(data => {
    console.log('Results:', data.items);
    if (data.items.length > 0) {
      const first = data.items[0];
      console.log(`Coordinates: ${first.position.lon}, ${first.position.lat}`);
    }
  })
  .catch(error => console.error('Error:', error));
```

#### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var query = Uri.EscapeDataString("Prague Castle");
var url = $"https://api.mapy.com/v1/geocode?apikey={apiKey}&query={query}&limit=5";

using var client = new HttpClient();
var json = await client.GetStringAsync(url);
Console.WriteLine(json);
```

## Reverse Geocoding

Convert geographic coordinates to addresses.

### Key Parameters

- `apikey` (string) — Your API key for authentication
- `lon` (number) — Longitude coordinate
- `lat` (number) — Latitude coordinate
- `type` (string) — Result type: `regional`, `street`, `address`, `poi`

### Examples

#### cURL

```bash
# Find address for coordinates
curl "https://api.mapy.com/v1/rgeocode?apikey=YOUR_API_KEY&lon=14.4378&lat=50.0755"
```

#### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const lon = 14.4378, lat = 50.0755;

fetch(`https://api.mapy.com/v1/rgeocode?apikey=${apiKey}&lon=${lon}&lat=${lat}`)
  .then(response => response.json())
  .then(data => {
    console.log('Address:', data.items[0]?.name);
  })
  .catch(error => console.error('Error:', error));
```

#### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var url = $"https://api.mapy.com/v1/rgeocode?apikey={apiKey}&lon=14.4378&lat=50.0755";

using var client = new HttpClient();
var json = await client.GetStringAsync(url);
Console.WriteLine(json);
```

## Autocomplete/Suggest

Get search suggestions as users type, useful for implementing search boxes.

### Key Parameters

- `apikey` (string) — Your API key for authentication
- `query` (string) — Partial search query
- `limit` (integer) — Maximum number of suggestions (default: 10)
- `lang` (string) — Language for results
- `locality` (string) — Limit suggestions to specific area

### Examples

#### cURL

```bash
# Get autocomplete suggestions
curl "https://api.mapy.com/v1/suggest?apikey=YOUR_API_KEY&query=Prag&limit=10"
```

#### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const query = 'Prag';

fetch(`https://api.mapy.com/v1/suggest?apikey=${apiKey}&query=${encodeURIComponent(query)}&limit=10`)
  .then(response => response.json())
  .then(data => {
    console.log('Suggestions:', data.items.map(item => item.name));
  })
  .catch(error => console.error('Error:', error));
```

#### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var query = Uri.EscapeDataString("Prag");
var url = $"https://api.mapy.com/v1/suggest?apikey={apiKey}&query={query}&limit=10";

using var client = new HttpClient();
var json = await client.GetStringAsync(url);
Console.WriteLine(json);
```

## Common Errors and Limits

- **401 Unauthorized**: Invalid or missing API key
- **400 Bad Request**: Invalid parameters (missing query, invalid coordinates)
- **403 Forbidden**: API key doesn't have access to this resource
- **429 Too Many Requests**: Rate limit exceeded
- **404 Not Found**: No results found for the query

For detailed error responses and rate limits, see the [OpenAPI specification](https://api.mapy.com/v1/docs/geocode/openapi.yaml).

## Related

- [Getting Access](getting-access.md)
- [Route Planning](route-planning.md)
- [URL Search](../url-mapy/search.md)
- [REST API Documentation](README.md)

Last verified: 2025-10-13
