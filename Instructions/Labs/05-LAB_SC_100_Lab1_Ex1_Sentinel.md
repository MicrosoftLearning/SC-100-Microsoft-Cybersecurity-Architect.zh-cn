# 安全运营中心

## 练习概述

Contoso 有一个安全运营中心 (SOC)，用于监视和响应整个企业的安全事件。 SOC 配备了安全分析师、安全工程师和网络工程师。 SOC 已决定使用 Microsoft Sentinel 作为其安全信息和事件管理 (SIEM) 解决方案。 为了收集和分析整个企业的安全日志，SOC 有一个 Log Analytics 工作区。 SOC 要求根据最低特权原则保护对 Log Analytics 工作区的访问。 SOC 有两个不同的角色，安全分析师和安全工程师，分别有不同的权限要求。 网络团队要求仅访问 Cisco Umbrella 日志。

## 第 1 部分：设计解决方案（必需）

在此任务中，你将设计一个概念，用于监控和响应具有 Contoso 安全运营中心特定访问权限的安全事件。

### 设计方法

初始步骤包括基于所描述的场景分析要求，理解目标，并定义要求。

根据提供的用例，可以概述以下要求：

- 部署 SIEM/SOAR 解决方案
- 限制对特定 SOC 角色的访问
- 创建包含事件及其警报的自定义视图的仪表板

在此场景中，你将基于 Microsoft Sentinel 部署 SIEM SOAR 解决方案，在工作区上下文中设置基于角色的访问控制，并将网络团队的访问权限限制为 Log Analytics 工作区中的单个表。 工作簿允许安全分析师和管理员使用图形显示来可视化安全数据。 它们提供了一个在控制面板中呈现和分析数据的工具。

### 建议的解决方案

| 要求 | 解决方案 | 操作计划 |
| ---- | ---- | ---- |
| 部署 SIEM/SOAR 解决方案 | Microsoft Sentinel，Log Analytics 工作区 | 设置 Log Analytics 工作区并部署 Microsoft Sentinel |
| 限制对特定 SOC 角色的访问 | Log Analytics 工作区，基于角色的访问控制 | 为 Log Analytics 工作区设置 RBAC |
| 创建包含事件及其警报的自定义视图的仪表板 | Microsoft Sentinel，工作簿 | 创建包含当前事件和警报的自定义视图的工作簿 |

## 第 2 部分：实施解决方案（可选）

### 任务 1 - 创建 Log Analytics 工作区

在此任务中，你将创建一个 Log Analytics 工作区。该工作区需要容纳 Microsoft Sentinel 将引入并用于其检测和分析的所有数据。

1. 使用 **lon-sc1\admin** 帐户登录到客户端 1 VM (LON-SC1)。 密码应由实验室托管提供程序提供。
2. 打开 **Microsoft Edge**，选择地址栏，导航到 **`https://portal.azure.com`** 并以 **User1-*******@LODSUATMCA.onmicrosoft.com** 用户身份（其中 ****** 是实验室托管提供商提供的唯一租户 ID）登录到 Azure 门户。 用户的密码应由实验室托管提供程序提供。
3. 在“保持登录?”对话框上，选中“不再显示此内容”复选框，然后选择“**否**”。
4. 选择底部的“从不”关闭密码保存对话框，以便不在浏览器中保存默认的全局管理员凭据。
5. 取消“欢迎使用 Microsoft Azure”的屏幕。
6. 选择“**创建资源**”并搜索“**Log Analytics 工作区**”
7. 找到 **“Log Analytics 工作区”图块**，选择“**创建**”。
8. 在“创建 Log Analytics 工作区”站点上，新建**资源组**并将其命名为**`rg_eastus_soc`**。
9. 在实例详细信息中输入名称**`law-sentinel`**，在区域处选择“**美国东部**”。
10. 选择“**查看并创建**”
11. 选择“创建”以开始部署。****

你已成功为 Sentinel 部署创建 Log Analytics 工作区。

### 任务 2 - 创建 Sentinel

在此任务中，将 Sentinel 添加到创建的 Log Analytics 工作区并添加演示日志，因为演示租户在 Log Analytics 工作区中没有现成数据，因此导入演示日志以更好地了解 Sentinel 的工作原理。

1. 你仍应登录到 Azure 门户 **https://portal.azure.com**。
1. 在搜索栏页面顶部的蓝色横幅中，输入 **Microsoft Sentinel**，然后从“服务”下列出的搜索结果中选择它。
1. 在“**Microsoft Sentinel**”页中，选择“**创建**”。
1. 在“**将 Microsoft Sentinel 添加到工作区**”页中，应列出以前创建的 Log Analytics 工作区。  选择“**law-sentinel**”，然后选择“**添加**”。
1. 将 Sentinel 添加到工作区可能需要几分钟时间。  添加后，将显示“**Microsoft Sentinel | 新建和指南**”页。  你会收到通知，Microsoft Sentinel 免费试用版已激活。  选择“确定”。
1. 在页面中心，选择“**转到内容中心**”。  或者，在左侧导航面板中，展开“**内容管理**”，然后选择“**内容中心**”。
1. 在“内容中心”中，安装 **Microsoft Sentinel 培训实验室**。  要查找此解决方案，请筛选 **Provider = Microsoft** 和 **Category = Training and Tutorials**。 设置并应用这些筛选器后，将列出 **Microsoft Sentinel 培训实验室**。 从搜索结果中选择它，然后**安装**解决方案。
1. 选择**创建**。
1. 选择资源组 **rg_eastus_soc** 和工作区 **law-sentinel**。
1. 选择“**查看和创建**”，然后选择“**创建**”。
1. 请等待解决方案成功安装。

