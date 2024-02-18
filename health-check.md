# Health Check

Our application provides two health check endpoints:

## Liveness Check

This endpoint checks if the application is running.

**Endpoint:** `/health-check/liveness`

**Method:** `GET`

**Curl Command:**

```bash
curl -X 'GET' 'http://localhost:8080/health-check/liveness' -H 'accept: */*'
```

### Liveness Check Response:

```json
{
  "status": "fully functional",
  "version": "0.0.1"
}
```

## Readiness Check

This endpoint checks if the application and its integrations are ready to handle requests.

**Endpoint:** /health-check/readiness

**Method:** GET

**Curl Command:**

```bash
curl -X 'GET' 'http://localhost:8080/health-check/readiness' -H 'accept: */*'
```

### Readiness Check Response

```json
{
  "name": "forceupdate-api",
  "version": "1.0.0",
  "status": true,
  "date": "2024-02-18T18:41:25.157Z",
  "duration": 0.008,
  "integrations": [
    {
      "name": "database",
      "status": true,
      "response_time": 0.005
    }
  ]
}
```
