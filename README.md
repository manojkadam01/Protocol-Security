# IEC 61850 MMS Demo

This folder has only two demo flows:

```text
Without_TLS = open/readable MMS
With_TLS    = encrypted TLS traffic
```

Do not run anything from the GitHub source folders for the demo.
Run only the EXE files inside this folder.

## Folder

```text
D:\Workshop Demo\Protocol_Security
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

Use this first when you want to show readable MMS packets.

Start Wireshark:

```text
Interface: Adapter for loopback traffic capture
Filter:    mms
```

Run server first:

```cmd
cd /d "D:\Workshop Demo\Protocol_Security\Without_TLS\Server"
SERVER.exe
```

Run client second:

```cmd
cd /d "D:\Workshop Demo\Protocol_Security\Without_TLS\Client"
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

The client keeps running and reading values.
Stop it with `CTRL+C`.

If you still see old `Unconfirmed <RPT>` rows, stop Wireshark capture and start a new capture. The old packets are already saved in the current view.

## 2. With TLS

Use this second when you want to show encrypted traffic.

Start Wireshark:

```text
Interface: Adapter for loopback traffic capture
Filter:    tls || tcp.port == 10201
```

Run TLS server first:

```cmd
cd /d "D:\Workshop Demo\Protocol_Security\With_TLS\Server"
TLS_SERVER_PORT_10201.exe
```

Run TLS client second:

```cmd
cd /d "D:\Workshop Demo\Protocol_Security\With_TLS\Client"
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

If you move the demo to another system or change the TLS server IP, regenerate only the TLS certificates:

```cmd
cd /d "D:\Workshop Demo\Protocol_Security\With_TLS"
REGENERATE_TLS_CERTS_FOR_NEW_IP.exe
```

Then run the TLS server and TLS client again.

For another system, the TLS client can run with a server IP:

```cmd
cd /d "D:\Workshop Demo\Protocol_Security\With_TLS\Client"
TLS_CLIENT_PORT_10201.exe SERVER_IP_HERE
```