你已成功将 Sentinel 部署到 Log Analytics 工作区并添加了数据。 

### 任务 3 - 设置 RBAC

必须基于最低特权原则确保访问安全，才能为特定角色要求创建角色分配。 在即将进行的高效部署中，安全操作中心将有两个不同的角色。
此外，网络团队还需要访问 Cisco Umbrella 日志。 必须确保网络团队只能访问这些日志。

#### 权限要求

| 角色 | 权限 |
|---|---|
| 安全分析师 | 查看数据、事件、工作簿和其他 Sentinel 资源 |
| | 分配/消除事件。 |
| 安全工程师 | 创建和编辑工作簿和分析规则 |
| | 从内容中心安装和更新解决方案 |
| 网络团队 | 组读取权限： 表上的 **NOC**： **Cisco_Umbrella_dns_CL**|

---

1. 你仍应登录到 Azure 门户 **https://portal.azure.com**。
1. 在顶部搜索栏中，搜索“**资源组**” 并选择之前创建的资源组 **rg_eastus_soc**。
1. 在左侧导航窗格中，选择“访问控制(IAM)”****。
1. 选择“**添加**”，然后从下拉列表中选择“**添加角色分配**”。
1. 搜索“**`Microsoft Sentinel Responder`**”并在“详细信息”列中选择“**视图**”。
1. 查看权限是否符合要求。
1. 关闭右上角带“**X**”的窗口。
1. 选择**下一步**。
1. 选择“+选择成员”。
1. 搜索“**`SOC Analysts`**”组，从搜索结果中选择“**SOC 分析师**”，按 **SELECT** 并添加角色分配。
1. 选择“查看 + 分配”。
1. 对 Sentinel 参与者角色重复这些步骤。 选择“**添加**”，然后从下拉列表中选择“**添加角色分配**”。
1. 搜索“**`Microsoft Sentinel Contributor`**”并选择该角色。
1. 选择**下一步**。
1. 选择“+选择成员”。
1. 在“**选择成员**”边栏选项卡上，搜索“**SOC 工程师**”组。  从搜索结果中选择“**SOC 工程师**”，按 **SELECT** 添加角色分配。
1. 选择“查看 + 分配”**** 两次。
1. 选择“**角色分配**”选项卡，确认已设置角色分配。
1. 现在，请添加自定义角色。 选择“**添加**”，从下拉菜单中选择“**添加自定义角色**”。
1. 将其命名为 **`NOC-CiscoUmbrellaCL-Read`**。
1. 对于“**基线权限**”，请选择“**从头开始**”。
1. 选择**下一步**。
1. 在“**权限**”标签页上，选择“**添加权限**”。
1. 搜索“**`Microsoft.OperationalInsights`**”，选择“**Azure Log Analytics**”卡。
1. 添加以下权限。
    - Microsoft.OperationalInsights/workspaces
        - 读取：获取工作区
        - 其他：搜索工作区数据

    - Microsoft.OperationalInsights/workspaces/analytics
        - 其他：搜索

    - Microsoft.OperationalInsights/workspaces/query
        - 读取：查询工作区中的数据

    - Microsoft.OperationalInsights/workspaces/tables/query
        - 读取：查询工作区表数据

1. 选择“查看 + 创建”  。
1. 选择“**创建**”，然后选择“**确定**”。
1. 在顶部搜索栏中，搜索 **`Resource groups`**，然后选择“**rg_eastus_soc**”。
1. 打开 Log Analytics 工作区 **law-sentinel**。
1. 在左侧导航窗格中，展开“**设置**”并选择“**表**”。
1. 搜索 **`Cisco_Umbrella_dns_CL`**。
1. 单击省略号 (...)，选择“**访问控制 (IAM)**”
1. 选择“添加”“添加角色分配”。
1. 搜索 **`NOC-CiscoUmbrellaCL-Read`**，然后选择自定义角色。
1. 选择**下一步**。
1. 选择“**选择成员**”，搜索“**NOC**”，从搜索结果中选择它，然后按“**选择**”
1. 选择“查看 + 分配”**** 两次。

你已成功根据 Contoso 安全运营团队的角色要求创建了基于角色的访问模型，并为网络团队创建了自定义角色，并在 Log Analytics 工作区中的特定表上分配了该角色。

### 任务 4 - 创建工作簿

在此任务中，你将创建一个工作簿，以获取包含自定义视图和当前事件及其警报的仪表板。

