# 条件访问

你发现员工正在从未知位置访问 Microsoft 365，尽管你的条件访问策略仅允许从特定位置和设备进行访问。 你的调查显示，这些员工在乘坐公共交通工具从办公室回家的途中访问 Microsoft 365。 此行为违反了行业法规，并且你想要使用连续访问评估来防止此行为。 此外，你想要实现在上一练习中准备的身份验证强度，以保护处理客户数据的某些应用程序。 

## 第 1 部分：设计解决方案（必需）

在此任务中，你将设计一个概念来应对 Contoso Ltd. 面临的风险。

### 设计方法

初始步骤包括基于所描述的问题分析要求，理解目标并定义要求。

根据提供的用例，可以概述以下要求：

- 限制来自不安全/未知位置的访问
- 需要对包含敏感信息的应用进行强身份验证

第二步，检查 Contoso Ltd. 的现有环境。 Microsoft Entra ID 提供了使用 Entra ID 条件访问策略管理和限制用户访问的解决方案。 调查存在哪些控制措施以及哪些策略已到位。 使用 Entra ID 门户查看当前的配置和策略，并确定是否需要调整，或者是否需要实施新策略。

第三阶段涉及构思解决方案的概念。 经过调查，很明显尚未配置受信任的网络，并且当前策略都不满足定义的要求。 因此，一组新的条件访问策略是必不可少的。 

### 建议的解决方案

|要求|解决方案|操作计划|
|----|----|----|
|限制来自不安全/未知位置的访问|Entra ID 条件访问策略|将当前公司的网络定义为可信网络，并限制对该网络内设备的访问|
|需要对包含敏感信息的应用进行强身份验证|Entra ID 条件访问策略|新建范围限定为敏感应用程序的条件访问策略，该策略需要刚创建的强化身份验证强度，但不包括短信和语音等不安全的身份验证方法|

## 第 2 部分：实施解决方案（可选）

### 任务 1 - 创建受信任的网络

在此任务中，你将使用 VM 的外部 IP 地址创建命名位置，以定义可在以下任务中的条件访问策略中使用的受信任网络。 你将使用此地址，因为你的计算机位于公司网络中。

1. 使用 **lon-sc1\admin** 帐户登录到客户端 1 VM (LON-Sc1)。 密码应由实验室托管提供程序提供。
1. 使用鼠标右键选择“开始”菜单，然后选择“**终端**”，打开 **PowerShell** 窗口。
1. 输入以下 cmdlet 以检查当前的外部 IP 地址：`Invoke-RestMethod -Uri "http://ifconfig.me/ip"`
1. 记下 PowerShell 返回的 IP 地址。
1. 打开 **Microsoft Edge**，选择地址栏，导航到 **`https://entra.microsoft.com`** 并以 **MOD Administrator**admin@WWLxZZZZZZ.onmicrosoft.com（其中 ZZZZZZ 是实验室托管提供商提供的唯一租户 ID）登录到 Entra ID 门户。 管理员的密码应由实验室托管提供程序提供。
1. 如果系统要求设置多重身份验证，请按照说明操作。
1. 在“保持登录?”对话框上，选中“**不再显示此内容**”复选框，然后选择“**否**”。
1. 选择“**以后再说**”关闭密码保存对话框，以便不在浏览器中保存默认的全局管理员凭据。
1. 在左侧导航窗格中，导航到“**保护**” > “**条件访问**” > “**命名位置**”。
1. 选择“**+ IP 范围位置**”。
1. 输入“**受信任的 Contoso 网络**”名称。
1. 选择“标记为可信位置”。****
1. 选择 **+** 以添加在**步骤 4** 中记录的 IP 地址。
1. 输入应如下所示：``168.245.***.***/32``（*** 可能会根据你的实验室托管提供程序而有所不同）。
1. 选择 **添加** 。
1. 选择**创建**。

现在，已定义公司外部 IP 地址的命名和可信位置，可使用该地址限制公司网络之外的访问。

### 任务 2 - 新建限定范围的条件访问策略

成功创建受信任的网络后，现在将使用此网络创建条件访问策略，以限制公司网络之外的访问，并仅限个人用户访问，以便能够测试和防止通过 Entra ID 对公司范围的帐户进行锁定。

1. 你仍应在 Entra ID 门户**https://entra.microsoft.com**中保持登录状态。
2. 在左侧导航窗格上，导航到“**保护**” > “**条件访问**” > “**策略**”。
3. 选择“+ 新建策略”****。
4. 输入名称“**阻止受信任网络之外的访问**”。
5. 选择 **0 个已选用户和组**。
6. 在“**包括**”下，选择“**选择用户和组**”，然后单击“**用户和组**”。
7. 选择“**Allan Deyoung**”作为此策略的唯一测试用户。
8. 选择“**未选择任何目标资源**”，在“**包含**”下，选择“**所有云应用**”。
9. 选择“**已选择 0 个条件**”，然后在“**位置**”下选择“**未配置**”。
10. 选择“**是**”以配置位置条件。
11. 在“**包含**”下，选择“**任意位置**”。
12. 在“**排除**”下，选择“**所有受信任位置**”。
13. 在“**授权**”下选择“**已选择 0 个控件**”，并将其从“**授予访问权限”**切换到“** 阻止访问**”，然后选择页面底部的“**选择**”。
14. 在“**会话**”下，选择“**已选择 0 个控件**”。
15. 启用“**自定义连续访问评估**”，然后选择“**严格强制实施位置策略（预览）**”，然后选择底部的“**选择**”进行确认。
16. 在显示“**启用策略**”的位置，选择“**启用**”，然后选择“**创建**”。

