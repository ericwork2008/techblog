# Connectivity - Bluetooth

## References

{% embed url="https://learn.adafruit.com/introduction-to-bluetooth-low-energy/introduction" %}

{% embed url="https://docs.silabs.com/bluetooth/3.2/general/connections/bluetooth-connection-flowcharts" %}

{% embed url="https://medium.com/rtone-iot-security/a-brief-overview-of-bluetooth-low-energy-79be06eab4df" %}

## GAP (Generic Access Profile)

makes your device visible to the outside world, and determines how two devices can (or can't) interact with each other.

### Device Role

* Peripheral, devices are small, low power, resource contrained devices that can connect to a much more powerful central device.
* Central

Advertising data payload: transmitted out from the device to let central devices in range know that it exists. There are two ways to send advertising out with GAP. The _Advertising Data_ payload and the _Scan Response_ payload.

### Advertising & Scan

Only the scanning central device can initiate the connection.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>From <a href="https://learn.adafruit.com/introduction-to-bluetooth-low-energy/gap">https://learn.adafruit.com/introduction-to-bluetooth-low-energy/gap</a></p></figcaption></figure>



<figure><img src="https://docs.silabs.com/resources/bluetooth/general/connections/bluetooth-connection-flowcharts/images/advertise-scan.png" alt=""><figcaption></figcaption></figure>

### Connect

<figure><img src="https://docs.silabs.com/resources/bluetooth/general/connections/bluetooth-connection-flowcharts/images/connection.png" alt=""><figcaption></figcaption></figure>

### BroadCast

**BroadCast:** peripheral to send data (in advertising packet) to more than one device at a time. Once you establish a connection between your peripheral and a central device, the advertising process will generally stop and you will typically no longer be able to send advertising packets out anymore, and you will use GATT services and characteristics to communicate in both directions.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="https://learn.adafruit.com/assets/13825" alt=""><figcaption></figcaption></figure>

## GATT (**Generic ATTribute** Profile)

&#x20;it defines the way that two Bluetooth Low Energy devices transfer data back and forth using concepts called **Services** and **Characteristics**. GATT comes into play once a dedicated connection is established between two devices, meaning that you have already gone through the advertising process governed by GAP.

<figure><img src="../../.gitbook/assets/image (2).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="416"><figcaption></figcaption></figure>

**Profile:** it's simply a pre-defined collection of Services that has been compiled by either the Bluetooth SIG or by the peripheral designers. We can see all GATT-based profiles in [https://www.bluetooth.com/specifications/specs/](https://www.bluetooth.com/specifications/specs/)

**Service:** A service can have one or more characteristics, and each service distinguishes itself from other services by means of a unique numeric ID called a UUID, which can be either 16-bit (for officially adopted BLE Services) or 128-bit (for custom services). BLE services can be seen on the [Services](https://www.bluetooth.com/specifications/gatt/services).

**Characteristics:** each Characteristic distinguishes itself via a pre-defined 16-bit or 128-bit UUID, and you're free to use the [standard characteristics defined by the Bluetooth SIG](https://www.bluetooth.com/specifications/gatt/characteristics).  All 16-bit UUIDs are defined by the Bluetooth SIG and are known as **adopted** UUIDs. All 128-bit UUIDs are custom and may be used for any purpose without approval from the Bluetooth SIG.



### Communication

* **GATT client** - a device which accesses data on the remote GATT server via read, write, notify, or indicate operations
* **GATT server** - a device which stores data locally and provides data access methods to a remote GATT client

it is most common for the peripheral device to be the GATT server and the central device to be the GATT client. GATT is built on top of ATT, leveraging its attribute-based communication to structure and organize data in a way that makes sense for specific applications.

* You can use **read, write, notify**, or **indicate** operations to move data between the client and the server.
  * **Read** and **write** operations are requested by the client and the server responds (or acknowledges).
  * **Notify** and **indicate** operations are enabled by the client but initiated by the server, providing a way to push data to the client.
  * **Notifications** are unacknowledged, while **indications** are acknowledged. Notifications are therefore faster but less reliable.

Following link have the detail flow

{% embed url="https://docs.silabs.com/bluetooth/3.2/general/gatt-protocol/gatt-operation-flowcharts" %}

## LE Security

{% embed url="https://www.allaboutcircuits.com/technical-articles/bluetooth-le-security-modes-and-procedures-explained/" %}

"security procedures" refer to the actual actions or processes used to ensure the security of the Bluetooth LE device. Different modes give different levels of safety. Five security procedures: Encryption, Authentication, Data Signing, Encrypted Advertising, and Authorization.

### Authentication

'**authentication**' is the process that verifies the identity of a user, device, or system. Avoid MITM attacks.

1. **Passkey entry**: One device displays this passkey during the pairing process, and the user confirms and enters it on the other device to authenticate the pairing.
2. **Numeric key comparison:** a six-digit numeric key is displayed on both devices. The user is then asked to confirm if the numbers match on both devices to authenticate the pairing.

### Encryption

devices collaborate to generate a secret key which they use to encrypt the communication link

### Data signing

1. **Integrity** data hasn't been altered
2. **Authenticity** the signature also confirms that the data genuinely originates from the expected device

### Authorization

authentication ensures devices are who they claim to be, authorization verifies whether a device has the required permissions to access certain resources or perform certain actions.



## Bluetooth SIG Resources

* [Bluetooth Core Specification](https://www.bluetooth.com/specifications/bluetooth-core-specification)
* [Bluetooth Developer Portal](https://www.bluetooth.com/develop-with-bluetooth)
* Officially Adopted [BLE Profiles and Services](https://www.bluetooth.com/specifications/gatt)
* Officially Adopted [BLE Characteristics](https://www.bluetooth.com/specifications/gatt/characteristics)
