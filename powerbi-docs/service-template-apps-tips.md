---
title: 在 Power BI 中创作模板应用的提示
description: 有关创建查询、数据模型、报表和仪表板以制作优秀模板应用的提示
author: teddybercovitz
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-service
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: tebercov
ms.openlocfilehash: 04b50882c28bf561e628e9f02dff6c147233d260
ms.sourcegitcommit: 08b73af260ded51daaa6749338cb85db2eab587f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2019
ms.locfileid: "74099749"
---
# <a name="tips-for-authoring-template-apps-in-power-bi"></a>在 Power BI 中创作模板应用的提示

在 Power BI 中[创作模板应用](service-template-apps-create.md)时，其中一部分是创建工作区、测试和生产的后勤工作。 但其他重要的部分显然是创作报表和仪表板。 我们可以将创作过程细分为四个主要部分。 使用这些组件有助于创建最佳模板应用：

* 通过“查询”，[连接](desktop-connect-to-data.md)和[转换](desktop-query-overview.md)数据，并定义[参数](https://powerbi.microsoft.com/blog/deep-dive-into-query-parameters-and-power-bi-templates/)  。 
* 在“数据模型”中，可以创建[关系](desktop-create-and-manage-relationships.md)[度量值](desktop-measures.md)和问答的改进  。  
* [报表页](desktop-report-view.md)中包括视觉对象和筛选器，以帮助洞察数据  。  
* [仪表板](consumer/end-user-dashboards.md)和[磁贴](service-dashboard-create.md)提供对内含见解的概览  。
* 示例数据让应用在安装后可被立即发现。

你可能熟悉其中每个部分（作为现有 Power BI 功能）。 在构建模板应用时，每个部分还有其他考虑事项。 请参阅下面的每个部分以获取更多详细信息。

<a name="queries"></a>

## <a name="queries"></a>查询
对于模板应用，在 Power BI Desktop 中开发的查询用于连接数据源和导入数据。 必须使用这些查询返回一致的架构，并且它们受支持以用于计划的数据刷新（不支持 DirectQuery）。

### <a name="connect-to-your-api"></a>连接到 API
需要从 Power BI Desktop 连接到你的 API 才能开始生成查询。

可以使用 Power BI Desktop 中的数据连接器来连接到 API。 可以使用 Web 数据连接器（获取数据 -> Web）连接到 Rest API 或 OData 连接器（获取数据 -> OData 数据源）来连接到 OData 数据源。

> [!NOTE]
> 目前，模板应用不支持自定义连接器，建议探索使用 Odatafeed Auth 2.0 作为一些连接用例的缓解措施，或提交你的连接器以供认证。 若要详细了解如何开发并认证连接器，请查看[数据连接器文档](https://aka.ms/DataConnectors)。

### <a name="consider-the-source"></a>考虑源
查询可定义包含在数据模型中的数据。 根据你系统的大小，这些查询还应包括筛选器以确保客户处理适合你业务方案的可管理的查询量。

Power BI 模板应用可并行执行多个查询，也可同时为多个用户执行查询。  请提前规划你的限制条件和并发策略，并就如何使你的模板应用具备容错能力向我们寻求帮助。

### <a name="schema-enforcement"></a>架构实施
确保你的查询能够弹性应对系统中发生的更改，刷新时的架构变更可破坏模型。 如果源可能为某些查询返回 NULL 或架构缺失结果，请考虑返回空表或有意义的自定义错误消息。

### <a name="parameters"></a>参数
Power BI Desktop 中的[参数](https://powerbi.microsoft.com/blog/deep-dive-into-query-parameters-and-power-bi-templates/)允许你的用户提供用于自定义数据（由用户检索）的输入值。 提前考虑这些参数以避免在耗费时间生成详细的查询或报表之后返工。

> [!NOTE]
> 模板应用支持除 Any 和 Binary 之外的所有参数。
>

### <a name="additional-query-tips"></a>其他查询提示

* 确保正确键入所有列。
* 列具有描述性名称（请参阅[问答](#qa)）。  
* 对于共享逻辑，请考虑使用函数或查询。  
* 目前，该服务不支持隐私级别。 如果收到有关隐私级别的提示，则可能需要重写查询以使用相对路径。  

## <a name="data-models"></a>数据模型

定义明确的数据模型可确保客户可轻松直观地与模板应用进行交互。 在 Power BI Desktop 中创建数据模型。

> [!NOTE]
> 应在[查询](#queries)中执行大部分基本建模（键入功能、列名）。

### <a name="qa"></a>问答
建模还影响问答为客户提供结果的能力。 确保将同义词添加到常用列，并在[查询](#queries)中为列正确命名。

### <a name="additional-data-model-tips"></a>其他数据模型提示

请确保已完成以下操作：

* 将格式应用于所有值列。 在查询中应用类型。  
* 将格式应用于所有度量值。
* 设置默认汇总。 特别是“不汇总”（如果适用）（例如，对于唯一值的情况）。  
* 设置数据类别（如果适用）。  
* 根据需要设置关系。  

## <a name="reports"></a>报表
报表页提供了对更多模板应用中包含的数据的见解。 使用报表中的页面来回答模板应用正尝试解决的关键业务问题。 使用 Power BI Desktop 创建报表。


### <a name="additional-report-tips"></a>其他报表提示

* 每页使用多个视觉对象进行交叉筛选。  
* 仔细使各视觉对象对齐（不重叠）。  
* 页面设置为“4:3”或“16:9”布局模式。  
* 所提供的全部聚合将使数字有意义（平均值、唯一值）。  
* 切片产生合理结果。  
* 徽标至少位于报表顶部。  
* 元素最尽可能地位于客户端的的配色方案中。  

<a name="dashboard"></a>

## <a name="dashboards"></a>仪表板
仪表板是与客户的模板应用交互的主要位置。 它应包括所含内容（尤其是你业务方案的重要指标）的概述。

要为模板应用创建仪表板，只需通过“获取数据”>“文件”上传 PBIX 或者直接从 Power BI Desktop 进行发布即可。


### <a name="additional-dashboard-tips"></a>其他仪表板提示

* 在固定时保持相同的主题，以便仪表板上的磁贴保持一致。  
* 将徽标固定到主题，以便使用者知道包的来源。  
* 建议用于多数屏幕分辨率的布局是 5-6 个小磁贴的宽度。  
* 所有仪表板磁贴应具有适当的标题/副标题。  
* 考虑在仪表板中为不同的方案分组（垂直或水平）。  

## <a name="sample-data"></a>示例数据
属于应用创建阶段的模板应用将缓存数据作为应用的一部分包装在工作区中：

* 可便于安装人员在连接数据之前了解应用的功能和用途。
* 创造促使安装人员进一步探索应用功能的体验，从而促成连接应用数据集。

建议在创建应用前获得优质示例数据， 以确保应用报表和仪表板都填充有数据。

## <a name="publishing-on-appsource"></a>发布到 AppSource
模板应用可以发布到 AppSource。将应用提交到 AppSource 前，请遵循以下指南：

* 确保你创建的模板应用包含有吸引力的示例数据，有助于安装人员了解应用的用途（空的报表和仪表板不会获准）。
模板应用支持仅包含示例数据的应用，请务必选中静态应用复选框。 [了解详细信息](https://docs.microsoft.com/power-bi/service-template-apps-create#create-the-test-template-app)
* 制定可供验证团队遵循的说明，其中包括连接到数据所需的凭据和参数。
* 应用必须在 Power BI 中和 CPP 产品/服务上添加“应用”图标。 [了解详细信息](https://docs.microsoft.com/power-bi/service-template-apps-create#create-the-test-template-app)
* 已配置登陆页面。 [了解详细信息](https://docs.microsoft.com/power-bi/service-template-apps-create#create-the-test-template-app)
* 务必遵循 [Power BI 应用产品/服务](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/power-bi/cpp-power-bi-offer)文档中的指南。
* 如果仪表板是应用的一部分，请确保仪表板不为空。
* 提交前，先使用应用链接安装应用，以确保你能连接数据集，且应用体验符合预期。
* 将 bpix 上传到模板工作区前，请先务必要卸载任何不必要的连接。
* 遵循 Power BI [报表和视觉对象设计最佳做法](https://docs.microsoft.com/power-bi/visuals/power-bi-visualization-best-practices)，以实现对用户的最大影响，并获得分发批准。
<!--- * In general, only application with valuable functionality can be approved for general use on AppSource. Application with sample data content only must have either a guidance or statistical value.) -->

## <a name="known-limitations"></a>已知限制

| 功能 | 已知限制 |
|---------|---------|
|目录：数据集   | 应存在一个完整的数据集。 仅支持在 Power BI Desktop（.pbix 文件）中构建的数据集。 <br>不支持：来自其他模板应用、跨工作区数据集、分页报表（.rdl 文件）、Excel 工作簿的数据集 |
|目录：仪表板 | 禁止使用实时磁贴（也就是说，不支持推送或流式处理数据集） |
|目录：数据流 | 不支持：数据流 |
|文件内容 | 仅支持 PBIX 文件。 <br>不支持：.rdl 文件（分页报表）、Excel 工作簿   |
| 数据源 | 可支持云“计划数据”刷新的数据源。 <br>不支持： <li> DirectQuery</li><li>实时连接（无 Azure AS）</li> <li>本地数据源（不支持个人和企业网关）</li> <li>实时数据源（不支持推送数据集）</li> <li>复合模型</li></ul> |
| 数据集：跨工作区 | 不支持跨工作区的数据集  |
| 查询参数 | 不支持：用于数据集的“Any”或“Binary”类型块刷新操作的参数 |
| 自定义视觉对象 | 仅支持公开可用的自定义视觉效果。 不支持[组织自定义视觉效果](developer/power-bi-custom-visuals-organization.md) |

## <a name="next-steps"></a>后续步骤

[什么是 Power BI 模板应用？](service-template-apps-overview.md)
