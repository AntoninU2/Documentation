# OPC UA Connection Guide ⚡🔧💻

## Overview 🚀

This guide provides detailed instructions on how to establish and manage an OPC UA connection using `FbOpcUaConnectToServer`, as well as utilizing function blocks such as `UA_Connect` and `UA_GetNamespaceIndex`.

## Prerequisites ✅

- An operational OPC UA server.
- A properly configured PLC with OPC UA capabilities.

## Function Block Structure

### `FB_OpcUaCli_Connect`

This function block establishes and manages an OPC UA connection.

#### Input Variables:
- **UserInfos**: User authentication credentials.
- **ServerEndpointUrl**: The URL of the OPC UA server.
- **NamespaceUri**: Namespace URI for communication.
- **Connect**: Set to `TRUE` to initiate a connection.
- **Disconnect**: Set to `TRUE` to disconnect from the server.
- **Reset**: Set to `TRUE` to reset the connection in case of an error.

#### Output Variables:
- **Connected**: `TRUE` if the connection is successful.
- **Error**: `TRUE` if an error occurs.
- **ErrorID**: Error identifier.
- **NamespaceIndex**: The retrieved namespace index.
- **ConnectionHdl**: Connection handle.

## Establishing a Connection to the Ltu LathingA1 OPC UA Server 📎🛏

### Variable Declaration 📝

Before establishing a connection, declare the connection function block:

```structured-text
(* Connect to server *)
VAR
    FbOpcUaConnectToServer : FB_OpcUaCli_Connect;
END_VAR
```

### Connection Setup ⚙️

To establish a connection to the OPC UA server, configure the function block as follows:

```structured-text
FbOpcUaConnectToServer.Connect := TRUE;
FbOpcUaConnectToServer.Reset := gBP.ErrorReset;
FbOpcUaConnectToServer.ServerEndpointUrl := 'opc.tcp://opcuaserver.com:48010';
FbOpcUaConnectToServer.NamespaceUri := 'http://www.unifiedautomation.com/DemoServer/';
FbOpcUaConnectToServer();
```

### Explanation 🔍

- `Connect`: Initiates the connection when the server is running.
- `Reset`: Resets the connection in case of an error.
- `ServerEndpointUrl`: Specifies the address of the OPC UA server.
- `NamespaceUri`: Defines the namespace for communication.

## Utilizing `UA_Connect` 🔌

`UA_Connect` establishes a connection between an OPC UA client and an OPC UA server and is executed only once per connection.

### Code Example 🖥️

```structured-text
SessionConnectInfo_0.SecurityMsgMode := UASecurityMsgMode_None;
SessionConnectInfo_0.SecurityPolicy := UASecurityPolicy_None;
SessionConnectInfo_0.TransportProfile := UATP_UATcp;
SessionConnectInfo_0.UserIdentityToken.UserIdentityTokenType := UAUITT_Username;
SessionConnectInfo_0.UserIdentityToken.TokenParam1 := 'admin';
SessionConnectInfo_0.UserIdentityToken.TokenParam2 := 'password';
SessionConnectInfo_0.SessionTimeout := T#1m;
SessionConnectInfo_0.MonitorConnection := T#10s;

UA_Connect_0(Execute := ExecuteConnect_0,
   ServerEndpointUrl := 'opc.tcp://localhost:4841',
   SessionConnectInfo := SessionConnectInfo_0,
   Timeout := T#10s);

IF (UA_Connect_0.Busy = 0) THEN
   ExecuteConnect_0 := 0;
   IF (UA_Connect_0.Done = 1) THEN
       ConnectionHdl := UA_Connect_0.ConnectionHdl;
   END_IF
   IF (UA_Connect_0.Error = 1) THEN
       ConnectionHdl := 0;
   END_IF
END_IF
```

### Explanation 🤨

- Configures security settings (no security in this example).
- Sets up user authentication with a username and password.
- Establishes a connection to the OPC UA server.
- Checks for a successful connection and stores the connection handle.

## Retrieving the Namespace Index Using `UA_GetNamespaceIndex` 🔍

This function block reads the namespace index from the OPC UA server.

### Code Example 🖥️

```structured-text
UA_GetNamespaceIndex_0(Execute := ExecuteGetnamespaceindex_0,
    ConnectionHdl := ConnectionHdl,
    NamespaceUri := 'http://br-automation.com/OpcUa/PLC/PV/',
    Timeout := T#5s);

IF (UA_GetNamespaceIndex_0.Busy = 0) THEN
    ExecuteGetnamespaceindex_0 := 0;
    IF (UA_GetNamespaceIndex_0.Done = 1) THEN
        NamespaceIndex := UA_GetNamespaceIndex_0.NamespaceIndex;
    END_IF
    IF (UA_GetNamespaceIndex_0.Error = 1) THEN
        NamespaceIndex := 0;
    END_IF
END_IF
```

### Explanation 🤨

- `Execute`: Initiates execution of the function block.
- `ConnectionHdl`: Uses the established connection handle.
- `NamespaceUri`: Specifies the namespace URI to retrieve.
- `Timeout`: Defines the execution timeout.

## Troubleshooting 🛠️

| Issue                 | Possible Cause                | Solution                       |
| --------------------- | ----------------------------- | ------------------------------ |
| Connection failure    | Incorrect `ServerEndpointUrl` | Verify server address and port |
| Authentication error  | Incorrect username/password   | Ensure correct credentials     |
| Namespace index error | Incorrect `NamespaceUri`      | Verify the namespace URI       |

## Conclusion 🎯💪

This document provides a step-by-step guide on establishing an OPC UA connection, retrieving namespace indices, and troubleshooting potential issues. Ensure that your PLC configuration and server details are correctly set up for successful implementation.

