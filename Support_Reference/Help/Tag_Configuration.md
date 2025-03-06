# OPC UA Node Identification Guide 🆔

## Overview 🚀

In OPC UA, each variable, task, and module is assigned a unique **Node ID**. This allows the OPC UA client to access the correct data on the server. The structure of these Node IDs depends on whether the variable is **local** (inside a task) or **global** (available across tasks).

## Addressing Syntax 📌

### **Global Variables** 🌍
Global variables are addressed with:
```structured-text
::VariableName
```
Example:
```structured-text
::MachineStatus
```

### **Local Variables** 🏠
Local variables (inside a task) are addressed with:
```structured-text
::TaskName:VariableName
```
Example:
```structured-text
::MainTask:Temperature
```

### **Application Modules and Tasks**
Application modules and tasks are structured as:
```structured-text
AppModuleName::
AppModuleName::TaskName
```
Examples:
```structured-text
FactoryModule::
FactoryModule::ConveyorTask
```

### **Arrays** 🔢
To access an array element, include the index:
```structured-text
::TaskName:ArrayName[Index]
```
Example:
```structured-text
::MainTask:SensorValues[2]
```
For multi-dimensional arrays:
```structured-text
::MainTask:Matrix[1,3]
```

### **Structures** 🏗️
To access a structure element, use a dot (`.`):
```structured-text
::TaskName:StructureName.Element
```
Example:
```structured-text
::MainTask:Motor.Speed
```
For structures inside arrays:
```structured-text
::MainTask:Motors[1].Speed
```
## Conclusion 🎯

Using the correct node address format ensures smooth communication with the OPC UA server. Always follow the correct syntax and enable variables as OPC UA tags before using them.

