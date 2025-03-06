# OPC UA Data Types Guide 🗃️

## Overview 🚀

In OPC UA, data types define the format of values exchanged between an OPC UA server and client. The data type of a selected node on the OPC UA client (e.g., an Automation Studio program) must match the node data type on the OPC UA server (bus controller). Otherwise, an error message with the corresponding error number will be displayed during read or write access.

This guide provides an overview of supported OPC UA data types and their corresponding Automation Studio equivalents.

## Data Type Compatibility ⚠️

Not all OPC UA devices support every data type. Some devices, such as `OpcUa_any`, only support a subset of available data types.

## Common Data Types and Mappings 🔄

| OPC UA Data Type       | Value | Automation Studio Type | Description                         | Supported in OpcUa_any? |
|------------------------|-------|------------------------|-------------------------------------|-------------------------|
| **Boolean**           | 1     | BOOL                   | Boolean data type                  | ✅ Yes                  |
| **SByte**             | 2     | SINT                   | 1-byte signed integer              | ✅ Yes                  |
| **Byte**              | 3     | USINT                  | 1-byte unsigned integer            | ✅ Yes                  |
| **Int16**             | 4     | INT                    | 2-byte signed integer              | ✅ Yes                  |
| **UInt16**            | 5     | UINT                   | 2-byte unsigned integer            | ✅ Yes                  |
| **Int32**             | 6     | DINT                   | 4-byte signed integer              | ✅ Yes                  |
| **UInt32**            | 7     | UDINT                  | 4-byte unsigned integer            | ✅ Yes                  |
| **Float**             | 10    | REAL                   | Floating-point number (4 bytes)    | ✅ Yes                  |
| **Double**            | 11    | LREAL                  | Double precision floating-point    | ❌ No                   |
| **String**            | 12    | STRING[255]            | String                             | ❌ No                   |
| **DateTime**          | 13    | DATE_AND_TIME          | Time and date                      | ❌ No                   |
| **ByteString**        | 15    | UAByteString           | Data structure for binary data     | ❌ No                   |
| **NodeID**            | 17    | UANodeID               | Identification of the object       | ❌ No                   |
| **ExpandedNodeID**    | 18    | UAExpandedNodeID       | Structure for expanded NodeID      | ❌ No                   |
| **QualifiedName**     | 20    | UAQualifiedName        | Structure for QualifiedName        | ❌ No                   |
| **LocalizedText**     | 21    | UALocalizedText        | Structure for LocalizedText        | ❌ No                   |
| **DataValue**         | 23    | UADataValue            | Structure for DataValue            | ❌ No                   |
| **ExtensionObject**   | 296   | UAMethodArgument       | Input and output arguments         | ❌ No                   |

## Special Data Buffers 📦

### ByteString → UAByteString (Automation Studio)

| Name   | Data Type   | Description                           |
|--------|------------|---------------------------------------|
| Length | DINT       | Current length of data               |
| Data   | USINT[0..MaxIndex_Bytestring] | Data buffer (default max 1024 bytes) |

### ExtensionObject [Argument] → UAMethodArgument (Automation Studio)

| Name  | Data Type  | Description                          |
|-------|-----------|--------------------------------------|
| Name  | STRING[64] | Name of the method argument         |
| Value | STRING[255] | Value of the method argument       |

## Conclusion 🎯

Matching OPC UA data types correctly is essential for successful communication between an OPC UA client and server. Always ensure that the data type configuration aligns between the Automation Studio program and the OPC UA server to avoid errors during read and write operations.

