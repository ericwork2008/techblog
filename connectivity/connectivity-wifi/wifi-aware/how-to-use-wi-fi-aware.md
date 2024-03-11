---
description: ChatGPT
---

# How to use Wi-Fi Aware

##

{% embed url="https://developer.android.com/develop/connectivity/wifi/wifi-aware" %}

## REF sample code

{% embed url="https://github.com/anagramrice/NAN/blob/DataTransfer-feature/app/src/main/java/net/mobilewebprint/nan/MainActivity.java#L827" %}

{% embed url="https://github.com/davidsalido/CrowdStreaming" %}

## Concepts in Android API&#x20;

From ChatGPT

In Wi-Fi Aware, the relationship between `WifiAwareSession`, `DiscoverySession`, and a connection can be understood as follows:

1. **WifiAwareSession:**
   * `WifiAwareSession` represents the overall communication session established between devices using Wi-Fi Aware. It is the entry point for creating and managing various aspects of Wi-Fi Aware communication. A single `WifiAwareSession` is created when a device attaches to the Wi-Fi Aware service.
2. **DiscoverySession:**
   * `DiscoverySession` is a sub-session within the overall `WifiAwareSession`. It is created when a device initiates or receives a service discovery request. During service discovery, devices advertise and discover services offered by one another. The `DiscoverySession` allows devices to exchange information about available services.
3. **Connection in Wi-Fi Aware:**
   * A connection in Wi-Fi Aware refers to the establishment of a communication link between two devices after a service discovery process. Once a device discovers a peer offering a desired service, a connection can be initiated to enable communication. The actual connection may involve setting up a network connection, creating sockets, and exchanging data.

Here's a simplified sequence of events:

* **Establishing WifiAwareSession:**
  * A device initiates a `WifiAwareSession` by attaching to the Wi-Fi Aware service.
* **Initiating Service Discovery:**
  * Within the `WifiAwareSession`, a device can initiate a service discovery to find other devices offering specific services.
* **Creating DiscoverySession:**
  * As part of the service discovery process, a `DiscoverySession` is created. This session allows devices to exchange information about services.
* **Discovering Peers:**
  * Devices participating in the service discovery exchange information about the services they offer. This information includes service names, service-specific data, etc.
* **Initiating a Connection:**
  * After discovering a peer offering a desired service, a connection can be initiated. This typically involves creating network specifiers or establishing a network connection.
* **Communication Over the Connection:**
  * Once the connection is established, devices can communicate with each other. This communication might involve exchanging messages, data, or establishing further communication channels like sockets.

In summary, `WifiAwareSession` is the overarching session, `DiscoverySession` is a subset for service discovery, and a connection refers to the establishment of a communication link between devices participating in Wi-Fi Aware. These components work together to enable efficient peer-to-peer communication over Wi-Fi Aware.
