# OPC UA Monitoring Guide 📡🔍

## Overview 🚀

This guide provides instructions on how to implement monitoring using OPC UA with `FbMonitorLathingA1`. It includes setting up monitored items, handling subscriptions, and managing monitored values.

## Prerequisites ✅

- An operational OPC UA server.
- A properly configured PLC with OPC UA capabilities.

## Setting Up Monitoring 🔧

### Variable Declaration 📝

Before enabling monitoring, declare the monitoring function block:

```structured-text
(* Monitoring setup *)
VAR
    FbMonitorLathingA1 : FB_OpcUaCli_Monitor;
END_VAR
```

📌 Declare `FbMonitorLathingA1` to manage OPC UA monitoring.

### Monitoring Configuration ⚙️

To set up monitored items, configure `FbMonitorLathingA1` as follows:

```structured-text
(***************** Monitoring*****************)
FbMonitorLathingA1.Execute := 2;
FbMonitorLathingA1.Reset := gBP.ErrorReset;
FbMonitorLathingA1.NbMonitoredItems := 2;
FbMonitorLathingA1.ConnectionHdl := FbOpcUaConnectLathingA1.ConnectionHdl;

FbMonitorLathingA1.NodeID_0[0].NamespaceIndex := 2;
FbMonitorLathingA1.NodeID_0[0].Identifier := 'Demo.Dynamic.Scalar.String';
FbMonitorLathingA1.NodeID_0[0].IdentifierType := UAIdentifierType_String;
FbMonitorLathingA1.Variable[0] := '::UM_Logic:MonitorLtuRecipeName';

FbMonitorLathingA1.NodeID_0[1].NamespaceIndex := 3;
FbMonitorLathingA1.NodeID_0[1].Identifier := 'Demo.Dynamic.Scalar.DateTime';
FbMonitorLathingA1.NodeID_0[1].IdentifierType := UAIdentifierType_String;
FbMonitorLathingA1.Variable[1] := '::UM_Logic:MonitorLtuRecipeName1';
```

📌 Ensure `Execute` is enabled when the connection is active.
📌 Set `NbMonitoredItems` to define the number of monitored variables.
📌 Map each monitored item to a local variable.

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

