# 终结点安全性

Contoso 使用 Microsoft Intune 管理其设备，并为员工提供 Windows 10 和 Windows 11 设备。 该公司过去已经实现多个配置文件。 然而，公司范围的基础结构分析显示，现有策略是独立创建的，因此很难整体查看和跟踪这些策略。 作为公司的网络安全架构师，你决定将所有必要和关键配置合并到一个位置。 

此外，分析还显示 Tailwind Traders 在其环境中使用 macOS 设备。 作为即将合并的一部分，你计划为新的 macOS 设备准备 Contoso 租户。

## 第 1 部分：设计解决方案（必需）

在此任务中，你将设计一个概念来应对 Contoso Ltd. 面临的挑战。

### 设计方法

在给定应用场景中，可以概述以下要求：

- 将 Windows 设备的所有配置合并到一个位置
- 保护 MacOS 设备

具有安全基线 的Microsoft Endpoint Manager 可集中管理和保护终结点安全，包括台式机、笔记本电脑、移动设备和服务器。 它集成 Intune 和 Configuration Manager 等工具，可实现应用程序的高效部署、策略强制实施、合规性和设备监视。 

安全基线策略包括 Microsoft 建议的一组配置设置，详细说明其安全影响。 这些设置基于 Microsoft 安全工程团队、产品组、合作伙伴和客户的反馈制定。 这些基线包含类似于各种 Intune 策略中的设备配置设置。 自定义每个已部署的基线可强制实施必要的设置和值。 在 Intune 中建立安全基线配置文件实质上是在创建包含多个设备配置文件的模板。 必须根据各自的业务需求定期审查和调整这些建议。

### 建议的解决方案

|要求|解决方案|操作计划|
|----|----|----|
|将 Windows 设备的所有配置合并到一个位置|Microsoft Endpoint Manager - 终结点安全性|部署安全基线策略
|保护 MacOS 设备|Microsoft Endpoint Manager|在 macOS 设备上部署防病毒软件|

## 第 2 部分：实施解决方案（可选）

### 任务 1：部署终结点安全基线策略

总体目标是保护终结点安全并将尽可能多的策略合并到在一个位置。 为此，可以在 Intune 中为 Windows 设备创建终结点安全基线策略。

1. 使用本地**管理员**帐户登录到 Windows 客户端 VM **LON-SC1**。 密码应由实验室托管提供程序提供。
1. 以 **MOD Administrator**admin@WWLxZZZZZZ.onmicrosoft.com（其中 ZZZZZZ 是实验室托管提供商提供的唯一租户 ID）登录到 Microsoft Intune 管理中心 **`https://intune.microsoft.com`**。 用户的密码应由实验室托管提供程序提供。
1. 如果在屏幕右上方看到显示“**管理多重身份验证**”的信息框，请通过选择 **X** 将其关闭。
1. 在 Microsoft Intune 管理中心的左侧导航窗格中，选择“**终结点安全性**”。
1. 在“**终结点安全性 | 概述**”页的“**概述**”下，选择“**安全基线**”。
1. 在“**终结点安全性 | 安全基线**”页上，选择“**Windows 10 及更高版本的安全基线**”。
1. 在“**Windows 10 及更高版本的安全基线 | 配置文件**”页上，选择“**+ 创建配置文件**”。
1. 查看“**创建配置文件**”边栏选项卡上的说明，然后选择“**创建**”。
1. 在“**基本信息**”边栏选项卡上，输入：
    1. 名称：**`Secure Windows Endpoints`**
    1. 说明：**`The Security Baseline for Windows 10 and later represents the recommendations for configuring Windows.`**。
1. 选择**下一步**。
1. 在“**配置设置**”边栏选项卡上，调查不同的配置选项。 完成后，选择“**下一步**”。
1. 在“**作用域标记**”边栏选项卡上，选择“**下一步**”。
1. 在“**分配**”边栏选项卡的“**包含的组**”下，选择“**添加所有用户**”，然后选择“**下一步**”。
1. 在“**查看 + 创建**”边栏选项卡上，选择“**创建**”。
1. 返回到“**终结点安全性 | 安全基线**”页。
1. 在“**终结点安全性 | 安全基线** ”页，选择“**Microsoft Defender for Endpoint 安全基线**”。
1. 在“**Microsoft Defender for Endpoint 安全基线 | 配置文件**”上，选择“**+ 创建配置文件**”。
1. 查看“**创建配置文件**”边栏选项卡上的说明，然后选择“**创建**”。
1. 在“**基本信息**”边栏选项卡上，输入：
    1. 名称：**`Defender for Endpoint Security Baseline`**
    1. 说明：**`Security best practices for the Microsoft security stack on devices managed by Intune`**。
1. 选择**下一步**。
1. 在“**配置设置**”边栏选项卡上，调查不同的配置选项。 完成后，选择“**下一步**”。
1. 在“**作用域标记**”边栏选项卡上，选择“**下一步**”。
1. 在“**分配**”边栏选项卡的“**包含的组**”下，选择“**添加所有用户**”，然后选择“**下一步**”。
1. 在“**查看 + 创建**”边栏选项卡上，选择“**创建**”。

