---
title:  The case-sensitive !endswith_cs string operator
description: Learn how to use the !endswith_cs string operator to filter data that doesn't contain a case-insensitive string. 
ms.reviewer: alexans
ms.topic: reference
ms.date: 08/11/2024
---
# !endswith_cs operator

> [!INCLUDE [applies](../includes/applies-to-version/applies.md)] [!INCLUDE [fabric](../includes/applies-to-version/fabric.md)] [!INCLUDE [azure-data-explorer](../includes/applies-to-version/azure-data-explorer.md)] [!INCLUDE [monitor](../includes/applies-to-version/monitor.md)] [!INCLUDE [sentinel](../includes/applies-to-version/sentinel.md)]

Filters a record set for data that doesn't contain a case-insensitive ending string.

[!INCLUDE [endswith-operator-comparison](../includes/endswith-operator-comparison.md)]

## Performance tips

[!INCLUDE [performance-tip-note](../includes/performance-tip-note.md)]

## Syntax

*T* `|` `where` *col* `!endswith_cs` `(`*expression*`)`  

[!INCLUDE [syntax-conventions-note](../includes/syntax-conventions-note.md)]

## Parameters

| Name | Type | Required | Description |
|--|--|--|--|
| *T* | `string` |  :heavy_check_mark:| The tabular input whose records are to be filtered. |
| *col* | `string` |  :heavy_check_mark: | The column to filter. |
| *expression* | `string` |  :heavy_check_mark: | The expression used to filter. |

## Returns

Rows in *T* for which the predicate is `true`.

## Example

:::moniker range="azure-data-explorer"
> [!div class="nextstepaction"]
> <a href="https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAwsuyS/KdS1LzSsp5qpRKC7NzU0syqxKVYAI2Sbnl+aVaGgqJFUqBJcklqQC1ZRnpBalQngKiql5KcXlmSUZ8cnFCkqOSgAvfsIqTgAAAA==" target="_blank">Run the query</a>
::: moniker-end

```kusto
StormEvents
| summarize Events=count() by State
| where State !endswith_cs "A"
```

The following table only shows the first 10 results. To see the full output, run the query.

|State| Events|
|--|--|
|TEXAS| 4701|
|KANSAS| 3166|
|ILLINOIS| 2022|
|MISSOURI| 2016|
|WISCONSIN| 1850|
|NEW YORK| 1750|
|COLORADO| 1654|
|MICHIGAN| 1637|
|KENTUCKY| 1391|
|OHIO| 1233|