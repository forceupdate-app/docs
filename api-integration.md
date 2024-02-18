# API Integration

This section is dedicated to the most generic way to integrate ForceUpdate into your app. You can use the API to check if the app needs to be updated, and based on the response, implement your own logic to handle the updates.

## Getting Started

<!-- TODO: fix the link -->

Before you can begin using the API, you need to obtain your API key from the dashboard. If you do not have an API key, follow [this guide](#api-key).

## Endpoint

| Method | URL                                       | Description                          |
| ------ | ----------------------------------------- | ------------------------------------ |
| POST   | https://api.forceupdate.app/check-version | Check if the app needs to be updated |

## Body params

| Parameter | Description                                                                        | Example |
| --------- | ---------------------------------------------------------------------------------- | ------- |
| platform  | Must be either 'ANDROID' or 'IOS'                                                  | ANDROID |
| version   | The current version of your app                                                    | 1.0.0   |
| api_key   | The API key for your project. Obtainable from your forceupdate.app dashboard       | 727f... |
| language  | The message language. Must match the one defined on your forceupdate.app dashboard | en      |

## Response

### 200: Success

When the request is successful, the response will be:

```js
{
  "needs_update": true,
  "force_update": false,
  "title": "Update Available",
  "message": "We are always working to improve your experience. Update to the latest version to get the new features and improvements.",
  "update_button_text": "Update Now",
  "dismiss_button_text": "Later",
  "store_url": "https://play.google.com/store/apps/details?id=com.example.myapp"
}
```

### 400: Bad Request

If the API key is not related to any project or invalid, the response will be:

```js
{
  "message": ["This API key is not related to any project."],
  "error": "Bad Request",
  "statusCode": 400
}
```

## Collections

### Swagger

For the most recent API documentation, refer to our Swagger: [https://api.forceupdate.app/docs](https://api.forceupdate.app/docs)

### Postman

<!-- TODO: Add postman -->

Coming soon

## Examples

### Example: CURL

```js
curl -X 'POST' \
  'https://api.forceupdate.app/check-version' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "version": "1.0.0",
  "platform": "ANDROID",
  "language": "en",
  "api_key": "727fb40897b3c11dce6be9f72b7e3cbaea73fe030565392084fbd7f449d09fd3"
}'
```

### Example: javascript fetch

```js
const myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");
myHeaders.append(
  "Authorization",
  "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c3VfaWQiOiI4MjY3YmY5Mi0wYmE2LTQ2M2ItOTZiYi03Y2VjMmMwMmVjMDkiLCJ1c3VfbmFtZSI6Ikpvw6NvIE1hbnRvdmFuaTIiLCJpYXQiOjE3MDc5MjIxNzAsImV4cCI6MTcwODAwODU3MH0.D_StaRR1doH2l9qgKnOcMffgs7ieCgw4yxLiZ2rvnCI"
);

const raw = JSON.stringify({
  version: "1.0.3",
  platform: "ANDROID",
  api_key: "6bbc51d23e5938d512a62a83c230dfdef89ab7a2c75bdc1f0f42909d5e04feb5",
  language: "en",
});

const requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow",
};

fetch("https://api.forceupdate.app/check-version", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```

### Example: javascript axios

```js
import axios from "axios";

const headers = {
  "Content-Type": "application/json",
  Authorization:
    "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c3VfaWQiOiI4MjY3YmY5Mi0wYmE2LTQ2M2ItOTZiYi03Y2VjMmMwMmVjMDkiLCJ1c3VfbmFtZSI6Ikpvw6NvIE1hbnRvdmFuaTIiLCJpYXQiOjE3MDc5MjIxNzAsImV4cCI6MTcwODAwODU3MH0.D_StaRR1doH2l9qgKnOcMffgs7ieCgw4yxLiZ2rvnCI",
};

const data = JSON.stringify({
  version: "1.0.3",
  platform: "ANDROID",
  api_key: "6bbc51d23e5938d512a62a83c230dfdef89ab7a2c75bdc1f0f42909d5e04feb5",
  language: "en",
});

axios
  .post("https://api.forceupdate.app/check-version", data, { headers })
  .then((response) => console.log(response.data))
  .catch((error) => console.error(error));
```

### Example: Dart - DIO

```dart
var headers = {
  'Content-Type': 'application/json',
  'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c3VfaWQiOiI4MjY3YmY5Mi0wYmE2LTQ2M2ItOTZiYi03Y2VjMmMwMmVjMDkiLCJ1c3VfbmFtZSI6Ikpvw6NvIE1hbnRvdmFuaTIiLCJpYXQiOjE3MDc5MjIxNzAsImV4cCI6MTcwODAwODU3MH0.D_StaRR1doH2l9qgKnOcMffgs7ieCgw4yxLiZ2rvnCI'
};
var data = json.encode({
  "version": "1.0.3",
  "platform": "ANDROID",
  "api_key": "6bbc51d23e5938d512a62a83c230dfdef89ab7a2c75bdc1f0f42909d5e04feb5",
  "language": "en"
});
var dio = Dio();
var response = await dio.request(
  'https://api.forceupdate.app/check-version',
  options: Options(
    method: 'POST',
    headers: headers,
  ),
  data: data,
);

if (response.statusCode == 200) {
  print(json.encode(response.data));
}
else {
  print(response.statusMessage);
}
```

### Example: Dart - HTTP

```dart
var headers = {
  'Content-Type': 'application/json',
  'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c3VfaWQiOiI4MjY3YmY5Mi0wYmE2LTQ2M2ItOTZiYi03Y2VjMmMwMmVjMDkiLCJ1c3VfbmFtZSI6Ikpvw6NvIE1hbnRvdmFuaTIiLCJpYXQiOjE3MDc5MjIxNzAsImV4cCI6MTcwODAwODU3MH0.D_StaRR1doH2l9qgKnOcMffgs7ieCgw4yxLiZ2rvnCI'
};
var request = http.Request('POST', Uri.parse('https://api.forceupdate.app/check-version'));
request.body = json.encode({
  "version": "1.0.3",
  "platform": "ANDROID",
  "api_key": "6bbc51d23e5938d512a62a83c230dfdef89ab7a2c75bdc1f0f42909d5e04feb5",
  "language": "en"
});
request.headers.addAll(headers);

http.StreamedResponse response = await request.send();

if (response.statusCode == 200) {
  print(await response.stream.bytesToString());
}
else {
  print(response.reasonPhrase);
}
```

### Example: Android

Check our [Android example](android-integration) for a complete example.

### Example: Swift

Check our [Swift example](ios-integration) for a complete example.

### Example React-native (with or without Expo)

Check our [React Native example](react-native-integration) for a complete example.