现已创建并启用 CA 策略，以限制受信任网络之外的访问，这仅影响自己的测试用户帐户。

### 任务 3 - 测试配置的策略

由于你已经创建条件访问策略，限制对公司所有云应用程序的访问，因此必须确保访问仍然是可能的。

>[!警报] 为了便于说明，这个任务被大大简化了！
在真实世界的场景中，你将对一个更大、更有代表性的群体进行更长时间的测试，以确保没有不可预见的事件扭曲结果。

1. 使用鼠标右键选择其任务栏图标，然后选择“**新建 InPrivate 窗口**”，在 **Microsoft Edge** 浏览器中打开新的 **InPrivate** 窗口。
1. 选择地址栏，导航到 **`https://portal.microsoft.com`** 并以 **Allan Deyoung**alland@WWLxZZZZZZ.onmicrosoft.com 的身份（其中 ZZZZZZ 是实验室托管提供商提供的唯一租户 ID）登录到 M365 门户。 用户的密码应由实验室托管提供程序提供。
1. 在“保持登录?”对话框上，选中“**不再显示此内容**”复选框，然后选择“**否**”。
1. 登录成功后，可以关闭 **InPrivate** 窗口。
1. 切换回 Edge 浏览器窗口，你仍应登录到 Entra ID 门户 **https://entra.microsoft.com**。
1. 在左侧导航窗格中，导航到“**保护**” > “**条件访问**” > “**监控**” > “**登录日志**”。
1. 选择“**添加筛选器**”，然后按 **Allan Deyoung** 的“**用户**”进行筛选。
1. 选择 **Allan Deyoung** 的最新日志项目。
1. 在“**条件访问**”选项卡下，选择“**阻止受信任网络之外的访问**”。
1. 选择“**用户**”分配，应会看到它通过“**直接分配**”“**匹配**”。
1. 选择“**应用程序**”分配，应会看到它通过“**包含的所有应用程序**”“**匹配**”。
1. 你还应看到，“**位置**”条件“**不匹配**”，因为它位于被排除的受信任网络内。
如果尝试从具有其他外部 IP 地址的网络登录，则此条件将匹配并阻止登录尝试。
1. 关闭“**条件访问策略详细信息**”和“**活动详细信息：登录**”。

你现已成功测试并确保从公司网络内访问所有云应用程序。 你还检查了登录日志，以确保策略按预期工作，并使用正确的分配和条件来限制从公司网络外部访问云应用程序。

### 任务 4 - 公司范围的策略推出

在上一任务中测试成功后，现在可以为整个公司启用该策略。 为此，你将编辑现有策略的用户范围。

>[!警报] 在此任务中执行的操作可能导致帐户锁定！
请确保在实际生产场景中，至少有一个紧急管理员帐户被排除在此策略之外。 

1. 你仍应在 Entra ID 门户**https://entra.microsoft.com**中保持登录状态。
2. 在左侧导航窗格上，导航到“**保护**” > “**条件访问**” > “**策略**”。
3. 选择策略“**阻止受信任网络之外的访问**”。
4. 在“用户”下，选择“**已包含的特定用户**”。
5. 选择“所有用户”。
6. 在窗口底部显示的警告中，选择“**从此策略中排除当前用户admin@WWLxZZZZZZ.onmicrosoft.com**”。
7. 选择“保存”。

现已配置有效的条件访问策略，该策略可防止用户在定义为公司外部 IP 地址的受信任网络之外进行登录。 这是使用有限的用户范围进行测试的，以确保所有云应用程序都始终可访问。 最后，已向所有用户推出 CA 策略。

已成功限制来自受信任网络外部的访问。

### 任务 5 - 需要对 Salesforce 进行 MFA

在此任务中，你将创建 CA 策略，以在登录 Salesforce 时强制实施在上一练习中创建的身份验证强度。 

>[!IMPORTANT] 重要说明：此任务将跳过测试阶段。 在实际应用场景中，将首先使用有限的用户范围进行测试，如前面的任务所示，并在成功测试阶段后全面推出。

1. 你仍应在 Entra ID 门户**https://entra.microsoft.com**中保持登录状态。
2. 在左侧导航窗格上，导航到“**保护**” > “**条件访问**” > “**策略**”。
3. 选择“+ 新建策略”****。
4. 输入名称 **Salesforce 身份验证强度**。
5. 选择 **0 个已选用户和组**。
6. 在“**包括**”下，选择“**选择用户和组**”，然后单击“**用户和组**”。
7. 从 Sales 中选择“**Alex Wilber**”作为策略的唯一测试用户。
8. 选择“**未选择任何目标资源**”，然后在“**包含**”下选择“**选择应用**”。
9. 在“**选择**”下选择“**无**”，然后搜索“**Salesforce**”。
10. 选择“**选择**”确认选择。
11. 在“**授予**”下选择“**已选择 0 个控件**”，并启用“**要求身份验证强度**”。
12. 选择自定义创建的身份验证强度“**强化 MFA**”，并选择“**选择**”按钮进行确认。
13. 现在，使用底部控件条将策略设置为“**打开**”，然后选择“**创建**”。
14. 使用有限的用户范围成功完成测试阶段后，选择“**Salesforce 身份验证强度**”。
15. 在“用户”下，选择“**已包含的特定用户**”。
16. 选择“所有用户”。
17. 选择“保存”。

现已创建 CA 策略，以向 Salesforce 强制实施身份验证强度策略（不包括验证码短信），从而防止使用短信拦截成功攻击。
