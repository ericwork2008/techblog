---
description: From ChatGPT
---

# How Wi-Fi Direct get IP address?

Devices participating in a Wi-Fi Direct group negotiate and assign IP addresses within the 192.168.49.0/24 subnet.

Here's a simplified explanation of how IP address assignment works in Wi-Fi Direct:

1. **Group Owner (GO) Role:**
   * In a Wi-Fi Direct group, one device takes on the role of the Group Owner (GO). The GO acts as an access point, and other devices connect to it.
2. **DHCP:**
   * The Group Owner typically runs a DHCP server to dynamically assign IP addresses to connected devices. This DHCP server is responsible for providing each connected device with an IP address, subnet mask, default gateway, and other network configuration parameters.
3. **Automatic IP Configuration:**
   * Devices joining the Wi-Fi Direct group, known as Clients or Peers, use DHCP to automatically obtain an IP address from the Group Owner. The negotiation process involves the client requesting an IP address, and the GO responding with the assigned IP information.
4. **IP Address Range:**
   * The default IP address range used for Wi-Fi Direct connections is often in the 192.168.49.0/24 subnet. This means that IP addresses assigned to connected devices will fall within the range 192.168.49.1 to 192.168.49.254.

Here's a snippet of code illustrating how you might obtain IP information programmatically in an Android application using Wi-Fi Direct:

```java
javaCopy codepublic class WifiDirectActivity extends Activity {

    private WifiP2pManager wifiP2pManager;
    private WifiP2pManager.Channel channel;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        wifiP2pManager = (WifiP2pManager) getSystemService(WIFI_P2P_SERVICE);
        channel = wifiP2pManager.initialize(this, getMainLooper(), null);

        // Other initialization code...
    }

    private void requestConnectionInfo() {
        wifiP2pManager.requestConnectionInfo(channel, new WifiP2pManager.ConnectionInfoListener() {
            @Override
            public void onConnectionInfoAvailable(WifiP2pInfo info) {
                if (info.groupFormed && info.isGroupOwner) {
                    // This device is the Group Owner (GO)
                    String groupOwnerAddress = info.groupOwnerAddress.getHostAddress();
                    // Use groupOwnerAddress as the IP address of the GO
                } else if (info.groupFormed) {
                    // This device is a client
                    String groupOwnerAddress = info.groupOwnerAddress.getHostAddress();
                    // Use groupOwnerAddress as the IP address of the GO
                }
            }
        });
    }
}
```

In this example, the `WifiP2pManager.ConnectionInfoListener` is used to retrieve connection information, including the IP address of the Group Owner. The IP address is obtained from the `WifiP2pInfo` object, and you can use it as needed in your application logic. Keep in mind that this is a simplified example, and error handling and additional logic may be required for a production-level application.
