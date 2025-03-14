# 跨租户同步

Contoso 最近收购了 Tailwind Traders，因此很难确定哪些来宾是客户或合作伙伴，以及哪些是被收购公司的员工。 你的任务是向 Contoso 提供 Tailwind Traders 的初始通用地址列表。 此外，租户必须仅允许将已知域名作为来宾用户添加到 Entra ID。 你希望限制只有内部用户才能邀请来宾加入组织。 将限制外部访问，仅允许一个域。 你还将创建从 Tailwind Traders 到 Contoso 的 B2B 协作和跨租户同步。

作为网络安全架构师，必须确保满足所有要求，并对 Entra ID 实施必要更改，以应用零信任原则。 在本练习中，你将与实验室伙伴合作，创建跨租户同步。

## 第 1 部分：设计解决方案（必需）

在此任务中，你将设计一个概念来解决 Contoso Ltd. 和 Tailwind Traders 面临的需求。

### 设计方法

初始步骤包括基于所描述的场景分析要求，理解目标并定义要求。

根据提供的用例，可以概述以下要求：

- 同步两家公司的用户
- 使外部用户可识别为外部用户
- 仅限内部员工拥有邀请权 
- 限制外部访问，仅允许公司信任的域

第二步，检查 Contoso Ltd. 的现有环境。 Microsoft Entra ID 提供启用外部协作的解决方案。 调查存在哪些控制措施以及哪些策略已到位。 使用 Entra ID 门户查看当前的配置和策略，并确定必须实施哪些调整才能满足分割的要求。

第三阶段涉及构思解决方案的概念。 经过调查发现，跨租户同步显然是实现满足所有要求的最佳协作方式。  

### 建议的解决方案

|要求|解决方案|操作计划|
|----|----|----|
|仅限内部员工拥有邀请权|外部协作设置|将邀请权限限制为租户成员和特定管理员邀请角色|
|阻止来自除受信任域列表外的所有邀请|外部协作设置|创建仅包含受信任域的邀请允许列表|
|同步两家公司的用户|Entra ID 跨租户同步|在两个 Entra ID 租户之间建立访问信任，并创建将用户同步到另一个租户的配置|
|使外部用户可识别为外部用户|跨租户属性映射|使用跨租户同步配置的属性映射函数创建自定义表达式，编辑每个同步用户的 displayName 属性|

## 第 2 部分：实施解决方案（可选）

### 任务 1 - 组队和准备工作

在此任务中，你将检查跨租户同步的先决条件，并与实验室伙伴合作。

1. 使用 **lon-sc1\admin** 帐户登录到客户端 1 VM (LON-Sc1)。 密码应由实验室托管提供程序提供。
1. 打开 **Microsoft Edge**，选择地址栏，导航到**`https://entra.microsoft.com`** 并以 **MOD 管理员**身份admin@WWLxZZZZZZ.onmicrosoft.com（其中 ZZZZZZ 是实验室托管服务提供商提供的唯一租户 ID）登录到 Entra ID 门户。 管理员的密码应由实验室托管提供程序提供。
1. 如果系统要求设置多重身份验证，请按照说明操作。
1. 在“保持登录?”对话框上，选中“不再显示此内容”复选框，然后选择“否”  。
1. 选择“**以后再说**”关闭密码保存对话框，以便不在浏览器中保存默认的全局管理员凭据。
1. 在左侧导航窗格中，导航到“**标识**” > “**概述**”。
1. 记下“**概览**”选项卡下显示的**租户 ID** 和**主域**。
1. 与实验室合作伙伴交换**租户 ID** 和**主域**。

你应计划好要使用什么拓扑、第一次同步测试的范围包含哪些用户，以及要如何将它们映射到 Contoso 的 Entra ID 租户。

### 任务 2 - 配置跨租户访问

由于现在已准备好实现跨租户同步所需的信息，因此现在将执行必要的步骤，以便你的租户可以访问合作伙伴的租户，反之亦然。

