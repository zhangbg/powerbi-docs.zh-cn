---
title: Power BI API 将自动保留策略用于实时数据
description: 了解 Power BI 服务的自动保留策略
author: KesemSharabi
ms.author: kesharab
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-developer
ms.topic: conceptual
ms.date: 06/08/2018
ms.openlocfilehash: cdbf50ee5078eaade7794242b3ed522e043cab22
ms.sourcegitcommit: c395fe83d63641e0fbd7c98e51bbab224805bbcc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2019
ms.locfileid: "74265851"
---
# <a name="automatic-retention-policy-for-real-time-data"></a>实时数据的自动保留策略

Power BI 服务中的自动保留策略是一个查询字符串参数，用于启用默认保留策略，以便在新数据不断进入仪表板时自动清除旧数据。 第一个保留策略称为先进先出 (FIFO)。  如果已启用，数据将收集在表中，直到它达到 200,000 行。 一旦数据超出 200,000 行，将从该数据集中删除最早的行。 这将使行数保持在 200,000 至 210,000 之间，仅包含最新数据。  
  
<center>

![保留策略](media/api-Automatic-retention-policy-for-real-time-data/retention-policy.png) 

</center>

首次创建数据集时将启用保留策略。 你只需将“default retention policy”查询参数添加到 POST 数据集调用并令其值等于“basicFIFO”即可。   
  
    POST https://api.powerbi.com/v1.0/myorg/datasets?defaultRetentionPolicy={None | basicFIFO}