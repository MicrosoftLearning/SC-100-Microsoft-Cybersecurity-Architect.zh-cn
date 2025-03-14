# 数据分类框架

你被分配了为 Contoso Ltd. 构建数据分类的任务，以便为 ISO-27001：2022 审核做准备。 目标是建立一个可靠的框架，这对于确保有效保护数据免遭泄露、删除和丢失至关重要。 你的角色涉及为公司内部的建筑项目集成新的项目 ID 系统。 为遵守政府法规，所有包含特定项目 ID 的文件必须保留 5 年。

下面给出了对项目 ID 进行分类的示例：

|项目 ID|
|----|
|PAR-1023-DA|
|BER-0822-AB|
|Rom-0419-bm|
|sTr*1223-Se|
|BaR#0418-ag|
|dui0522-in|

## 第 1 部分：设计解决方案（必需）

在此任务中，你将设计一个概念来解决 Contoso Ltd. 面临的问题。

### 设计方法

引入新的项目 ID 需要创建相应的敏感信息类型 (SIT)，这需要开发包含正则表达式的自定义模式。 随后，此 SIT 可用于设计保留标签和关联的自动标记策略。

### 建议的解决方案

|要求|解决方案|操作计划|
|----|----|----|
|标识包含项目 ID 的文档|Microsoft Purview 信息保护|创建自定义敏感信息类型|
|遵守政府法规，将数据保留 5 年| Microsoft Purview 数据生命周期管理|部署保留策略|

## 第 2 部分：实施解决方案（可选）

### 任务 1 - 创建自定义敏感信息类型

你将创建自定义敏感信息类型来检测包含项目 ID 的文档。

1. 以 Allan Deyoung 的身份，使用他的管理员帐户：**MOD Administrator** 登录到 Microsoft Purview 合规门户 **`https://purview.microsoft.com/`**。
1. 如果系统要求设置多重身份验证，请按照说明操作。
1. 你将转到新的 Microsoft Purview 门户登陆页面。 选择“**我同意数据流披露条款和隐私声明**”这一描述旁边的框，然后选择“**开始**”。
1. 在左侧导航面板中，选择“**解决方案**”，然后选择“**信息保护**”。 或者，在主窗口中选择“**查看所有解决方案**”磁贴，然后选择“数据安全”下列出的“**信息保护**”磁贴。
1. 展开**分类器**并选择“**敏感信息类型**”。
1. 在“**敏感信息类型**”页上，选择“**+ 创建敏感信息类型**”。
1. 在“**为敏感信息类型命名** ”页面上，输入以下信息：
    - 名称：**`Project Identification Number`**
    - 说明：**`Identifies project identification number`**
1. 选择**下一步**。
1. 在“定义此敏感信息类型的模式”页面上，选择“+ 创建模式”。********
1. 在“**新建模式**”页面，选择“**添加主元素**”，然后选择“**正则表达式**”。
1. 在 **ID** 文本框中的“**添加正则表达式**”页上，键入 **`ProjectID`**。
1. 在“**正则表达式**”文本框中，输入以下表达式：

    **`[a-zA-Z]{3}(\W)?[\d]{4}(\W)?[a-zA-Z]{2}`**

    >[!NOTE] 所提供的正则表达式经过精心设计，用于标识以下序列：三个字母，后跟可能可选的非单词字符，然后是四位数字，再后跟可选的非单词字符，最终以两个字母结尾。 非单词字符的存在是可自由裁量的，总体模式旨在对应于数据中的特定格式或结构。

1. 在“正则表达式”文本框中，选择“**字符串匹配**”，然后选择“**完成**”。
1. 在“**新建模式**”窗口中，对于“**可信度**”，选择“**高可信度**”，选择“**创建**”，然后选择“**下一步**”。
1. 在“**选择推荐的可信度以显示在合规性策略中**”页面，将设置保留为“**高可信度**”，然后选择“**下一步**”。
1. 在“**检查设置并完成**”上，验证设置，选择“**创建**”，然后在创建策略时选择“**完成**”。

已成功创建一个新的敏感信息类型来标识项目 ID。

### 任务 2：创建保留标签

你将创建保留标签，以将与建筑项目相关的所有文档保留 5 年。

