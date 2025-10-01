# FreeFire Banned Check API

A Flask-based API service that checks if Free Fire player UIDs are banned by bypassing Garena's whitelist restrictions. This service provides a simple and reliable way to verify ban status for Free Fire accounts.

## Features

- ✅ **Bypasses Garena Whitelist**: Uses multiple approaches to circumvent IP restrictions
- ✅ **User-Friendly Responses**: Returns clear, simple ban status messages
- ✅ **Retry Strategy**: Implements robust retry logic for failed requests
- ✅ **Fallback Support**: Optional mock responses when API access is restricted
- ✅ **Vercel Ready**: Configured for easy deployment on Vercel
- ✅ **Error Handling**: Comprehensive error handling with detailed responses

## API Endpoints

### Check Ban Status

**GET** `/check_banned`

Check if a Free Fire UID is banned.

#### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `uid` | string | ✅ | - | Free Fire player UID to check |
| `lang` | string | ❌ | `en` | Language code for the response |
| `base` | string | ❌ | Garena API URL | Override the base API URL |
| `fallback` | string | ❌ | `false` | Return mock response when API is restricted |

#### Example Requests

```bash
# Basic check
GET /check_banned?uid=2932217690

# With language
GET /check_banned?uid=2230657357&lang=en

# With fallback enabled
GET /check_banned?uid=1234567890&fallback=true
```

#### Response Examples

**Banned Account:**
```json
{
  "banned": true,
  "message": "banned",
  "uid": "2230657357"
}
```

**Clean Account:**
```json
{
  "banned": false,
  "message": "NOT banned",
  "uid": "2932217690"
}
```

**Invalid Account:**
```json
{
  "banned": null,
  "message": "invalid account",
  "uid": "1234567890"
}
```

**API Restricted (with fallback):**
```json
{
  "uid": "1234567890",
  "status": "unknown",
  "message": "API access restricted - using fallback response",
  "banned": false,
  "reason": "Unable to verify due to API restrictions",
  "note": "This is a fallback response. The actual ban status could not be determined."
}
```

### Home Endpoint

**GET** `/`

Returns API documentation and available endpoints.

## Installation & Setup

### Local Development

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd FreeFire-Banned-Check
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the application:**
   ```bash
   python app.py
   ```

4. **Access the API:**
   - API will be available at `http://localhost:5000`
   - Home endpoint: `http://localhost:5000/`
   - Check endpoint: `http://localhost:5000/check_banned?uid=YOUR_UID`

### Vercel Deployment

This project is pre-configured for Vercel deployment:

1. **Install Vercel CLI:**
   ```bash
   npm i -g vercel
   ```

2. **Deploy:**
   ```bash
   vercel
   ```

3. **Environment Variables:**
   No additional environment variables are required for basic functionality.

## How It Works

The API uses multiple strategies to bypass Garena's whitelist restrictions:

1. **Cookie Management**: Sets required cookies for API access
2. **Multiple Approaches**: Tries different request patterns:
   - Visit support page first, then API
   - Direct request with cookies
   - Visit main page first, then API
   - Request with different referer headers
3. **Retry Strategy**: Implements exponential backoff for failed requests
4. **Session Management**: Uses persistent sessions for better success rates

## Dependencies

- **Flask 2.3.3**: Web framework
- **requests 2.31.0**: HTTP library with retry support
- **urllib3 2.0.7**: HTTP client library

## Error Handling

The API handles various error scenarios:

- **Missing UID**: Returns 400 error with clear message
- **Invalid UID**: Returns appropriate error response
- **API Restrictions**: Provides fallback option or detailed error
- **Network Issues**: Implements retry logic with exponential backoff
- **Server Errors**: Returns appropriate HTTP status codes

## Rate Limiting & Best Practices

- The API implements retry logic to handle temporary failures
- Use reasonable request intervals to avoid overwhelming the target API
- Consider implementing client-side rate limiting for production use
- Monitor API responses for any changes in Garena's restrictions

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is for educational purposes. Please respect Garena's terms of service and use responsibly.

## Disclaimer

This tool is provided as-is for educational and research purposes. Users are responsible for complying with Garena's terms of service and applicable laws. The authors are not responsible for any misuse of this tool.
