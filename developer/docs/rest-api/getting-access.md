# Getting Access to Mapy.com REST API

To use the Mapy.com REST API, you need to register for an account and obtain an API key.

## Registration and API Key

1. **Create an Account**: Visit the [Mapy.com Developer Portal](https://developer.mapy.com/) and log in with your Seznam account
2. **Create an API Project**: In the My Account portal, create a new API Project for your application
3. **Get Your API Key**: An API key will be automatically generated within your project
4. **Use the API Key**: Include the API key in your requests using the `apikey` parameter

## Authentication

The API uses simple API key authentication. Include your key in every request:

```
https://api.mapy.com/v1/[endpoint]?apikey=YOUR_API_KEY&[other_parameters]
```

## Security Best Practices

- **Never commit API keys to version control**: Use environment variables or secure configuration
- **Use server-side proxies for sensitive calls**: Don't expose your API key in client-side code for production applications
- **Monitor your usage**: Track credit consumption in the My Account portal
- **Rotate keys if compromised**: Generate new keys immediately if you suspect unauthorized access

## Hello API - First Request

Here's a simple example calling the geocoding API to find coordinates for an address:

### cURL

```bash
curl "https://api.mapy.com/v1/geocode?apikey=YOUR_API_KEY&query=Prague"
```

### JavaScript (fetch)

```js
const apiKey = process.env.MAPY_API_KEY;
const query = 'Prague';

fetch(`https://api.mapy.com/v1/geocode?apikey=${apiKey}&query=${encodeURIComponent(query)}`)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### C# (HttpClient)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

var apiKey = Environment.GetEnvironmentVariable("MAPY_API_KEY");
var query = "Prague";
var url = $"https://api.mapy.com/v1/geocode?apikey={apiKey}&query={Uri.EscapeDataString(query)}";

using var client = new HttpClient();
var response = await client.GetStringAsync(url);
Console.WriteLine(response);
```

## Rate Limits and Pricing

The Mapy.com REST API uses a credit-based system:

- **Free tier**: Available for testing and small-scale projects
- **Paid plans**: Available for production applications with higher usage
- **Credit consumption**: Different endpoints consume different amounts of credits
- **Monitoring**: Track your usage in the My Account portal

For current pricing and detailed information about rate limits, visit:
- [Mapy.com REST API Pricing](https://developer.mapy.com/en/rest-api-mapy-cz/)
- [My Account Portal](https://developer.mapy.com/)

## Testing

Use the interactive Swagger UI to test API calls and explore responses:
- [API Documentation](https://api.mapy.com/v1/docs/)

You can try different parameters and see the actual responses before implementing in your application.

## Related

- [REST API Documentation](README.md)
- [Map Tiles](map-tiles.md)
- [Forward Geocoding](forward-geocoding.md)
- [Reverse Geocoding](reverse-geocoding.md)
- [Routing](routing.md)
- [Matrix Routing](matrix-routing.md)

Last verified: 2025-10-13