1. 你仍应登录到 Microsoft Purview 门户 **https://purview.microsoft.com/**。
1. 在左侧导航面板中，选择“**解决方案**”，然后选择“**数据生命周期管理**”。
1. 选择“**保留标签**”。
1. 在“**标签**”页上，选择“**创建标签**”。
1. 在“**为保留标签命名**”页上输入以下信息：

    - 名称：**`Retention of Construction Project Documentation`**
    - 面向用户的说明：**`The construction project documentation Retention Policy dictates the retention of all project-related documents for five years following project completion.`**
    - 面向管理员的说明：**`This label is applied to retain construction project documents for a period of five years, and it is utilized in conjunction with auto-labeling.`**

1. 选择**下一个**
1. 在“**定义标签设置**”页上，选择“**将项永远保留或保留特定期限**”，然后选择“**下一步**”。
1. 在“**定义保留期限**”页面上，输入以下信息：

    - 将项保留：**5 年**
    - 保留期开始依据：**项创建时间**

1. 选择**下一步**。
1. 在“**选择保留期过后发生的情况**”页上选择“**停用保留设置**”，然后选择“**下一步**”。
1. 在“**查看并完成**”页面上查看设置，然后选择“**创建标签**”。
1. 在“**已创建保留标签**”页面上，有多个选项。  选择“**不执行任何操作**”，然后选择“**完成**”。  你将在下一个任务中创建自动应用策略。 选择“自动将此标签应用于特定类型的内容”，将引导你完成后续任务中的步骤，从步骤 4 开始。

1. 请不要关闭浏览器标签页，继续执行下一个任务。

你已成功创建保留期为 5 年的保留标签。

### 任务 3 - 自动应用保留标签

你将使用在本练习中创建的敏感信息类型来自动应用保留标签。

1. 你仍应登录到 Microsoft Purview 门户中的数据生命周期管理解决方案。  如果没有，请导航到 **`https://purview.microsoft.com/`** > “**解决方案**” > “**数据生命周期管理**”。
1. 在“**数据生命周期管理** ”窗格上，选择“**标签策略**”。
1. 在“**标签策略**”边栏选项卡上，选择“**自动应用标签**”。
1. 在“**我们开始吧**”页面上，输入以下信息：

    - 名称：**`Label documents related to construction projects`**
    - 说明：**`This policy automatically enforces the "Construction Project Documentation Retention" policy on any document pertaining to construction projects.`**

1. 选择**下一步**。
1. 在“**选择要应用此标签的内容类型**”页上，选择“**将标签应用于包含敏感信息的内容**”，然后选择“**N下一步**”。
1. 在“**包含敏感信息的内容**”页面上，选择“**自定义**”，然后选择“**自定义策略**”，再选择“**下一步**”。

1. 在“**定义包含敏感信息的内容**”上，指定以下设置：
    - 组名：**`Project ID lookup`**
    - 在“敏感信息类型”下选择“**添加**”，然后选择“**敏感信息类型**”。  
    - 在“敏感信息类型”页的搜索字段中，输入创建的 **`Project Identification number`** 标签的名称，然后按“返回”。  选择“**项目标识号**”，然后选择“**添加**”。
    - 将可信度保留为“**高可信度**”。
    - 将实例计数保留为“**1 到任意数字**”。
    - 选择**下一个**
1. 在“**策略范围**”页上，将“管理单元”设置保留为“**完整目录**”，然后选择“**下一步**”。
1. 在“**选择要创建的保留策略类型**”页上，选择“**静态**”，然后选择“**下一步**”。
1. 在“**选择自动应用标签的位置**”上，验证所有可用位置的状态是否设置为“**开**”，然后选择“**下一步**”。
1. 在“**选择要自动应用的标签**”上，选择“**添加标签**”，然后选择你在上一任务中创建的“**建设项目文档的保留**”，选择“**添加**”，然后选择“**下一步**”。
1. 在“**决定是测试还是运行策略**”中，选择“**启用策略**”，然后选择“**下一步**”。
1. 在“**查看并完成**”页上查看所有设置，选择“**提交**”，然后选择“**完成**”。

你已成功发布保留标签并将其自动应用于包含项目 ID 的所有文档。
