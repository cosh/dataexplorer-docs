---
title:  Entity names
description:  This article describes Entity names.
ms.reviewer: orspodek
ms.topic: reference
ms.date: 08/11/2024
---
# Entity names

> [!INCLUDE [applies](../../includes/applies-to-version/applies.md)] [!INCLUDE [fabric](../../includes/applies-to-version/fabric.md)] [!INCLUDE [azure-data-explorer](../../includes/applies-to-version/azure-data-explorer.md)] [!INCLUDE [monitor](../../includes/applies-to-version/monitor.md)] [!INCLUDE [sentinel](../../includes/applies-to-version/sentinel.md)]

:::moniker range="azure-data-explorer"
Kusto entities are referenced in a query by name. Entities that can be referenced by their name include *databases*, *tables*, *columns*, and *stored functions*, but not *clusters*. The name you assign an entity is called an identifier. In addition to entities, you can also assign an identifier to [query parameters](../query-parameters-statement.md), or variables set through a [let statement](../let-statement.md).
::: moniker-end

:::moniker range="microsoft-fabric"
Kusto entities are referenced in a query by name. Entities that can be referenced by their name include *databases*, *tables*, *columns*, and *stored functions*. The name you assign an entity is called an identifier. In addition to entities, you can also assign an identifier to [query parameters](../query-parameters-statement.md), or variables set through a [let statement](../let-statement.md).
::: moniker-end

An entity's name is unique to the entity type in the context of its container. For example, two tables in the same database can't have the same name, but a database and a table can have the same name because they're different entity types. Similarly, a table and a stored function may have the same name.

> [!NOTE]
> Entity names, that are also reflected as an [Azure resource](/azure/templates/microsoft.kusto/allversions), follow the [ARM reserve words rules](/azure/azure-resource-manager/troubleshooting/error-reserved-resource-name).

## Pretty names

In addition to the entity's name, some entities may have a **pretty name**. Similar to the use of entity names, pretty names can be used to reference an entity in queries. But unlike entity names, pretty names aren't necessarily unique in the context of their container. When a container has multiple entities with the same pretty name, the pretty name can't be used to reference the entity.

Pretty names allow middle-tier applications to map automatically created entity names (such as UUIDs) to names that are human-readable for display and referencing purposes.

For an example on how to assign a pretty name, see [.alter database prettyname command](../../management/alter-database.md).

## Identifier naming rules

An identifier is the name you assign to [entities](index.md), [query parameters](../query-parameters-statement.md), or variable set through a [let statement](../let-statement.md). Valid identifiers must follow these rules:

* Identifiers are case-sensitive. Database names are case-insensitive, and therefore an exception to this rule.
* Identifiers must be between 1 and 1024 characters long.
* Identifiers may contain letters, digits, and underscores (`_`).
* Identifiers may contain certain special characters: spaces, dots (`.`), and dashes (`-`). For information on how to reference identifiers with special characters, see [Reference identifiers in queries](#reference-identifiers-in-queries).

> [!NOTE]
> * Only the specified UTF-8 characters of letters, digits, underscores, dots, and dashes are supported. For instance, semicolons and colons are not supported.
> * The `$` character is allowed in entity names when generated by a KQL operator.

### Avoid naming identifiers as language keywords or literals

In KQL, there are keywords and literals that have similar naming rules as [identifiers](#identifier-naming-rules). You can have identifiers with the same name as keywords or literals. However, we recommend that you avoid doing so as referencing them in queries requires [special quoting](#reference-identifiers-in-queries).

To avoid using an identifier that might also be a language keyword or literal, such as `where`, `summarize`, and `1day`, you can choose your entity name according to the following conventions, which aren't applicable to language keywords:

* Use a name that starts with a capital letter (`A` to `Z`).
* Use a name that starts or ends with a single underscore (`_`).

    > [!NOTE]
    > KQL reserves all identifiers that start or end with a sequence of two underscore characters (`__`); users can't define such names for their own use.

For information on how to reference these identifiers, see [Reference identifiers in queries](#reference-identifiers-in-queries).

### Reference identifiers in queries

The following table provides an explanation on how to reference identifiers in queries.

|Identifier type|Identifier   |Reference         |Explanation  |
|--------|-------|------------------|-------------|
|Normal  | `entity` |`entity`    |Identifiers (`entity`) that don't include special characters or map to some language keyword don't need to be enclosed in quotation marks.|
|Special character| `entity-name`|`['entity-name']` |Identifiers names that include special characters (such as `-`) must be enclosed using `['` and `']` or using `["` and `"]`.|
|language keyword| `where`       |`["where"]`       |Identifier names that are language keywords must be enclosed using `['` and `']` or `["` and `"]`.|
|literal  | `1day` | `["1day"]`    | Identifier names that are literals must be enclosed using `['` and `']` or `["` and `"]`.|

> [!NOTE]
> Identifiers are case-sensitive. For example, you can't refer to a table called `ThisTable` as `thisTABLE`.

## Related content

* [Entity types](index.md).
* [Syntax conventions for reference documentation](../syntax-conventions.md).