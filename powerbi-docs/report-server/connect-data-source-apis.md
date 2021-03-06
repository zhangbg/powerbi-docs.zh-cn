---
title: 使用 PowerShell 更改数据源连接字符串
description: 使用 PowerShell 中的 API 更改数据源连接字符串 - Power BI 报表服务器。
author: maggiesMSFT
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-report-server
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: maggies
ms.openlocfilehash: 77716514ffbb6dc8d3f128ada85276b46bf7af05
ms.sourcegitcommit: 17f45a81b0dcbf9e3f1fb2a551584170baecd320
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923656"
---
# <a name="change-data-source-connection-strings-in-power-bi-reports-with-powershell---power-bi-report-server"></a>使用 PowerShell 更改 Power BI 报表中的数据源连接字符串 - Power BI 报表服务器

可以使用 PowerShell 中的 API，在 Power BI 报表服务器中更改 Power BI 报表中的数据源连接字符串。 

1. 安装 Power BI 报表服务器 PowerShell commandlet。 在 [https://github.com/Microsoft/ReportingServicesTools](https://github.com/Microsoft/ReportingServicesTools) 中查找 commandlet 和安装说明。 

2. 通过 PowerShell commandlet 提取 Power BI 文件的现有数据源信息：

    ```powershell
    Get-RsRestItemDataSource -RsItem '/MyPbixReport'
    ```

    查看 Power BI 报表中包含的第一个数据源的信息： 

    ```powershell
    $dataSources[0]
    ```

3. 根据需要更新连接和凭据信息。 如果更新连接字符串和数据源使用了存储的凭据，则需要提供帐户密码。 

    更新数据源连接字符串：

    ```powershell
    $dataSources[0].ConnectionString = 'data source=myCatalogServer;initial catalog=ReportServer;persist security info=False' 
    ```

    更改数据源凭据类型：

    ```powershell
    $dataSources[0].DataModelDataSource.AuthType = 'Integrated'
    ```

    更改数据源用户名/密码：

    ```powershell
    $dataSources[0].DataModelDataSource.Username = 'domain\user
    ```
    ```powershell
    $dataSources[0].DataModelDataSource.Secret = 'password'
    ```

4. 将更新后的凭据保存回服务器。

    ```powershell
    Set-RsRestItemDataSource -RsItem '/MyPbixReport' -RsItemType 'PowerBIReport' -DataSources $dataSources
    ```

## <a name="next-steps"></a>后续步骤

[Power BI 报表服务器中的分页报表数据源](connect-data-sources.md) 

更多问题？ [尝试咨询 Power BI 社区](https://community.powerbi.com/)

