---
description: Just some Study Note
---

# Connectivity - WiFi

## Reference

{% embed url="https://source.android.com/docs/core/connect/wifi-overview" %}

## Overall technolodgies

* Wi-Fi infrastructure (STA)
* Wi-Fi hotspot (Soft AP) in either tethered or local-only modes
* Wi-Fi Direct (p2p)
* Wi-Fi Aware (NAN)
* Wi-Fi RTT (IEEE 802.11mc FTM)

## Software Architecture

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

`system/connectivity/wificond` process communicates with the Wi-Fi driver over standard `nl80211` commands.

## Wi-Fi multi-interface concurrency <a href="#wi-fi_multi-interface_concurrency" id="wi-fi_multi-interface_concurrency"></a>

Support different combinations of Wi-Fi interfaces concurrently.



## Wi-Fi P2P

{% embed url="https://www.youtube.com/watch?v=ttROgEIqPys" %}