1. 你仍应在 Entra ID 门户**https://entra.microsoft.com**中保持登录状态。
1. 在左侧导航窗格中，导航到“**标识**” > “**外部标识**” > “**跨租户访问设置**”。
1. 选择“**组织设置**”选项卡，然后选择“**添加组织**”。
1. 使用实验室合作伙伴提供的租户 ID 填写**租户 ID 或域名**字段，然后选择“**添加**”。
1. 在 Contoso 的“**入站访问**”下，选择“**继承自默认值**”，以访问该特定租户的跨租户同步设置。
1. 在“**信任设置**”下，启用“**自动兑换租户 Contoso 的邀请**”。
1. 选择“保存”。
1. 在**跨租户同步**下，启用“**允许用户同步到此租户**”。
1. 选择“**保存**”，然后使用右上角的 **X** 关闭对话框。
1. 在 Contoso 的“**出站访问**”下，选择“**继承自默认值**”，以访问该特定租户的跨租户同步设置。
1. 在“**信任设置**”下，启用“**自动兑换租户 Contoso 的邀请**”。
1. 选择“**保存**”，然后使用右上角的 **X** 关闭对话框。

你已启用租户与合作伙伴租户的双向同步功能，用户无需手动兑换其邀请。 

### 任务 3 - 限制外部访问

在此任务中，你将通过控制有权限发送邀请的用户范围来限制邀请新来宾用户加入组织的能力。 你将限制可被邀请为合作伙伴租户域的来宾的域范围。

1. 你仍应在 Entra ID 门户**https://entra.microsoft.com**中保持登录状态。
1. 在左侧导航窗格中，导航到“**标识**” > “**外部标识**” > “**外部协作设置**”。
1. 在“**来宾邀请设置**”部分下，选择“**成员用户和分配到特定管理员角色的用户可以邀请来宾用户(包括具有成员权限的来宾)**”。
1. 在“**协作限制**”下，选择“**仅允许向指定域发送邀请**”。
1. 添加实验室合作伙伴的域 **WWLxZZZZZZ.onmicrosoft.com**（其中 ZZZZZZ 是实验室合作伙伴由实验室托管提供程序提供的唯一租户 ID）。
1. 选择“保存”。

现在，你已限制谁可以邀请和谁可以被邀请到你的租户。

### 任务 4 - 创建配置

在此任务中，你将创建基本配置并测试与其他租户的连接。

1. 你仍应在 Entra ID 门户**https://entra.microsoft.com**中保持登录状态。
1. 在左侧导航窗格中，导航到“**标识**” > “**外部标识**” > “**跨租户同步**” > “**配置**”。
1. 创建**新配置**。
1. 输入名称 **Contoso 跨租户同步**，然后选择“**创建**”。
1. 在“**预配**”下将“**预配模式**”更改为“**自动**”。
1. 在“**租户 ID**”框中，输入你在任务 1 中记下的合作伙伴租户 ID。
1. 选择“测试连接”。
1. 选择“保存”。

如果正确配置了访问设置，你应会看到一条消息，显示所提供的凭据已授权启用预配。 如果测试连接失败，请检查合作伙伴是否已在任务 3 中输入了正确的域，并按照任务 2 中所述配置了跨租户同步。

### 任务 5 - 定义用户范围

在此任务中，你将定义谁在第一次同步的范围内。

1. 你仍应该在刚刚创建的配置的预配设置中登录到 Entra ID 门户 **https://entra.microsoft.com**。 如果没有，请执行以下 3 个步骤返回那里：
   1. 在左侧导航窗格中，导航到“**标识**” > “**外部标识**” > “**跨租户同步**” > “**配置**”。
   1. 选择“**Contoso 跨租户同步**”
   1. 导航到“**预配**”。
1. 展开“**设置**部分”。
1. 验证“范围”下拉列表是否设置为“**仅同步已分配的用户和组**”。
1. 如果更改任何设置，请选择“**保存**”。
1. 在导航窗格中，选择“**用户和组**”。
1. 选择“添加用户/组”。****
1. 在“**添加分配**”页上，选择“**未选择任何内容**”链接。
1. 使用复选框搜索“**法律**”，突出显示“**法律团队**”，然后选择“**选择**”。
1. 选择“**分配**”，将由两名用户组成的团队添加到同步范围。

