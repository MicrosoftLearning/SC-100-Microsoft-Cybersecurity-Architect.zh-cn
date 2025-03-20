---
lab: 3
title: 练习 5 - 威胁搜寻
---


# 实验室 3 - 练习 5 - 内容搜索 Microsoft Purview

贵公司在完整性和安全性方面正面临严重威胁。 已检测到大量恶意电子邮件。 你的任务是识别这些电子邮件的发件人地址或主题行。 确定这些恶意电子邮件后，必须将其全部删除。

## 第 1 部分：设计解决方案（必需）

### 设计方法

第一步是识别恶意电子邮件。 因此，对当前邮件流进行分析至关重要。 Microsoft Defender 门户全面概述了所有传出和传入邮件流，以及用于调查此流量的更多功能。 修正操作涉及删除恶意电子邮件。 

### 建议的解决方案

|要求|解决方案|操作计划|
|----|----|----|
|识别恶意电子邮件|Microsoft Defender for Office 365|调查邮件流并分析邮件标头|
|消除恶意电子邮件带来的风险|Microsoft Defender for Office 365|删除恶意邮件|

### 任务 1：调查恶意邮件

你将调查 Microsoft Defender 门户中的传入邮件，并识别哪些邮件可疑且包含恶意内容。

1. 使用 Allan Deyoung 的管理员帐户 **MOD 管理员**并以此身份登录到 Microsoft Defender 门户**https://security.microsoft.com**。
1. 在 Microsoft 安全门户中，展开“**电子邮件和协作**”，然后选择“**资源管理器**”。
1. 在“**资源管理器**”页上，调整查询的时间段以查看过去 30 天的所有邮件流，然后选择“**刷新**”
1. 结果将显示所有传入和传出邮件。 调查邮件流和邮件的主题。
1. 主题为“你今天有任务到期”的邮件似乎可疑。
1. 选择主题进行进一步调查。
1. 在新出现的“**你今天有任务到期**”窗格上，查看提供的信息并选择“**视图标头**”。
1. 在“**电子邮件标头**”窗格上，选择“**复制邮件标头**”，然后选择“**Microsoft 邮件标头分析器**”。
1. 浏览器中将打开一个新选项卡。
1. 在“**邮件标头分析器**”页上，粘贴邮件标头，然后选择“**分析标头**”。
1. 查看结果。

已成功在 Microsoft Defender 门户中审查邮件流。

### 任务 2：执行对恶意邮件的批量删除

你得出的结论是，主题为“你今天有任务到期”的邮件是网络钓鱼攻击。 修正操作涉及使用 Powershell 批量删除这些邮件。

>[!NOTE] 你应该已安装 Exchange Online PowerShell 模块。 如果缺少该模块，请按照安装模块的说明进行操作。

1. 打开提升的 PowerShell 窗口，方法是右键选择 Windows 按钮，然后选择“Windows PowerShell (管理员)”。
1. 选择“是”确认“用户帐户控制”窗口 。
1. 输入以下 cmdlet，安装最新版 Exchange Online PowerShell 模块：

    ```powershell
    Install-Module ExchangeOnlineManagement
    ```
1. 输入表示“是”的 **Y** 并按 **Enter** 键，以确认不受信任的存储库安全性对话框。  此过程可能需要一段时间才能完成。
1. 打开提升的 PowerShell 窗口，方法是右键选择 Windows 按钮，然后选择“Windows PowerShell (管理员)”。
1. 选择“是”确认“用户帐户控制”窗口 。
1. 输入 cmdlet 以连接到安全性与合规性 PowerShell：

    ```powershell
    Connect-IPPSSession
    ```

1. 显示“**登录**”窗口时，以admin@WWLxZZZZZZ.onmicrosoft.com（其中 ZZZZZZ 是实验室托管服务提供商提供的唯一租户 ID）身份登录，并使用租户的管理密码。
1. 输入以下 cmdlet 以执行内容搜索，并在 cmdlet 中指定时间段：

    ```powershell
    $Search=New-ComplianceSearch -Name "Purge phishing messages" -ExchangeLocation All -ContentMatchQuery '(Received:mm/dd/yyyy..mm/dd/yyyy) AND (Subject:"You have tasks due today")'
    Start-ComplianceSearch -Identity $Search.Identity
    ```
1. 输入以下 cmdlet 以硬删除邮件：

    ```powershell
    New-ComplianceSearchAction -SearchName "Purge phishing messages" -Purge -PurgeType HardDelete
    ---
You have successfully performed a mass-deletion of malicious mails.