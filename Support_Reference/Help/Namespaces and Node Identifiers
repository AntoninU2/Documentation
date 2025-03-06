# Understanding OPC UA Namespaces and Node Identifiers 🔍

## Overview 🚀

This guide provides an in-depth explanation of OPC UA namespaces, namespace indexes, and node identifiers, which are essential for interacting with OPC UA servers.

## What is a Namespace? 🏷️

A **namespace** in OPC UA is a mechanism used to distinguish node identifiers from different vendors, applications, or components within an OPC UA server. 

Each namespace is identified by a **Namespace URI (Uniform Resource Identifier)**, which ensures uniqueness. Since URIs can be long, OPC UA assigns a **Namespace Index (NamespaceIndex)** to each URI, which is a shorter numerical reference used within the server.

### Finding the Namespace URI in UaExpert 🖼️

To find the **Namespace URI** in **UaExpert**:
1. Connect to your OPC UA server.
2. Open the **Address Space** tab.
3. Select a node and check its **Namespace URI** in the properties pane.

![UaExpert showing Namespace URIs](C:\Users\aesseul\Pictures\NameSpaceURI.PNG "UaExpert NameSpaceURI")

## Namespace Index (NamespaceIndex) 🔢

The **Namespace Index** is a server-assigned numeric value that maps to a specific Namespace URI. 

- Namespace indexes can change between server restarts.
- Namespace indexes are used in **Node IDs** instead of full URIs for efficiency.
- The namespace index can be retrieved dynamically using `UA_GetNamespaceIndex`.

### Example of Namespace Index Mapping:

| Namespace URI                                | Namespace Index |
|----------------------------------------------|----------------|
| `http://opcfoundation.org/UA/`              | 0              |
| `http://mycompany.com/MyCustomNamespace`    | 2              |
| `http://thirdparty.com/ExternalNamespace`   | 3              |

## Understanding Node IDs 🆔

A **Node ID** uniquely identifies an item in the OPC UA server. A Node ID consists of:

- **NamespaceIndex** (`UINT`): Specifies which namespace the node belongs to.
- **Identifier** (`STRING` or `UINT`): The unique identifier of the node within the namespace.
- **IdentifierType**: Defines the format of the identifier (e.g., `Numeric`, `String`).

### Finding Namespace Index, Identifier, and Identifier Type in UaExpert 🖼️

To find the **Namespace Index**, **Identifier**, and **Identifier Type** in **UaExpert**:
1. Connect to your OPC UA server.
2. Open the **Address Space** tab.
3. Select a node and check its **Namespace Index**, **Identifier**, and **Identifier Type** in the properties pane.

!UaExpert showing Namespace Index, Identifier, and Identifier](C:\Users\aesseul\Pictures\NameSpace.PNG "UaExpert NameSpaceURI")

### Types of Node Identifiers:

1. **Numeric Identifier**
   - Example: `NodeID = (NamespaceIndex=2, Identifier=1001, IdentifierType=Numeric)`
2. **String Identifier**
   - Example: `NodeID = (NamespaceIndex=2, Identifier='MyVariable', IdentifierType=String)`

## Best Practices ✅

- **Avoid hardcoding namespace indexes** since they may change between server restarts.
- **Use `UA_GetNamespaceIndex` dynamically** to resolve Namespace URIs to the correct indexes.
- **Ensure correct IdentifierType** when specifying Node IDs to avoid mismatches.
- **Keep track of Namespace URIs** to avoid conflicts between vendors and custom implementations.

## Conclusion 🎯

Understanding OPC UA namespaces and Node IDs is essential for reliable communication with an OPC UA server. By dynamically resolving namespace indexes and correctly configuring node identifiers, you can ensure a robust and adaptable OPC UA client implementation.
