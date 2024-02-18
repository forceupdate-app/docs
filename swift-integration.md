# Swift integration

This guide will help you to integrate ForceUpdate into your Swift app.

## Getting Started

Before you can begin using the API, you need to obtain your API key from the dashboard. If you do not have an API key, follow [this guide](#api-key).

## Library

Actually, we don't have a library for Swift, but you can use the API to check if the app needs to be updated, and based on the response, implement your own logic to handle the updates.

Check [api-integration](api-integration) for more information about the API.

## Examples

### Integrate a swift app with the ForceUpdate API

Remember to replace the `api_key` with your own key.

```swift
import Foundation

func checkAppVersion(apiKey: String, platform: String, version: String, language: String) {
  let url = URL(string: "https://api.forceupdate.app/check-version")!
  var request = URLRequest(url: url)
  request.httpMethod = "POST"

  let bodyParams: [String: Any] = [
    "platform": platform,
    "version": version,
    "api_key": apiKey,
    "language": language
  ]

  do {
    request.httpBody = try JSONSerialization.data(withJSONObject: bodyParams, options: [])
  } catch {
    print("Error serializing request body: \(error)")
    return
  }

  let task = URLSession.shared.dataTask(with: request) { (data, response, error) in
    if let error = error {
      print("Error: \(error)")
      return
    }

    guard let data = data else {
      print("No data received")
      return
    }

    do {
      let json = try JSONSerialization.jsonObject(with: data, options: [])
      if let responseDict = json as? [String: Any] {
        // Handle the response here
        let needsUpdate = responseDict["needs_update"] as? Bool ?? false
        let forceUpdate = responseDict["force_update"] as? Bool ?? false
        let title = responseDict["title"] as? String ?? ""
        let message = responseDict["message"] as? String ?? ""
        let updateButtonText = responseDict["update_button_text"] as? String ?? ""
        let dismissButtonText = responseDict["dismiss_button_text"] as? String ?? ""
        let storeURL = responseDict["store_url"] as? String ?? ""

        // Use the response values as needed
        print("Needs Update: \(needsUpdate)")
        print("Force Update: \(forceUpdate)")
        print("Title: \(title)")
        print("Message: \(message)")
        print("Update Button Text: \(updateButtonText)")
        print("Dismiss Button Text: \(dismissButtonText)")
        print("Store URL: \(storeURL)")
      }
    } catch {
      print("Error deserializing response: \(error)")
    }
  }

  task.resume()
}

// Example usage
let apiKey = "727f..."
let platform = "IOS"
let version = "1.0.0"
let language = "en"

checkAppVersion(apiKey: apiKey, platform: platform, version: version, language: language)
```
