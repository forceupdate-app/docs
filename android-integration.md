# Android integration

This guide will help you to integrate ForceUpdate into your Android app.

## Getting Started

Before you can begin using the API, you need to obtain your API key from the dashboard. If you do not have an API key, follow [this guide](#api-key).

## Library

Actually, we don't have a library for Android, but you can use the API to check if the app needs to be updated, and based on the response, implement your own logic to handle the updates.

Check [api-integration](api-integration) for more information about the API.

## Examples

### Integrate a android app with the ForceUpdate API

Remember to replace the `api_key` with your own key.

```kotlin
import android.os.Bundle
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import com.android.volley.Request
import com.android.volley.Response
import com.android.volley.toolbox.JsonObjectRequest
import com.android.volley.toolbox.Volley
import org.json.JSONObject

class MainActivity : AppCompatActivity() {

override fun onCreate(savedInstanceState: Bundle?) {
super.onCreate(savedInstanceState)
setContentView(R.layout.activity_main)

    val apiEndpoint = "https://api.forceupdate.app/check-version"
    val apiKey = "YOUR_API_KEY"
    val currentVersion = "1.0.0"

    val params = JSONObject()
    params.put("platform", "ANDROID")
    params.put("version", currentVersion)
    params.put("api_key", apiKey)
    params.put("language", "en")

    val request = JsonObjectRequest(Request.Method.POST, apiEndpoint, params,
      Response.Listener { response ->
        val needsUpdate = response.getBoolean("needs_update")
        val forceUpdate = response.getBoolean("force_update")
        val title = response.getString("title")
        val message = response.getString("message")
        val updateButtonText = response.getString("update_button_text")
        val dismissButtonText = response.getString("dismiss_button_text")
        val storeUrl = response.getString("store_url")

        // Implement your logic based on the response
        if (needsUpdate) {
          // Show update dialog or redirect to store
          Toast.makeText(this, "Update available", Toast.LENGTH_SHORT).show()
        } else {
          // App is up to date
          Toast.makeText(this, "App is up to date", Toast.LENGTH_SHORT).show()
        }
      },
      Response.ErrorListener { error ->
        // Handle error
        Toast.makeText(this, "Error: ${error.message}", Toast.LENGTH_SHORT).show()
      })

    Volley.newRequestQueue(this).add(request)
  }
}
```
