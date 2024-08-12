---
title:  rank_tdigest()
description: Learn how to use the rank_tdigest() function to calculate the approximate rank of the value in a set.
ms.reviewer: alexans
ms.topic: reference
ms.date: 08/11/2024
---
# rank_tdigest()

> [!INCLUDE [applies](../includes/applies-to-version/applies.md)] [!INCLUDE [fabric](../includes/applies-to-version/fabric.md)] [!INCLUDE [azure-data-explorer](../includes/applies-to-version/azure-data-explorer.md)] [!INCLUDE [monitor](../includes/applies-to-version/monitor.md)] [!INCLUDE [sentinel](../includes/applies-to-version/sentinel.md)]

Calculates the approximate rank of the value in a set.
Rank of value `v` in a set `S` is defined as count of members of `S` that are smaller or equal to `v`, `S` is represented by its `tdigest`.

## Syntax

`rank_tdigest(`*digest*`,` *value*`)`

[!INCLUDE [syntax-conventions-note](../includes/syntax-conventions-note.md)]

## Parameters

|Name|Type|Required|Description|
|--|--|--|--|
|*digest*| `string` |An expression that was generated by [tdigest()](tdigest-aggregation-function.md) or [tdigest_merge()](tdigest-merge-aggregation-function.md).|
|*value*|scalar|An expression representing a value to be used for ranking calculation.|

## Returns

The rank foreach value in a dataset.

>[!TIP]
>The values that you want to get its rank must be of the same type as the `tdigest`.

## Examples

In a sorted list (1-1000), the rank of 685 is its index:

:::moniker range="azure-data-explorer"
> [!div class="nextstepaction"]
> <a href="https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAytKzEtPVahQSCvKz1UwVCjJVzA0MDBQKC5JLVAw5KpRKC7NzU0syqxKVSiJr7AtSclMTy0u0ajQBEoVFOVnpSaXKBQl5mXH56fFm1mY2oLZMFVAHToKQFFNANdlA5xlAAAA" target="_blank">Run the query</a>
::: moniker-end

```kusto
range x from 1 to 1000 step 1
| summarize t_x=tdigest(x)
| project rank_of_685=rank_tdigest(t_x, 685)
```

**Output**

|`rank_of_685`|
|-------------|
|`685`        |

This query calculates the rank of value 4490$ over all damage properties costs:

:::moniker range="azure-data-explorer"
> [!div class="nextstepaction"]
> <a href="https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAwsuyS/KdS1LzSsp5qpRKC7NzU0syqxKVShJyUxPLS4JSi1WsIVxNFwScxPTUwOK8gtSi0oqNYEaCorys1KTSxSKEvOy4/PT4k1MLA1swRyYHoRBOgogWU0A4SQmJHIAAAA=" target="_blank">Run the query</a>
::: moniker-end

```kusto
StormEvents
| summarize tdigestRes = tdigest(DamageProperty)
| project rank_of_4490=rank_tdigest(tdigestRes, 4490) 
```

**Output**

|`rank_of_4490`|
|--------------|
|`50207`       |

Getting the estimated percentage of the rank (by dividing by the set size):

:::moniker range="azure-data-explorer"
> [!div class="nextstepaction"]
> <a href="https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAwsuyS/KdS1LzSsp5qpRKC7NzU0syqxKVShJyUxPLS4JSi1WsIVxNFwScxPTUwOK8gtSi0oqNXUUkvNL80o0NIE6C4rys1KTSxSKEvOy42HqEYboKJiYWBpoKmgpGBoY6Bko6EO0xgMAg34oyX8AAAA=" target="_blank">Run the query</a>
::: moniker-end

```kusto
StormEvents
| summarize tdigestRes = tdigest(DamageProperty), count()
| project rank_tdigest(tdigestRes, 4490) * 100.0 / count_
```

**Output**

|`Column1`         |
|------------------|
|`85.0015237192293`|

The percentile 85 of the damage properties costs is 4490$:

:::moniker range="azure-data-explorer"
> [!div class="nextstepaction"]
> <a href="https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAwsuyS/KdS1LzSsp5qpRKC7NzU0syqxKVShJyUxPLS4JSi1WsIVxNFwScxPTUwOK8gtSi0oqNYEaCorys1KTSxSAAslAMzJzUuNhihEm6ChYmOoolFQWpOanaeTk56VragIAa7SLZXcAAAA=" target="_blank">Run the query</a>
::: moniker-end

```kusto
StormEvents
| summarize tdigestRes = tdigest(DamageProperty)
| project percentile_tdigest(tdigestRes, 85, typeof(long))
```

**Output**

|`percentile_tdigest_tdigestRes`|
|-------------------------------|
|`4490`                         |