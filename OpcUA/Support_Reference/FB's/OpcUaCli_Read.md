# OPC UA Read Function Block Guide 📖🔧

## Overview 🚀

This guide provides detailed instructions on how to use the `FB_OpcUaCli_Read` function block to read data from an OPC UA server and store it in local PLC variables.

## Prerequisites ✅

- A functional OPC UA server.
- A PLC with OPC UA client capabilities.
- Established OPC UA connection using `FB_OpcUaCli_Connect`.

## Function Block Structure 🏗️

### `FB_OpcUaCli_Read`

This function block reads values from specified OPC UA nodes and writes them to local PLC variables.

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
- **NodeAddInfo\_0**: Additional node information if required.


#### Output Variables:

- **Done**: `TRUE` if the reading process is successfully completed.
- **Error**: `TRUE` if an error occurs during the reading process.
- **ErrorIndex**: Index of the node that caused an error.
- **ErrorID**: Error identifier providing details on the issue.

## Understanding Node ID and Namespace 🔍

### **What is a Node ID?**

A Node ID uniquely identifies an item in the OPC UA server. It consists of:

- **NamespaceIndex** (`UINT`): Specifies which namespace the node belongs to.
- **Identifier** (`STRING` or `UINT`): Unique identifier of the node.
- **IdentifierType**: Specifies the type of identifier (`Numeric`, `String`, etc.).

### **What is a Namespace?**

Namespaces are used to distinguish between different vendors or components within an OPC UA server. Each namespace has an associated index that is retrieved using `UA_GetNamespaceIndex`.

## Implementation 🛠️

### **Variable Declaration 📝**

Declare an instance of `FB_OpcUaCli_Read` in your PLC program:

```structured-text
VAR
    FbOpcUaRead : FB_OpcUaCli_Read;
    ExecuteRead : BOOL;
    Variable1 : INT;
    Variable2 : INT;
    Variable3 : INT;
END_VAR
```

### **Configuring and Executing the Read Process ⚙️**

To read values from the OPC UA server, configure the function block as follows:

```structured-text
FbOpcUaRead.Execute         := ExecuteRead;
FbOpcUaRead.Reset           := gBP.ErrorReset;
FbOpcUaRead.ConnectionHdl   := FbOpcUaConnectToServer.ConnectionHdl;

FbOpcUaRead.IsActive[0] := TRUE;
FbOpcUaRead.NodeID_0[0].NamespaceIndex   := 2;
FbOpcUaRead.NodeID_0[0].Identifier       := 'Demo.Dynamic.Scalar.Int16';
FbOpcUaRead.NodeID_0[0].IdentifierType   := UAIdentifierType_String;
FbOpcUaRead.Variable[0]                  := '::UM_Logic:Variable1';

FbOpcUaRead.IsActive[1] := FALSE;
FbOpcUaRead.NodeID_0[1].NamespaceIndex   := 2;
FbOpcUaRead.NodeID_0[1].Identifier       := 'Demo.Dynamic.Scalar.Int16';
FbOpcUaRead.NodeID_0[1].IdentifierType   := UAIdentifierType_String;
FbOpcUaRead.Variable[1]                  := '::UM_Logic:Variable2';

FbOpcUaRead.IsActive[2]                  := TRUE;
FbOpcUaRead.NodeID_0[2].NamespaceIndex   := 2;
FbOpcUaRead.NodeID_0[2].Identifier       := 'Demo.Dynamic.Scalar.Int16';
FbOpcUaRead.NodeID_0[2].IdentifierType   := UAIdentifierType_String;
FbOpcUaRead.Variable[2]                  := '::UM_Logic:Variable3';

FbOpcUaRead();

IF FbOpcUaRead.Done THEN
  ExecuteRead := FALSE;
END_IF
```

### **Explanation 🔍**

- `Execute`: Triggers the read process when `TRUE`.
- `Reset`: Resets the function block in case of an error.
- `ConnectionHdl`: Provides the connection handle for OPC UA communication.
- `IsActive`: Determines which nodes should be read.
- `NodeID_0`: Defines the node IDs to read.
- `Variable`: Specifies the corresponding PLC variables to store the read values.

## Function Block `FB_OpcUaCli_Read Steps` 🔄

The function block `FB_OpcUaCli_Read` handles reading data from an OPC UA server in multiple steps:

 1. **Read Initialization** 🏁
 2. **Getting Node Handles** 🔗
 3. **Reading Data** ⏳
 4. **Releasing Node Handles** 🔄
 5. **Handling Errors** ❌

## Troubleshooting 🛠️

| Issue                | Possible Cause                | Solution                                       |
| -------------------- | ----------------------------- | ---------------------------------------------- |
| Read operation fails | Incorrect `NodeID_0` settings | Verify the node identifier and namespace index |
| No data received     | Server not responding         | Ensure the OPC UA server is running            |
| Unexpected values    | Data type mismatch            | Check variable data types                      |

## Conclusion 🎯

This guide provides step-by-step instructions on how to configure and use `FB_OpcUaCli_Read` to retrieve data from an OPC UA server. Ensure that your `NodeID_0` settings and namespace indexes are correctly configured for a successful implementation.

