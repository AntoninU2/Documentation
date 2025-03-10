# OPC UA Methods Guide as client 🛠️📡

## Overview 🚀

This guide explains how to execute methods on an OPC UA server using `FbMethodMultiply`. It details how to configure object nodes, method nodes, input and output arguments, and properly call a method.

## Prerequisites ✅

- An operational OPC UA server.
- A properly configured PLC with OPC UA capabilities.
- A valid connection to the OPC UA server.

## Function Block Structure

### `FB_OpcUaCli_Method`

This function block is responsible for handling OPC UA client method calls.

#### Input Variables:
- **Execute**: Triggers the execution of the method call.
- **Reset**: Resets the execution state.
- **ConnectionHdl**: Handle of the active OPC UA connection.
- **ObjectNodeID**: Node ID of the object containing the method.
- **MethodNodeID**: Node ID of the method to be called.
- **InputArguments**: Array of up to 10 input arguments required by the method.
- **OutputArguments**: Array of up to 10 output arguments to store the method results.

#### Output Variables:
- **Error**: Indicates if an error occurred during execution.
- **ErrorID**: Error identifier providing details on the issue.
- **Done**: Indicates if the method call was successfully completed.

## Setting Up Method Execution ⚙️

### Variable Declaration 📝

Before executing a method, declare the function block:

```structured-text
(* Method Execution Setup *)
VAR
    FbMethodMultiply : FB_OpcUaCli_Method;
END_VAR
```
```structured-text

(* Method variables Setup *)
VAR
    Input1 : REAL;
    Input2 : REAL;
    Output : REAL;
END_VAR
```
### Configuring the Method Call 🔧

To call a method, configure `FbMethodMultiply` as follows:

```structured-text
(***************** Method *****************)
// Multiply
FbMethodMultiply.Execute := ExecuteMethod1;
FbMethodMultiply.Reset := gBP.ErrorReset;
FbMethodMultiply.ConnectionHdl := FbOpcUaConnectToServer.ConnectionHdl;

FbMethodMultiply.ObjectNodeID.NamespaceIndex    := FbOpcUaConnectToServer.NamespaceIndex;
FbMethodMultiply.ObjectNodeID.Identifier        := 'Demo.Method';
FbMethodMultiply.ObjectNodeID.IdentifierType    := UAIdentifierType_String;

FbMethodMultiply.MethodNodeID.NamespaceIndex    := FbOpcUaConnectToServer.NamespaceIndex;
FbMethodMultiply.MethodNodeID.Identifier        := 'Demo.Method.Multiply';
FbMethodMultiply.MethodNodeID.IdentifierType    := UAIdentifierType_String;

FbMethodMultiply.InputArguments[0].Name         := 'a';
FbMethodMultiply.InputArguments[0].Value        := '::UM_Logic:Input1';
FbMethodMultiply.InputArguments[1].Name         := 'b';
FbMethodMultiply.InputArguments[1].Value        := '::UM_Logic:Input2';

FbMethodMultiply.OutputArguments[0].Name        := 'result';
FbMethodMultiply.OutputArguments[0].Value       := '::UM_Logic:Output';

FbMethodMultiply();

IF FbMethodMultiply.Done THEN
    ExecuteMethod1 := FALSE;
END_IF
```

### Explanation 🔍

- `Execute`: Triggers the method call.
- `ObjectNodeID`: References the object containing the method.
- `MethodNodeID`: Specifies the method to be executed.
- `InputArguments`: Defines the input parameters for the method.
- `OutputArguments`: Stores the result of the method execution.

## Understanding ObjectNodeID and MethodNodeID 🔍

### ObjectNodeID 🏷️

The `ObjectNodeID` represents the object that contains the method. In the example, it is set to `'Demo.Method'`, meaning the method belongs to an object node within the OPC UA server.

### MethodNodeID ⚙️

The `MethodNodeID` identifies the specific method to be called. In this case, it is `'Demo.Method.Multiply'`, meaning the method `Multiply` is part of the `Demo.Method` object.

Both `ObjectNodeID` and `MethodNodeID` must be correctly referenced for the method call to succeed.

## Understanding Namespace and Identifier Types 📌

### Namespace Index 📡

The `NamespaceIndex` helps locate a node within an OPC UA server. Since namespace indices can vary between servers, they should be retrieved dynamically.

### Identifier Types 🔢

- **`UAIdentifierType_String`**: Uses a string identifier, such as `'Demo.Method.Multiply'`. This is human-readable and commonly used.
- **`UAIdentifierType_Numeric`**: Uses a numerical identifier, which is more efficient and often used in industrial setups where nodes are pre-defined.

Choosing the correct identifier type ensures proper method execution and access to the right OPC UA nodes.

## Steps to Call an OPC UA Method 🛠️

1. **Retrieve the namespace index** 📡 (Using `UA_GetNamespaceIndex`)
2. **Get the method handle** 🏷️
3. **Execute the method** ✅
4. **Retrieve the result** 📤
5. **Release the method handle** 🔄

## Troubleshooting 🛠️

| Issue                     | Possible Cause            | Solution                        |
|---------------------------|--------------------------|--------------------------------|
| Method execution fails    | Incorrect `MethodNodeID`  | Verify the method identifier  |
| No result returned        | Wrong `OutputArguments`   | Ensure the method produces output |
| Connection issues         | Invalid `ConnectionHdl`  | Check if the OPC UA server is reachable |

## Conclusion 🎯✅

This guide explains how to execute methods on an OPC UA server, configure method nodes, define arguments, and troubleshoot potential issues. Always verify the namespace index, identifier types, and variable data types before execution to ensure a successful method call.