1. 你仍应登录到 Azure 门户 **https://portal.azure.com**。
1. 在顶部搜索栏上，搜索 **`Microsoft Sentinel`** 并将其打开。
1. 选择“**law-sentinel**”。
1. 在左侧导航窗格中，展开“**威胁管理**”并选择“**工作簿**”。
1. 选择“**添加工作簿**”。
1. 选择“编辑”  。
1. 选择右侧的第一个“**编辑**”按钮。
1. 选择“添加” > “添加参数”。
1. 选择“**添加参数**”并填写以下信息：
     - **参数名称：** TimeRange
     - **参数类型：** 时间范围选取器
1. 检查以下设置：
     - **必需？**
1. 选择“保存”。
1. 在左下角的“**TimeRange:**”下拉菜单中，选择“**过去 7 天**”。
1. 选择“**添加参数**”并填写以下信息：
     - **参数名称：** AlertSeverity
     - **参数类型：** 下拉列表
1. 检查以下设置：
     - **必需？**
     - **允许多重选择**
     - **在读取模式下隐藏参数**
1. 在“**Log Analytics 工作区日志查询**” 下粘贴到：

    ```KQL
    SecurityAlert
    | summarize Count = count() by AlertSeverity
    | order by Count desc, AlertSeverity
    | project Value = AlertSeverity, Label = strcat(AlertSeverity, ' - ', Count)
    ```

1. 在“**时间范围**”下拉菜单中选择“**TimeRange**”。
1. 向下滚动到“**包括在下拉列表中**”，选中“**全部**”，并将“**默认选定项**”设置为“**全部**”。
1. 选择“保存”。
1. 选择“**添加参数**”并填写以下信息：
     - **参数名称：** ProductName
     - **参数类型：** 下拉列表
1. 检查以下设置：
     - **必需？**
     - **允许多重选择**
     - **在读取模式下隐藏参数**

1. 在“**Log Analytics 工作区日志查询**” 下粘贴到：

    ```KQL
    SecurityAlert
    | summarize Count = count() by ProductName
    | order by Count desc, ProductName asc
    | project Value = ProductName, Label = strcat(ProductName, ' - ', Count)
    ```

1. 在“**时间范围**”下拉菜单中，选择“**TimeRange**”
1. 向下滚动到“**包括在下拉列表中**”，选中“**全部**”，并将“**默认选定项**”设置为“**全部**”。
1. 选择“保存”。
1. 选择“**添加**”，然后选择“**添加查询**”。
1. 在“**Log Analytics 工作区日志查询**” 下粘贴到：

    ```KQL
    SecurityIncident
    | where CreatedTime {TimeRange:value}
    | summarize arg_max(TimeGenerated,*) by tostring(IncidentNumber)
    | extend IncidentID = IncidentName
    | extend Alerts = extract("\\[(.*?)\\]", 1, tostring(AlertIds))
    | mv-expand AlertIds to typeof(string)
    | join
    (
        SecurityAlert
        | extend AlertEntities = parse_json(Entities)
        | mv-expand AlertEntities
    ) on $left.AlertIds == $right.SystemAlertId
    | summarize AlertCount=dcount(AlertIds) by IncidentNumber, Status, Severity, Title, Alerts, IncidentUrl, IncidentID
    | project IncidentNumber, IncidentID, Title, Severity, Status, AlertCount, Alerts, IncidentUrl
    | order by Severity
    ```

1. 在“时间范围”下拉菜单中选择“**TimeRange**”。
你将设置动态内容以获取所选事件的所有警报。 将导出警报并在此查询外部可用。
1. 在“**编辑查询**”窗口顶部选择“**高级设置**”标签页。
1. 检查以下设置：
    - **选择项后，导出参数** 
1. 选择“添加参数”并填写以下信息：
    - **要导出的字段：** 警报
    - **参数名称：** 警报
1. 选择“保存”。
1. 返回到“**设置**”标签页。
1. 选择“运行查询”。****
1. 选择“列设置”。
1. 选择“**IncidentUrl**”。
1. 将“列呈现器”设置为“**链接**”。
1. 在“链接设置”下，将“**要打开的视图**”设置为“**URL**”。
1. 选择**保存并关闭**。
1. 接下来，将基于所选事件创建警报视图。
1. 在“**编辑查询项**”窗口底部选择“**+ 添加**”。 选择“添加查询”。
1. 将 KQL 粘贴到 Log Analytics 工作区日志查询中

    ```KQL
    SecurityAlert
    | where SystemAlertId in ({Alerts})
    | summarize by  DisplayName, StartTime, EndTime,  SystemAlertId
    | sort by EndTime desc
    ```

1. 在“时间范围”下拉列表中选择“**TimeRange**”。
1. 选择“完成编辑”。
1. 在“**新建工作簿**”窗口的顶部栏中选择“**完成编辑**”。
1. 选择一个**事件**。
1. 与链接事件相关的警报将显示在下方。
1. 选择“保存”图标来保存查询。  
1. 在“**另存为**”窗口中，输入新工作簿的标题，从下拉列表中选择“**rg_eastus_soc**”资源组，然后选择“**另存为**”。

已成功创建仪表板，其中包含事件和相关警报的自定义视图。
