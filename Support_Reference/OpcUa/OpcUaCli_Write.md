# OPC UA Write Function Block Guide 📚🔧

## Overview 🚀

This guide provides detailed instructions on how to use the `FB_OpcUaCli_Write` function block to write data from local PLC variables to an OPC UA server.

## Prerequisites ✅

- A functional OPC UA server.
- A PLC with OPC UA client capabilities.
- Established OPC UA connection using `FB_OpcUaCli_Connect`.

## Function Block Structure 🏧

### `FB_OpcUaCli_Write`

This function block writes values from specified local PLC variables to OPC UA nodes.

#### Input Variables:

- **Execute**: Set to `TRUE` to initiate the write process.
- **Reset**: Resets the function block in case of errors.
- **ConnectionHdl**: The active OPC UA connection handle.
- **IsActive**: Defines which variables should be written to the OPC UA server.
- **NodeID_0**: An array of Node IDs corresponding to the target OPC UA nodes.
  - **NamespaceIndex**: Specifies the namespace index of the node.
  - **Identifier**: The unique identifier of the target node.
  - **IdentifierType**: Specifies the type of identifier (`String`, `Numeric`, etc.).
- **Variable**: An array of PLC variables mapped to OPC UA node values.
- **NodeAddInfo_0**: Additional node information if required.

#### Output Variables:

- **Done**: `TRUE` if the writing process is successfully completed.
- **Error**: `TRUE` if an error occurs during the writing process.
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

Declare an instance of `FB_OpcUaCli_Write` in your PLC program:

```structured-text
VAR
    FbOpcUaWrite : FB_OpcUaCli_Write;
    Variable1 : INT;
    Variable2 : INT;
    Variable3 : INT;
END_VAR
```

### **Configuring and Executing the Write Process ⚙️**

To write values to the OPC UA server, configure the function block as follows:

```structured-text
FbOpcUaWrite.Execute         := TRUE;
FbOpcUaWrite.Reset           := gBP.ErrorReset;
FbOpcUaWrite.ConnectionHdl   := FbOpcUaConnectLathingA1.ConnectionHdl;

FbOpcUaWrite.IsActive[0] := TRUE;
FbOpcUaWrite.NodeID_0[0].NamespaceIndex   := 2;
FbOpcUaWrite.NodeID_0[0].Identifier       := 'Demo.Static.Scalar.Int16';
FbOpcUaWrite.NodeID_0[0].IdentifierType   := UAIdentifierType_String;
FbOpcUaWrite.Variable[0]                  := '::UM_Logic:Variable1';

FbOpcUaWrite.IsActive[1] := FALSE;
FbOpcUaWrite.NodeID_0[1].NamespaceIndex   := 2;
FbOpcUaWrite.NodeID_0[1].Identifier       := 'Demo.Static.Scalar.Int16';
FbOpcUaWrite.NodeID_0[1].IdentifierType   := UAIdentifierType_String;
FbOpcUaWrite.Variable[1]                  := '::UM_Logic:Variable2';

FbOpcUaWrite.IsActive[2]                  := TRUE;
FbOpcUaWrite.NodeID_0[2].NamespaceIndex   := 2;
FbOpcUaWrite.NodeID_0[2].Identifier       := 'Demo.Static.Scalar.Int16';
FbOpcUaWrite.NodeID_0[2].IdentifierType   := UAIdentifierType_String;
FbOpcUaWrite.Variable[2]                  := '::UM_Logic:Variable3';

FbOpcUaWrite();
```

### **Explanation 🔍**

- `Execute`: Triggers the write process when `TRUE`.
- `Reset`: Resets the function block in case of an error.
- `ConnectionHdl`: Provides the connection handle for OPC UA communication.
- `IsActive`: Determines which nodes should be written to.
- `NodeID_0`: Defines the node IDs to write.
- `Variable`: Specifies the corresponding PLC variables storing the values.

## Function Block `FB_OpcUaCli_Write Steps` 🔄

The function block `FB_OpcUaCli_Write` handles writing data to an OPC UA server in multiple steps:

1. **Write Initialization** 🌟
2. **Getting Node Handles** 🔗
3. **Writing Data** ⏳
4. **Releasing Node Handles** 🔄
5. **Handling Errors** ❌

## Troubleshooting 🛠️

| Issue                | Possible Cause                | Solution                                       |
| -------------------- | ----------------------------- | ---------------------------------------------- |
| Write operation fails | Incorrect `NodeID_0` settings | Verify the node identifier and namespace index |
| No data written     | Server not responding         | Ensure the OPC UA server is running            |
| Unexpected values    | Data type mismatch            | Check variable data types                      |

## Conclusion 🎯

This guide provides step-by-step instructions on how to configure and use `FB_OpcUaCli_Write` to send data from the PLC to an OPC UA server. Ensure that your `NodeID_0` settings and namespace indexes are correctly configured for a successful implementation.
