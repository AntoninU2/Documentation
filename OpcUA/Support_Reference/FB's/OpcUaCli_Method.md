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
- **NodeID**: Node ID corresponding to the method.
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
    ExecuteMethod1 : BOOL;
END_VAR
```
```structured-text

(* Method variables Setup *)
VAR
    Input1 : LREAL;
    Input2 : LREAL;
    Output : LREAL;
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

FbMethodMultiply.NodeID	:= 'ns=2;s=Demo.Method.Multiply';

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
- `NodeID`: Specifies the node of the method.
- `InputArguments`: Defines the input parameters for the method.
- `OutputArguments`: Stores the result of the method execution.

## Steps to Call an OPC UA Method 🛠️

1. **Get the method handle** 🏷️
2. **Execute the method** ✅
3. **Retrieve the result** 📤
4. **Release the method handle** 🔄

## Troubleshooting 🛠️

| Issue                     | Possible Cause            | Solution                        |
|---------------------------|--------------------------|--------------------------------|
| Method execution fails    | Incorrect `NodeID`  | Verify the method identifier  |
| No result returned        | Wrong `OutputArguments`   | Ensure the method produces output |
| Connection issues         | Invalid `ConnectionHdl`  | Check if the OPC UA server is reachable |

## Conclusion 🎯✅

This guide explains how to execute methods on an OPC UA server, configure method nodes, define arguments, and troubleshoot potential issues. Always verify the NodeID, and variable data types before execution to ensure a successful method call.

