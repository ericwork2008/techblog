# Android App Security

For apps targeting Android 14, Android restricts apps from sending implicit intents to internal app components in the following ways:

* Implicit intents are only delivered to exported components. Apps must either use an explicit intent to deliver to unexported components, or mark the component as exported.
* If an app creates a mutable pending intent with an intent that doesn't specify a component or package, the system now throws an exception.

## Intent receiver exported&#x20;

RECEIVER\_NOT\_EXPORTED if you only want your receiver to be able to receive broadcasts **from the system** or your own app.&#x20;

Same as registered receiver with "exported=false"

#### Runtime-registered broadcasts receivers must specify export behavior <a href="#runtime-receivers-exported" id="runtime-receivers-exported"></a>

`RECEIVER_EXPORTED` or `RECEIVER_NOT_EXPORTED`

{% embed url="https://developer.android.com/about/versions/14/behavior-changes-14#security" %}
