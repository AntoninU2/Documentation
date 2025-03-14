# Gestion de l'OPC UA dans le Projet

## Introduction

Ce document décrit la gestion de l'OPC UA dans le projet utilisant une machine industrielle B&R. Il explique comment les variables sont interfacées entre l'OPC UA, le programme et le processus industriel.

## Architecture de l'Interface OPC UA

L'interface OPC UA repose sur une structure organisée en plusieurs niveaux :

1. **Méthodes OPC UA** : Ces méthodes permettent l'échange d'informations entre l'OPC UA et l'interface du programme.
2. **Interface** : Cette couche intermédiaire gère les variables mises à disposition par les méthodes OPC UA.
3. **Processus** : Il exploite les variables fournies par l'interface.

## Gestion des Variables

Les variables qui communiquent directement avec l'OPC UA sont encapsulées dans des structures spécifiques :

- **`udt_OpcUa_Cell`**
- **`udt_OpcUa_Ltu`**

Les variables qui communiquent directement avec le process sont encapsulées dans une structure spécifique :

- **`udt_OpcUa_Interface`**

Ces structures contiennent des sous-structures et servent de passerelle entre l'OPC UA et l'interface du programme.

### Liaison des Variables

Chaque variable de l'interface suit une règle stricte de passage par une variable OPC UA avant d'être utilisée. Exemple de liaison entre une variable de production et l'OPC UA :

```structured-text
OpcUaInterface.Methods.HoldProduction.Data.WorkOrderNumber		:= OpcUaCell.Methods.HoldProduction.WorkOrderNumber;
```

Dans cet exemple :

- `OpcUaCell.Methods.HoldProduction.WorkOrderNumber` provient d'une méthode OPC UA.
- `OpcUaInterface.Methods.HoldProduction.Data.WorkOrderNumber` sert d'interface pour l'OPC UA.
- Le process accède à cette valeur via `udt_OpcUaInterface`.

## Schéma de Communication

Le schéma ci-dessous illustre le fonctionnement général :

- Les **méthodes OPC UA** alimentent l'interface avec des données.
- L'**interface** gère les échanges et met les variables à disposition.
- Le **processus** utilise ces variables pour exécuter des opérations industrielles.

*(Voir le diagramme fourni pour plus de détails.)*