现已定义用户的第一个范围，以便同步到合作伙伴的租户。

### 任务 6 - 查看属性映射

在此任务中，你将查看属性映射，并确保同步的用户将显示在 Contoso 的全局地址列表中。

1. 仍应在跨租户同步配置的“**用户和组**”设置中登录到 Entra ID 门户 **https://entra.microsoft.com**。 如果未遵循用于返回到配置页的以下 2 个步骤：
   1. 在左侧导航窗格中，导航到“**标识**” > “**外部标识**” > “**跨租户同步**” > “**配置**”。
   1. 选择“**Contoso 跨租户同步**”。
1. 导航到“**预配**”，然后展开“**映射**”部分。
1. 选择“预配 Microsoft Entra ID 用户****”。
1. 在“**showInAddressList**”属性下，选择“编辑”。
1. 确保其映射类型设置为“**常量**”，其值设置为“**true**”。
1. 选择“确定”。
1. 在“**displayName**”属性下，选择“**编辑**”。
1. 将“映射类型”更改为“**表达式**”。
1. 移除预先存在的表达式。
1. 选择“**使用表达式生成器**”。
1. 选择函数“**追加**”。
1. 在“**选择特性化**”字段中，从下拉列表中选择 **[displayName]** 属性。
1. 在“**输入 后缀**”字段中，输入 **' (Contoso)'**，包括括号前的空格，但不含引号。
1. 选择“**添加表达式**”。 右侧的表达式应该类似于 `Append([displayName], " (Contoso)")`
1. 可以通过选择用户来测试表达式。
1. 选择“**应用表达式**”，然后选择“**确定**”以离开编辑对话框。
1. 选择“保存”。
1. 选择页面右上角的“**X**”，关闭“属性映射”页。

现在，你已确保所有同步的用户都会显示在其他租户的全局地址列表中。 此外，还向其他租户中每个已同步用户的显示名称添加了一个 (Contoso) 表达式。

### 任务 7 - 通过实验室合作伙伴启用预配和测试

在此任务中，你将启用预配并测试用户是否按你所需的方式出现在其他租户中。 由于自动预配可能需要一段时间，因此我们将触发手动预配。

1. 你仍应该在跨租户同步配置的预配设置中登录到 Entra ID 门户 **https://entra.microsoft.com**。 如果未遵循用于返回到配置页的以下 2 个步骤：
   1. 在左侧导航窗格中，导航到“**标识**” > “**外部标识**” > “**跨租户同步**” > “**配置**”。
   1. 选择“**Contoso 跨租户同步**”
1. 导航到“**按需预配**”。
1. 搜索作为限定范围的**法律团队**成员的 **`Grady Archie`**。
1. 选择“预配”****。
    - 预配完成后，你将收到一条确认信息。
1. 选择屏幕右上角的 **X** 关闭“执行操作”页。

你刚刚手动预配了第一个用户，在检查合作伙伴租户的用户列表时，你将看到用户 Grady Archie (Contoso)。

### 任务 8 - 推出完全同步

由于第一个用户预配已成功通过测试，现在你会将其余用户列表预配到其他租户。

1. 你仍应该在跨租户同步配置的“**按需预配**”页中登录到 Entra ID 门户 **https://entra.microsoft.com**。 如果未遵循用于返回到配置页的以下 2 个步骤：
   1. 在左侧导航窗格中，导航到“**标识**” > “**外部标识**” > “**跨租户同步**” > “**配置**”。
   1. 选择“**Contoso 跨租户同步**”
1. 选择“**用户和组**”。
1. 选择“添加用户/组”。
1. 在“**添加分配**”页上，选择“**未选择任何内容**”链接。
1. 选择“**所有员工**”组，因其是包含**所有 Contoso 员工**的组。
1. 确认选择。
1. 选择“分配”****。

现已成功在租户与另一个租户之间创建跨租户同步。 你应该能够根据 Tailwind Traders 的生产系统调整在演示租户中执行的一切操作，并帮助他们集成到 Contoso 中。
