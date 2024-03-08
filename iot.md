# IOT

1\) SRTP: to get video, web camera

2\) BLE service discovery

3\)&#x20;



## Connectivity

devices / services discovery

USB/WiFi NAN /BT



## Peer to peer Services

1\) Mouse/Camera/Display/File

2\) Auth



## Device group protocol

When multiple devices are connected together in a group, various scenarios and configurations can be considered, depending on the specific use case, application requirements, and the nature of the devices involved. Here are some common scenarios for devices connected in a group:

1. **Point-to-Point (P2P):**
   * Two devices directly communicate with each other. This is a basic one-to-one connection.
2. **Star Topology:**
   * Multiple devices connect to a central hub or node. The central device facilitates communication between all other devices in the network.
3. **Mesh Topology:**
   * Devices are interconnected, allowing for communication between any two devices in the network. Mesh networks are often self-healing and can handle communication even if one or more devices fail.
4. **Daisy Chain:**
   * Devices are connected sequentially, forming a chain. Communication passes from one device to the next.
5. **Tree Topology:**
   * Devices are connected in a hierarchical tree structure, with a central node or nodes connecting to other nodes and so on.
6. **Ring Topology:**
   * Devices are connected in a circular fashion. Each device is connected to exactly two other devices, forming a ring.
7. **Fully Connected (Complete) Topology:**
   * Every device is directly connected to every other device in the network. This can be complex and is often used in smaller networks due to the high number of connections.
8. **Hybrid Topology:**
   * A combination of different topologies. For example, a combination of star and mesh topologies can provide both centralized and decentralized communication.
9. **Client-Server Model:**
   * Devices are divided into clients and servers. Servers provide resources or services, and clients request and use these services.
10. **Peer-to-Peer (P2P) Network:**
    * All devices have equal status and can communicate with each other without a central entity.
11. **Ad Hoc Network:**
    * Devices form a temporary network for a specific purpose without the need for pre-existing infrastructure.
12. **Clustered Configuration:**
    * Devices are grouped into clusters based on some criteria, and communication mainly occurs within clusters.

## MQTT

{% embed url="https://www.hivemq.com/blog/mqtt-essentials-part-1-introducing-mqtt/" %}

MQTT is a “lightweight” Client Server publish/subscribe messaging transport protocol.

1\) MQTT uses a binary message format

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

2\) well-suited for use in low-bandwidth or low-power environments

* Simple implementation
* Quality of Service data delivery
* Lightweight and bandwidth-efficient
* Data agnostic
* Continuous session awareness

### MQTT's Messaging Model <a href="#heading-mqt-ts-messaging-model-how-it-works-and-why-it-matters-for-io-t-i-io-t" id="heading-mqt-ts-messaging-model-how-it-works-and-why-it-matters-for-io-t-i-io-t"></a>

Topics: strings that messages are published to and subscribed to

Subscriptions: specify which topics a client is interested in receiving messages from

### MQTT QoS

QoS 0: at most once

QoS 1: at least once

QoS 2: exactly once. received exactly once by the subscriber

### MQTT, message persistence

MQTT, message persistence is achieved by storing messages on the server until they are delivered to the subscriber.

persistence options: Non-persistent/Queued persistent/Persistent with acknowledgment

### MQTT Security <a href="#heading-mqtt-security-protecting-your-io-t-devices-from-cyber-attacks" id="heading-mqtt-security-protecting-your-io-t-devices-from-cyber-attacks"></a>

