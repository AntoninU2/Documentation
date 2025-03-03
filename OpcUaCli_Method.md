# OPC UA Methods Guide 🛠️📡

## Overview 🚀

This guide explains how to execute methods on an OPC UA server using `FbMethodProgramToUseLathingA1`. It details how to configure object nodes, method nodes, input and output arguments, and properly call a method.

## Prerequisites ✅

- An operational OPC UA server.
- A properly configured PLC with OPC UA capabilities.
- A valid connection to the OPC UA server.

## Setting Up Method Execution ⚙️

### Variable Declaration 📝

Before executing a method, declare the function block:

```structured-text
(* Method Execution Setup *)
VAR
    FbMethodProgramToUseLathingA1 : FB_OpcUaCli_Method;
END_VAR
```

📌 Declare `FbMethodProgramToUseLathingA1` to handle OPC UA method calls.

### Configuring the Method Call 🔧

To call a method, configure `FbMethodProgramToUseLathingA1` as follows:

```structured-text
(***************** Method*****************)
// ProgramToUse in Lathing A1
FbMethodProgramToUseLathingA1.Execute := ExecuteMethod1;
FbMethodProgramToUseLathingA1.Reset := gBP.ErrorReset;
FbMethodProgramToUseLathingA1.ConnectionHdl := FbOpcUaConnectLathingA1.ConnectionHdl;

FbMethodProgramToUseLathingA1.ObjectNodeID.NamespaceIndex := FbOpcUaConnectLathingA1.NamespaceIndex;
FbMethodProgramToUseLathingA1.ObjectNodeID.Identifier := 'Demo.Method';
FbMethodProgramToUseLathingA1.ObjectNodeID.IdentifierType := UAIdentifierType_String;

FbMethodProgramToUseLathingA1.MethodNodeID.NamespaceIndex := FbOpcUaConnectLathingA1.NamespaceIndex;
FbMethodProgramToUseLathingA1.MethodNodeID.Identifier := 'Demo.Method.Multiply';
FbMethodProgramToUseLathingA1.MethodNodeID.IdentifierType := UAIdentifierType_String;

FbMethodProgramToUseLathingA1.InputArguments[0].Name := 'a';
FbMethodProgramToUseLathingA1.InputArguments[0].Value := '::UM_Logic:Input1';
FbMethodProgramToUseLathingA1.InputArguments[1].Name := 'b';
FbMethodProgramToUseLathingA1.InputArguments[1].Value := '::UM_Logic:Input2';

FbMethodProgramToUseLathingA1.OutputArguments[0].Name := 'result';
FbMethodProgramToUseLathingA1.OutputArguments[0].Value := '::UM_Logic:Output';

FbMethodProgramToUseLathingA1();
```

📌 Ensure `Execute` is set to trigger the method call.
📌 Define `ObjectNodeID` and `MethodNodeID` to reference the OPC UA method.
📌 Set input arguments before executing the method.
📌 Store the method result in `OutputArguments`.
📌 The variables (`::UM_Logic:Input1`, `::UM_Logic:Input2`, `::UM_Logic:Output`) must be declared internally and match the expected data type of the OPC UA method.

## Understanding ObjectNodeID and MethodNodeID 🔍

### ObjectNodeID 🏷️

The `ObjectNodeID` represents the object that contains the method. In the example, it is set to `'Demo.Method'`. This means that the method belongs to an object node within the OPC UA server.

### MethodNodeID ⚙️

The `MethodNodeID` identifies the specific method to be called. In this case, it is `'Demo.Method.Multiply'`, which means that the method `Multiply` is part of the `Demo.Method` object.

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
