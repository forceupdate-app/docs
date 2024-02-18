# Flutter integration

This guide will help you to integrate ForceUpdate into your Flutter app.

## Getting Started

Before you can begin using the API, you need to obtain your API key from the dashboard. If you do not have an API key, follow [this guide](#api-key).

## Library

Actually, we don't have a library for Flutter, but you can use the API to check if the app needs to be updated, and based on the response, implement your own logic to handle the updates.

Check [api-integration](api-integration) for more information about the API.

## Examples

### Integrate a flutter app with the ForceUpdate API

Remember to replace the `api_key` with your own key.

```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<void> checkVersion() async {
  final response = await http.post(
    Uri.parse('https://api.forceupdate.app/check-version'),
    headers: <String, String>{
      'Content-Type': 'application/json; charset=UTF-8',
    },
    body: jsonEncode(<String, String>{
      'version': '1.0.0',
      'platform': 'ANDROID',
      'language': 'en',
      'api_key': '727fb40897b3c11dce6be9f72b7e3cbaea73fe030565392084fbd7f449d09fd3',
    }),
  );

  if (response.statusCode == 200) {
    final Map<String, dynamic> data = jsonDecode(response.body);
    if (data['update']) {
      // Show a dialog to the user
    }
  } else {
    throw Exception('Failed to check version');
  }
}
```
