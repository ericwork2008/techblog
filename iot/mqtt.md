# MQTT

##

{% embed url="https://www.hivemq.com/blog/mqtt-essentials-part-1-introducing-mqtt/" %}

Study Node: from above Links



## Overview

MQTT is a “lightweight” Client Server publish/subscribe messaging transport protocol.

1\) MQTT uses a binary message format

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

2\) well-suited for use in low-bandwidth or low-power environments

* Simple implementation
* Quality of Service data delivery
* Lightweight and bandwidth-efficient
* Data agnostic
* Continuous session awareness

## On top of TCP

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="563"><figcaption></figcaption></figure>

## MQTT's Messaging Model <a href="#heading-mqt-ts-messaging-model-how-it-works-and-why-it-matters-for-io-t-i-io-t" id="heading-mqt-ts-messaging-model-how-it-works-and-why-it-matters-for-io-t-i-io-t"></a>

<figure><img src="../.gitbook/assets/image (6).png" alt="" width="563"><figcaption></figcaption></figure>

Topics: strings that messages are published to and subscribed to. **Case sensitive**

Subscriptions: specify which topics a client is interested in receiving messages from



Scalability/Decouple, but it have broker Single-Point-Failure problem.

### MQTT, message persistence

MQTT, message persistence is achieved by storing messages on the server until they are delivered to the subscriber.

persistence options: Non-persistent/Queued persistent/Persistent with acknowledgment

<figure><img src="../.gitbook/assets/image (9).png" alt="" width="563"><figcaption></figcaption></figure>

## Connect Setup

<figure><img src="../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>

## Communication

PUBLISH/SUBSCRIBE/SUBACK/UNSUBSCRIBE

## MQTT QoS

QoS 0: at most once

QoS 1: at least once

QoS 2: exactly once. received exactly once by the subscriber

<figure><img src="../.gitbook/assets/image (8).png" alt="" width="563"><figcaption></figcaption></figure>

## MQTT Security <a href="#heading-mqtt-security-protecting-your-io-t-devices-from-cyber-attacks" id="heading-mqtt-security-protecting-your-io-t-devices-from-cyber-attacks"></a>

## LWT

<figure><img src="../.gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure>

## Keep alive & client takeover

<figure><img src="../.gitbook/assets/image (11).png" alt="" width="563"><figcaption></figcaption></figure>
