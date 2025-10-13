# Time Zones

The Time Zones API provides time zone information, local time, and UTC offsets for specific geographic coordinates. Use this to display local times for locations worldwide or convert between time zones.

## Quick Links

- Textual documentation: https://developer.mapy.com/en/rest-api-mapy-cz/function/api-for-time-zones/
- Swagger UI: https://api.mapy.com/v1/docs/timezone/
- OpenAPI (YAML): https://api.mapy.com/v1/docs/timezone/openapi.yaml

## Typical Endpoints

- `GET /v1/timezone` - Get time zone information for specific coordinates

## Key Parameters (Selection)

- `apikey` (string) — Your API key for authentication
- `lon` (number) — Longitude coordinate
- `lat` (number) — Latitude coordinate
- `timestamp` (integer) — Optional Unix timestamp (seconds since epoch) to query historical timezone data

> Complete parameter list available in Swagger / YAML above.

## Examples

### Current Time Zone

#### cURL

```bash
# Get time zone information for Prague
curl "https://api.mapy.com/v1/timezone?apikey=YOUR_API_KEY&lon=14.4378&lat=50.0755"
```

#### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const lon = 14.4378, lat = 50.0755;

fetch(`https://api.mapy.com/v1/timezone?apikey=${apiKey}&lon=${lon}&lat=${lat}`)
  .then(response => response.json())
  .then(data => {
    console.log('Time Zone:', data.timeZoneId);
    console.log('UTC Offset:', data.rawOffset, 'seconds');
    console.log('DST Offset:', data.dstOffset, 'seconds');
    console.log('Time Zone Name:', data.timeZoneName);
  })
  .catch(error => console.error('Error:', error));
```

#### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var url = $"https://api.mapy.com/v1/timezone?apikey={apiKey}&lon=14.4378&lat=50.0755";

using var client = new HttpClient();
var json = await client.GetStringAsync(url);
Console.WriteLine(json);
```

### Historical Time Zone Query

Query time zone information for a specific date/time:

#### cURL

```bash
# Get time zone for Prague on January 1, 2020
curl "https://api.mapy.com/v1/timezone?apikey=YOUR_API_KEY&lon=14.4378&lat=50.0755&timestamp=1577836800"
```

#### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const lon = 14.4378, lat = 50.0755;
const timestamp = Math.floor(new Date('2020-01-01').getTime() / 1000);

fetch(`https://api.mapy.com/v1/timezone?apikey=${apiKey}&lon=${lon}&lat=${lat}&timestamp=${timestamp}`)
  .then(response => response.json())
  .then(data => {
    console.log('Time Zone at that time:', data.timeZoneId);
    console.log('Total offset:', data.rawOffset + data.dstOffset, 'seconds');
  })
  .catch(error => console.error('Error:', error));
```

#### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var timestamp = new DateTimeOffset(new DateTime(2020, 1, 1)).ToUnixTimeSeconds();
var url = $"https://api.mapy.com/v1/timezone?apikey={apiKey}&lon=14.4378&lat=50.0755&timestamp={timestamp}";

using var client = new HttpClient();
var json = await client.GetStringAsync(url);
Console.WriteLine(json);
```

## Response Fields

The API typically returns:
- `timeZoneId` - IANA time zone identifier (e.g., "Europe/Prague")
- `timeZoneName` - Human-readable time zone name
- `rawOffset` - Base UTC offset in seconds (without DST)
- `dstOffset` - Daylight saving time offset in seconds (0 if not in DST)
- Total offset = rawOffset + dstOffset

## Use Cases

- **Event Scheduling**: Display event times in local time zones
- **Travel Applications**: Show local time at destinations
- **Logistics**: Calculate delivery times across time zones
- **Meeting Planners**: Find suitable times across multiple time zones
- **Historical Data**: Analyze time-sensitive data with correct time zones

## Converting Offsets to Local Time

```js
// Example: Convert UTC time to local time using API response
const utcTime = new Date();
const totalOffsetSeconds = data.rawOffset + data.dstOffset;
const localTime = new Date(utcTime.getTime() + totalOffsetSeconds * 1000);
console.log('Local time:', localTime.toISOString());
```

## Common Errors and Limits

- **401 Unauthorized**: Invalid or missing API key
- **400 Bad Request**: Invalid coordinates or timestamp
- **403 Forbidden**: API key doesn't have access to this resource
- **429 Too Many Requests**: Rate limit exceeded

For detailed error responses and rate limits, see the [OpenAPI specification](https://api.mapy.com/v1/docs/timezone/openapi.yaml).

## Related

- [Getting Access](getting-access.md)
- [Geocoding](geocoding.md)
- [REST API Documentation](README.md)

Last verified: 2025-10-13
