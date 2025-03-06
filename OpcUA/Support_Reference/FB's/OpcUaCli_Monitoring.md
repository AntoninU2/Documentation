# OPC UA Monitoring Guide 📡🔍

## Overview 🚀

This guide provides instructions on how to implement monitoring using OPC UA with `FbMonitorVariables`. It includes setting up monitored items, handling subscriptions, and managing monitored values.

## Prerequisites ✅

- An operational OPC UA server.
- A properly configured PLC with OPC UA capabilities.

## Function Block Structure

### `FB_OpcUaCli_Monitor`

This function block is responsible for managing OPC UA monitoring operations.

#### Input Variables:

- **Execute**: Set to TRUE to initiate monitoring.
- **Reset**: Resets monitoring in case of errors.
- **ConnectionHdl**: The active OPC UA connection handle.
- **IsActive**: Active or not the actual variable for monitoring.
- **NodeID\_0**: An array of Node IDs corresponding to the monitored items.
  - **NamespaceIndex**: Specifies the namespace index of the node.
  - **Identifier**: The unique identifier of the monitored item.
  - **IdentifierType**: Specifies the type of identifier (`String`, `Numeric`, etc.).
- **Variable**: An array of variables in the PLC mapped to monitored values.

#### Output Variables:

- **Done**: `TRUE` if the reading process is successfully completed.
- **Error**: `TRUE` if an error occurs during the reading process.
- **ErrorIndex**: Index of the node that caused an error.
- **ErrorID**: Error identifier providing details on the issue.

## Setting Up Monitoring 🔧

### Variable Declaration 📝

Before enabling monitoring, declare the monitoring function block:

```structured-text
(* Monitoring setup *)
VAR
    FbMonitorVariables  : FB_OpcUaCli_Monitor;
END_VAR
```
```structured-text
(* Monitoring variables *)
VAR
    MonitorMachineRate   : REAL;
    MonitorMachineStart  : BOOL;
END_VAR
```
### Monitoring Configuration ⚙️

To set up monitored items, configure `FbMonitorVariables` as follows:

```structured-text
(***************** Monitoring *****************)
FbMonitorVariables.Execute             := FbOpcUaConnectToServer.Connected;
FbMonitorVariables.Reset               := gBP.ErrorReset;
FbMonitorVariables.ConnectionHdl       := FbOpcUaConnectToServer.ConnectionHdl;

FbMonitorVariables.IsActive[0]                   := TRUE;
FbMonitorVariables.NodeID_0[0].NamespaceIndex    := FbOpcUaConnectToServer.NamespaceIndex; //Index of the URI namespace
FbMonitorVariables.NodeID_0[0].Identifier        := 'Demo.Dynamic.Scalar.String';
FbMonitorVariables.NodeID_0[0].IdentifierType    := UAIdentifierType_String;
FbMonitorVariables.Variable[0]                   := '::UM_Logic:MonitorMachineRate';

FbMonitorVariables.IsActive[1]                   := TRUE;
FbMonitorVariables.NodeID_0[1].NamespaceIndex    := 3;    //Index of the specific namespace
FbMonitorVariables.NodeID_0[1].Identifier        := 'Demo.Dynamic.Scalar.DateTime';
FbMonitorVariables.NodeID_0[1].IdentifierType    := UAIdentifierType_String;
FbMonitorVariables.Variable[1]                   := '::UM_Logic:MonitorMachineStart';
```

### Explanation 🔍

- `Execute`: Controls whether monitoring is active.
- `IsActive`: Specifies if the variable is monitored.
- `ConnectionHdl`: Ensures the connection to the OPC UA server is active.
- `NodeID_0`: Specifies the nodes being monitored.
- `Variable`: Maps monitored items to PLC variables.

## Function Block `FB_OpcUaCli_Monitor` 🔄

The function block `FB_OpcUaCli_Monitor` handles monitoring in multiple steps:

1. **Initializing Monitoring** 🏁
2. **Getting Node Handles** 🔗
3. **Creating Subscriptions** 📡
4. **Adding Monitored Items** ➕
5. **Waiting for Changes** ⏳
6. **Removing Monitored Items** ❌
7. **Deleting Subscriptions** 🗑️
8. **Releasing Node Handles** 🔄

## Troubleshooting 🛠️

| Issue                    | Possible Cause          | Solution                        |
| ------------------------ | ----------------------- | ------------------------------- |
| No data updates          | Incorrect `NodeID`      | Verify node identifiers         |
| Subscription fails       | Connection issue        | Ensure `ConnectionHdl` is valid |
| Error in `MONITOR_ERROR` | Invalid namespace index | Verify `NamespaceIndex`         |

## Conclusion 🎯✅

This guide provides a step-by-step approach to setting up OPC UA monitoring, handling subscriptions, and troubleshooting issues. Ensure that your OPC UA server has the correct namespace indices and variables for proper monitoring.

