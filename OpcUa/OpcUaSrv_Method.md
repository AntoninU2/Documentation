# Declaring a Method on the OPC UA Server 📡🛠️

## Overview 🚀

This guide explains how to declare a method on an OPC UA server so that it can be invoked by an OPC UA client. The method `StartProduction` will be exposed on the server, allowing clients to pass input parameters and receive a response.

## Prerequisites ✅

- Automation Studio (AS) V4.3.2 or later.
- An operational OPC UA server.
- A properly configured PLC with OPC UA capabilities.

## Declaring the Method on the Server 🔧

### Variable Declaration 📝

Before exposing a method, declare the function block:

```structured-text
VAR
    FbMethodName : FB_OpcUaSrv_Method;
END_VAR
```

### Declaring the Corresponding Structure 🏗️

Define the structure that corresponds to the method:

```structured-text
udt_MyMethodName : STRUCT  
    WorkOrderNumber : STRING[255]; (* WorkOrder number to be resumed *)
    Area : STRING[255]; (* "Entire cell", "Side A" or "Side B" *)
    Status : BOOL;
END_STRUCT;
```

### Configuring the Method ⚙️

Define the method `FbMethodName` on the OPC UA server:

```structured-text
// MyMethodName Method
FbMethodName.Execute := ServerState = UASS_Running;
FbMethodName.Reset := gBP.ErrorReset;
FbMethodName.MethodeName := 'MyMethodName';
IF FbMethodName.IsCalled THEN
    OpcUaInterface.ErrorInterfaceID := NoError;
    OpcUaCell.Methods.MyMethodName.Status := TRUE; //Adding status to return with the method if the execution is true or false

    //Replace the code by yours
    IF '' <> OpcUaCell.Methods.MyMethodName.WorkOrderNumber THEN
        OpcUaInterface.Methods.MyMethodName.WorkOrderRequest := TRUE;
        OpcUaInterface.Methods.MyMethodName.Data.WorkOrderNumber := OpcUaCell.Methods.MyMethodName.WorkOrderNumber;
        MethodStartProductionDone := TRUE;
    ELSIF 'Entire cell' = OpcUaCell.Methods.MyMethodName.Area THEN
        OpcUaInterface.Methods.MyMethodName.EntireCellRequest := TRUE;        
    ELSIF 'Side A' = OpcUaCell.Methods.MyMethodName.Area THEN
        OpcUaInterface.Methods.MyMethodName.SideARequest := TRUE;    
    ELSIF 'Side B' = OpcUaCell.Methods.MyMethodName.Area THEN
        OpcUaInterface.Methods.MyMethodName.SideBRequest := TRUE;
    ELSE
        OpcUaInterface.ErrorInterfaceID := MyMethodName;
        OpcUaCell.Methods.MyMethodName.Status := FALSE;
    END_IF        
END_IF

FbMethodName();
```

📌 The logic inside the `IF` block should be replaced with the specific actions required for your application.
📌 If a valid `WorkOrderNumber` is provided, a production request is triggered.
📌 If no `WorkOrderNumber` is provided, the production area is checked.
📌 The method sets an error flag if neither condition is met.

### Declaring the Method in AS 🖥️

To declare an OPC UA method in Automation Studio:

1. Open **Logical View** and navigate to the task where the method will be assigned.
2. Use the **Object Catalog** to add an OPC UA method to the task.
3. Open the **.uam declaration file** and add the method using **Add OPC UA Method**.
4. Rename the method as needed.

### Example `.uam` Method Declaration 📄

```structured-text
OPCUA_METHOD StartProduction
    ARG_INPUT
        WorkOrderNumber : String := DataStartProduction.WorkOrderNumber;
        Area : String := DataStartProduction.Area;
    END_ARG
END_OPCUA_METHOD
```

📌 The `.uam` file registers `StartProduction` as an OPC UA method.
📌 The method takes two input arguments: `WorkOrderNumber` and `Area`.
📌 The values of these arguments are linked to process variables on the server.

## Executing the Method at Runtime 🔄

To execute a method on the B&R OPC UA server, use the function block `UAsrv_MethodOperate`.

📌 The method is identified only by its **name**.
📌 If no method is declared with the specified name, an error is returned.
📌 Method arguments must be linked to process variables.

## Calling the Method from an OPC UA Client 🔌

To invoke the method using **UaExpert** or another OPC UA client:

1. Ensure **read and execute rights** are assigned to the method.
2. Connect to the OPC UA server.
3. Locate `MyMethodName` in the **Methods** section.
4. Enter values for `WorkOrderNumber` and `Area`.
5. Execute the method and observe the results.

## Additional Resources 📚

Refer to the **B&R Automation Studio Help** for more details on:
- Declaring OPC UA methods in `.uam` files.
- Assigning process variables to OPC UA methods.
- Configuring execution rights for OPC UA methods.

## Conclusion 🎯✅

This guide provides a step-by-step approach to declaring and configuring an OPC UA method on the server. Proper configuration ensures that OPC UA clients can successfully invoke the method and receive responses.

