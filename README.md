# IEC 61850 MMS Demo

This folder is portable now.
You can copy the full `Protocol_Security` folder to `C:`, `D:`, `E:`, USB, or another Windows folder.

Important:

```text
Move/copy the complete Protocol_Security folder.
Do not move only one EXE by itself.
TLS needs the certificate files beside the TLS EXEs.
```

## Folder

```text
Protocol_Security
|-- README.md
|-- Without_TLS
|   |-- Server
|   |   `-- SERVER.exe
|   `-- Client
|       `-- CLIENT.exe
`-- With_TLS
    |-- REGENERATE_TLS_CERTS_FOR_NEW_IP.exe
    |-- Server
    |   |-- TLS_SERVER_PORT_10201.exe
    |   |-- server_CA1_1.key
    |   |-- server_CA1_1.pem
    |   |-- root_CA1.pem
    |   |-- client_CA1_1.pem
    |   `-- client_CA1_2.pem
    `-- Client
        |-- TLS_CLIENT_PORT_10201.exe
        |-- client_CA1_1.key
        |-- client_CA1_1.pem
        |-- root_CA1.pem
        `-- server_CA1_1.pem
```

## 1. Without TLS

Purpose:

```text
Show open/readable MMS packets.
```

Wireshark:

```text
Interface: Adapter for loopback traffic capture
Filter:    mms
```

Run server first:

```cmd
cd /d "YOUR_PATH\Protocol_Security\Without_TLS\Server"
SERVER.exe
```

Run client second:

```cmd
cd /d "YOUR_PATH\Protocol_Security\Without_TLS\Client"
CLIENT.exe
```

Example if copied to `E:\Demo`:

```cmd
cd /d "E:\Demo\Protocol_Security\Without_TLS\Server"
SERVER.exe
```

```cmd
cd /d "E:\Demo\Protocol_Security\Without_TLS\Client"
CLIENT.exe
```

What to show in Wireshark:

```text
MMS/IEC61850
Associate Request
Associate Response
GetDataValueRequest
GetDataValueResponse
Readable object names like simpleIOGenericIO
```

Note:

```text
Without_TLS uses port 102 so Wireshark shows MMS directly.
SERVER.exe can ask for Administrator permission because Windows protects port 102.
```

## 2. With TLS

Purpose:

```text
Show encrypted TLS traffic.
```

Wireshark:

```text
Interface: Adapter for loopback traffic capture
Filter:    tls || tcp.port == 10201
```

Run TLS server first:

```cmd
cd /d "YOUR_PATH\Protocol_Security\With_TLS\Server"
TLS_SERVER_PORT_10201.exe
```

Run TLS client second:

```cmd
cd /d "YOUR_PATH\Protocol_Security\With_TLS\Client"
TLS_CLIENT_PORT_10201.exe
```

Example if copied to `E:\Demo`:

```cmd
cd /d "E:\Demo\Protocol_Security\With_TLS\Server"
TLS_SERVER_PORT_10201.exe
```

```cmd
cd /d "E:\Demo\Protocol_Security\With_TLS\Client"
TLS_CLIENT_PORT_10201.exe
```

What to show in Wireshark:

```text
TLS handshake
Certificate
TLS Application Data
No readable MMS object names
```

## Difference In Wireshark

```text
Without_TLS + filter mms:
Readable MMS packets and object names are visible.

With_TLS + filter tls || tcp.port == 10201:
TLS packets are visible, but MMS data is encrypted.
```

## New IP For TLS

If the server IP changes, regenerate only the TLS certificates:

```cmd
cd /d "YOUR_PATH\Protocol_Security\With_TLS"
REGENERATE_TLS_CERTS_FOR_NEW_IP.exe
```

Enter the new server IP.

For a client on another system:

```cmd
cd /d "YOUR_PATH\Protocol_Security\With_TLS\Client"
TLS_CLIENT_PORT_10201.exe SERVER_IP_HERE
```