你已成功为 Windows 设备创建了两个安全基线策略。

### 任务 2：在 macOS 设备上部署防病毒软件

利用终结点安全基线策略保护 Windows 设备后，你将在 macOS 设备上部署防病毒软件并启用加密，以便为与 Trailwind Traders 合并准备好环境。

1. 你仍然应该登录到 Microsoft Intune 管理中心 **https://intune.microsoft.com**。
1. 在 Microsoft Intune 管理中心的左侧导航窗格中，选择“**终结点安全性**”。
1. 在“**终结点安全性 | 概述**”页的“**管理**”下，选择“**防病毒软件**”。
1. 在“**终结点安全性 | 防病毒**”页，选择“**+ 创建策略**”。
1. 在“**创建配置文件**”窗格的“**平台**”下，选择“**macOS**”，然后在“**配置文件**”下，选择“**Microsoft Defender 防病毒**”。
1. 选择**创建**。
1. 在“**基本信息**”边栏选项卡上，输入：
    1. 名称：**`Deploy antivirus on macOS devices`**
    1. 说明：**`Deploy antivirus and enable encryption on macOS devices to prepare your environment for merging with Trailwind Traders.`**
1. 选择**下一个**
1. 在“**配置设置**”选项卡上，确保按如下所示配置设置：
1. 在“**云交付保护首选项**”下：
    - 启用/禁用云交付保护：**已启用（默认）**
    - 启用/禁用自动示例提交：**已启用（默认）**
    - 诊断收集级别：**可选（默认值）**
    - 自动安全智能更新：**已启用（默认）**
1. 在“**反病毒引擎**”下：
    - 启用实时保护：**已启用（默认）**
    - 启用被动模式：**已禁用（默认）**
    - 排除合并：**admin_only**
1. 在“**威胁类型设置**”下，选择“**+ 添加**：
    - 在威胁类型的文本框中，输入：**potentially_unwanted_application**
    - 要执行的操作：**阻止**
    - 威胁类型设置合并：**admin_only**
1. 启用文件哈希计算：**True****
1. 更新定义后运行扫描：**已启用（默认值）**
1. 强制执行级别：**real_time**
1. 在“**网络保护**”下：
    - 强制执行级别：阻止
1. 在“**篡改防护**”下：
    - 强制执行级别：**阻止（默认）**
1. 在“**用户界面首选项**”下
    - 控制使用者版本的登录：**已启用（默认）**
    - 显示/隐藏状态菜单图标：**已禁用（默认）**
    - 用户发起的反馈：**已启用（默认）**
1. 选择**下一步**。
1. 在“**作用域标记**”边栏选项卡上，选择“**下一步**”。
1. 在“**分配**”选项卡上的文本框中，输入并选择“**所有用户**”。
1. 选择**下一步**。
1. 在“**查看 + 创建**”选项卡上，选择“**保存**”。

已成功为 macOS 设备配置和部署防病毒软件。

### 任务 3 加密 macOS 设备

在此任务中，你将加密 macOS 设备。

1. 你仍然应该登录到 Microsoft Intune 管理中心 **https://intune.microsoft.com**。
1. 在 Microsoft Intune 管理中心的左侧导航窗格中，选择“**终结点安全性**”。
1. 在“**终结点安全性 | 概述**”页的“**管理**”下，选择“**磁盘加密**”。
1. 在“**终结点安全性 | 磁盘加密**”页，选择“**+ 创建策略**”。
1. 在“**创建配置文件**”窗格的“**平台**”下选择“**macOS**”，然后在“**配置文件**”下选择“**FileVault**”。
1. 选择**创建**。
1. 在“**基本信息**”边栏选项卡上，输入：
    1. 名称：**`Encrypt macOS devices`**
    1. 说明：**`FileVault provides built-in Full Disk Encryption for macOS devices.`**
    1. 选择**下一步**。
1. 在“**配置设置**”边栏选项卡的“**加密**”下，配置以下设置：
   - 启用 FileVault：**是**
   - 个人恢复密钥轮换：**6 个月**
   - 个人恢复密钥的托管位置说明：**`To recover a lost or recently rotated recovery key, log in to the Intune Company Portal website using any device. Navigate to the Devices section within the portal, choose the device with FileVault enabled, and then select the option to retrieve the recovery key. The portal will display the current recovery key for that device.`**
   - 允许绕过的次数：**3**
   - 允许延迟至退出登录：**是**
   - 注销时禁用提示：**是**
   - 隐藏恢复密钥：**是**
   - 选择**下一步**。
1. 在“**作用域标记**”边栏选项卡上，选择“**下一步**”。
1. 在“**分配**”边栏选项卡的“**包含的组**”下，选择“**添加所有用户**”，然后选择“**下一步**”。
1. 在“**查看 + 创建**”边栏选项卡上，选择“**创建**”。

已成功配置并部署 FileVault 配置文件以加密 macOS 设备。
