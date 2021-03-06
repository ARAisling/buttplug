# Deprecated Messages

The following messages are considered deprecated, either because
they've been duplicated by new messages, or because their message
version has changed and they represent an old version of a message.

While some older versions of messages are not required to be
implemented unless a server wants to support version fallback, other
messages, such as deprecated device commands, should most likely be
implemented in all servers. Any reference servers provided by the
Buttplug Core Team will support all messages, even those that have
been deprecated.

---
### SingleMotorVibrateCmd

**Reason for Deprecation:** Superceded by
[VibrateCmd](generic.md#vibratecmd), which provided granular control
of an unlimited number of motors. Should most likely still be
implemented in servers, in order to support older applications, but is
not recommended for use in new client applications.

**Description:** Causes a device that supports vibration to run all
vibration motors at a certain speed.

**Introduced In Spec Version:** 0

**Fields:**

* _Id_ (unsigned int): Message Id
* _DeviceIndex_ (unsigned int): Index of device
* _Speed_ (double): Vibration speed with a range of [0.0-1.0]

**Expected Response:**

* Ok message with matching Id on successful request.
* Error message on value or message error.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>+Server: SingleMotorVibrateCmd Id=1
    Server->>-Client: Ok Id=1
</mermaid>

**Serialization Example:**

```json
[
  {
    "SingleMotorVibrateCmd": {
      "Id": 1,
      "DeviceIndex": 0,
      "Speed": 0.5
    }
  }
]
```

---
## DeviceList Version 0

**Reason for Deprecation:** Superceded by [DeviceList Version
1](enumeration.md#devicelist), which provides more information about
feature limits of generic messages.

**Description:** Server reply to a client request for a device list.

**Introduced In Spec Version:** 0

**Fields:**

* _Id_ (unsigned int): Message Id
* _Devices_ (array): Array of device objects
  * _DeviceName_ (string): Descriptive name of the device
  * _DeviceIndex_ (unsigned integer): Index used to identify the device when sending Device Messages.
  * _DeviceMessages_ (array of strings): Type names of Device Messages that the device will accept.

**Expected Response:**

None. Server-to-Client message only.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>+Server: RequestDeviceList Id=1
    Server->>-Client: DeviceList Id=1
</mermaid>

**Serialization Example:**

```json
[
  {
    "DeviceList": {
      "Id": 1,
      "Devices": [
        {
          "DeviceName": "TestDevice 1",
          "DeviceIndex": 0,
          "DeviceMessages": ["SingleMotorVibrateCmd", "RawCmd", "KiirooCmd", "StopDeviceCmd"]
        },
        {
          "DeviceName": "TestDevice 2",
          "DeviceIndex": 1,
          "DeviceMessages": ["SingleMotorVibrateCmd", "LovenseCmd", "StopDeviceCmd"]
        }
      ]
    }
  }
]
```
---
### DeviceAdded Version 0

**Reason for Deprecation:** Superceded by [DeviceList Version
1](enumeration.md#devicelist), which provides more information about
feature limits of generic messages.

**Introduced In Spec Version:** 0

**Fields:**

* _Id_ (unsigned int): Message Id
* _DeviceName_ (string): Descriptive name of the device
* _DeviceIndex_ (unsigned integer): Index used to identify the device
  when sending Device Messages.
* _DeviceMessages_ (array of strings): Type names of Device Messages
  that the device will accept.

**Expected Response:**

None. Server-to-Client message only.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    participant Client
    participant Server
    Server->>Client: DeviceAdded Id=0
</mermaid>

**Serialization Example:**

```json
[
  {
    "DeviceAdded": {
      "Id": 0,
      "DeviceName": "TestDevice 1",
      "DeviceIndex": 0,
      "DeviceMessages": ["SingleMotorVibrateCmd", "RawCmd", "KiirooCmd", "StopDeviceCmd"]
    }
  }
]
```
---
### RequestServerInfo Version 0

**Reason for Deprecation:** Superceded by [RequestServerInfo Version
1](identification.md#requestserverinfo), adding message version check.

**Description:** Sent by the client to register itself with the server, and request info from the server.

**Spec Version:** 0

**Fields:**

* _Id_ (unsigned int): Message Id
* _ClientName_ (string): Name of the client, for the server to use for UI if needed. Cannot be null.

**Expected Response:**

* ServerInfo message on success
* Error message on malformed message, null client name, or other error.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>Server: RequestServerInfo Id=0
    Server->>Client: ServerInfo Id=0
</mermaid>

**Serialization Example:**

```json
[
  {
    "RequestServerInfo": {
      "Id": 1,
      "ClientName": "Test Client"
    }
  }
]
```
