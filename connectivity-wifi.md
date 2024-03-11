---
description: Just some Study Note
---

# Connectivity - WiFi

## Reference

{% embed url="https://source.android.com/docs/core/connect/wifi-overview" %}

{% embed url="https://aaltodoc.aalto.fi/server/api/core/bitstreams/4a521391-e53a-4d8f-b49a-d776d0467a0a/content" %}

## Overall technolodgies

* Wi-Fi infrastructure (STA)
* Wi-Fi hotspot (Soft AP) in either tethered or local-only modes
* Wi-Fi Direct (p2p)
* Wi-Fi Aware (NAN)
* Wi-Fi RTT (IEEE 802.11mc FTM)

## Software Architecture

<figure><img src=".gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

`system/connectivity/wificond` process communicates with the Wi-Fi driver over standard `nl80211` commands.

## Wi-Fi multi-interface concurrency <a href="#wi-fi_multi-interface_concurrency" id="wi-fi_multi-interface_concurrency"></a>

Support different combinations of Wi-Fi interfaces concurrently.



## Wi-Fi P2P

{% embed url="https://www.youtube.com/watch?v=ttROgEIqPys" %}

{% embed url="https://devopedia.org/wi-fi-direct" %}

## Wi-Fi NAN

{% embed url="https://yichen.link/?log=blog&id=86" %}

{% embed url="https://www.youtube.com/watch?v=YcUf3RfEpTQ" %}

1\) Range apply



## P2P

Wi-Fi Aware is a new technology for P2P connection. It is designed to discover nearby peers without any manually searching or pairing,
