# 配置 Entra ID

你是 Contoso Ltd. 新晋升的 IT 安全专家 Allan Deyoung。由于该公司最近收购了 Tailwind Traders，因此你审查了 Entra ID 租户，并决定推出新的安全要求。 你的任务是负责管理任务并实现策略，以满足收购期间提出的要求。

你审查了企业应用程序，并注意到某些用户为第三方应用程序提供了访问其邮箱数据的权限。 这将带来电子邮件通信中数据丢失的潜在风险。 因此，你希望限制此行为，但允许用户登录并共享 Microsoft 已验证网站的登录 ID。 你还需要允许用户使用其 Entra ID 标识请求对新 SaaS 产品的特定访问权限。 

由于合作伙伴组织最近遭到短信拦截攻击，你需要按照 NIST 强制实施身份验证保证。 为此，你将创建身份验证强度策略以停用验证码短信，并限制在组织中使用 AAL1 身份验证方法。 将在 Entra ID 门户中创建此配置。

## 第 1 部分：设计解决方案（必需）

在此任务中，你将设计一个概念来应对 Contoso Ltd. 面临的风险。

### 设计方法

初始步骤包括基于所描述的问题分析要求，理解目标并定义要求。

根据提供的用例，可以概述以下要求：

- 限制来自第三方应用程序的不受控访问
- 允许用户共享已验证服务的登录 ID
- 允许用户请求访问 SaaS 产品
- 提高身份验证强度

第二步，检查 Contoso Ltd. 的现有环境。 Microsoft Entra ID 提供使用 Entra ID 策略管理和限制用户和云应用程序访问权限的解决方案。 调查存在哪些控制措施以及哪些策略已到位。 使用 Entra ID 门户查看当前的配置和策略，并确定是否需要调整，或者是否需要实施新策略。

第三阶段涉及构思解决方案的概念。 经过调查，很明显当前策略均不符合规定的要求。 因此，对 Entra ID 配置进行调整至关重要。

### 建议的解决方案

|要求|解决方案|操作计划|
|----|----|----|
|阻止来自第三方应用程序的不受控访问|Entra ID 应用程序策略|针对来自已验证发布者的应用或已在此组织中注册的应用，限制用户同意向其授予分类为“低影响”的权限。|
|允许用户共享已验证服务的登录 ID|Entra ID 应用程序策略|针对来自已验证发布者的应用或已在此组织中注册的应用，限制用户同意向其授予分类为“低影响”的权限。|
|允许用户请求访问 SaaS 产品|Entra ID 应用程序策略|定义有资格批准可安全使用的应用程序的用户|
|限制使用不安全的身份验证方法|Entra ID 身份验证方法|创建身份验证强度（不包括短信和语音方法）|

## 第 2 部分：实施解决方案（可选）

**备注：** 必须在 M365 实验室配置文件上执行这些实验室步骤。

### 任务 1 - 限制使用第三方应用，仅可使用 Microsoft 已验证的服务

在此任务中，你将限制用户可以向应用程序授予的访问权限级别。 还将添加用户请求其无法允许自己访问的访问权限的功能。 

1. 使用 **lon-sc1\admin** 帐户登录到客户端 1 VM (LON-Sc1)。 密码应由实验室托管提供程序提供。
1. 打开 **Microsoft Edge**，选择地址栏，导航到**`https://entra.microsoft.com`** 并以 **MOD 管理员**身份admin@WWLxZZZZZZ.onmicrosoft.com（其中 ZZZZZZ 是实验室托管服务提供商提供的唯一租户 ID）登录到 Entra ID 门户。 管理员密码应由实验室托管服务提供商提供。
1. 如果系统要求设置多重身份验证，请按照说明操作。
1. 在“保持登录?”对话框上，选中“不再显示此内容”复选框，然后选择“否”  。
1. 选择“**以后再说**”关闭密码保存对话框，以便不在浏览器中保存默认的全局管理员凭据。
1. 在左侧导航窗格上，导航到“**标识**” > “**应用程序**” > “**企业应用程序**” > “**安全**” > “**同意和权限**”。
1. 导航到“**权限分类**”。
1. Entra 建议将**低风险**权限作为最常用权限。
1. 选中所有这些权限，然后选择“**是，添加所选权限**”，以在 Entra ID 租户中将其分类为**低风险**。
1. 导航到“**用户同意设置**”。
1. 在“**应用程序的用户同意**”下，选择建议选项“**允许用户同意来自已验证发布者的应用，以获得所选权限**”。 这样一来，用户同意对来自已验证发布者的应用授予分类为“低影响”（以前选择的）的权限。
1. 选择“保存”。
1. 导航到“**管理员同意设置**”，选择“**是**”启用“管理员同意请求”，以允许用户请求管理员同意无法满足其需求的 APS。
1. 选择“**+ 添加用户**”以将**`Lidia Holloway`** 和**`MOD Administrator`** 添加为可以查看管理员同意请求的用户。
1. 在“**管理员同意设置**”窗口上选择“**保存**”。
1. 将此浏览器标签页保持打开状态，以供后续任务使用。

现已配置应用程序同意设置，以限制每个用户可以授予对应用程序的访问权限。 用户仍然可以请求授予对应用程序的权限同意，应用程序管理员团队可以在评估特定应用程序的需求和风险后批准此同意。

### 任务 2 - 创建身份验证强度

在此任务中，使用 Entra ID 门户创建自己的身份验证强度，以限制在组织中使用验证码短信。 

1. 你仍应在 Entra ID 门户**https://entra.microsoft.com**中保持登录状态。
2. 在左侧导航窗格上，导航到“**保护**” > “**身份验证方法**” > “**身份验证强度**”。
3. 选择“**+ 新建身份验证强度**”。
4. 输入名称**强化 MFA**。
5. 选中“**防钓鱼 MFA**”、“**无密码 MFA**”和“**多重身份验证**”。
6. 在“多重身份验证”下，取消选中以下内容：
   - **临时访问密码（多用途）**
   - **密码 + 短信**
   - **密码 + 语音**
   - **联合单一因素 + 短信**
   - **联合单一因素 + 语音**。
7. 选择**下一步**。
8. 查看并检查身份验证强度中未保留上述因素。
9.  选择**创建**。

现已创建身份验证强度，用于限制将验证码短信用作身份验证因素。
