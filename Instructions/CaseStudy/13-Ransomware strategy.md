---
casestudy:
  title: 案例研究：勒索软件策略
  module: 'Module 13: Recommend a ransomware strategy by using Microsoft Security Best Practices'
---
本案例研究练习旨在提供执行与本模块中学习的主题相关的一些概念设计任务的经验。

## 案例研究：使用 Microsoft 安全最佳做法推荐勒索软件策略
 
CONTOSO 是一家医疗服务公司，它的主要办事处位于芝加哥和英国伦敦。  
这家公司在它位于芝加哥和英国伦敦大都会区的机构为客户提供医疗保健服务。  Contoso 机构涵盖多家医院，医院有医生、护士和其他医疗保健专业人员。 Contoso 已经开始在公司范围将所有关键服务迁移到云。 本次迁移包括本地服务器、VM 以及支持的管理和监视设备。

Contoso 启动了云迁移，但由于合规性和监管要求，一些数据和应用程序需要在本地托管。 Contoso 有一个 Azure Active Directory (Azure AD) 租户。 此 Azure AD 与在本地域控制器上维护的 Active Directory 域服务 (AD DS) 同步。 随着迁移的进行，将根据需要为每个部门创建额外的 Azure 订阅，其中包括 SaaS 应用程序。 为了方便日常工作，大多数员工都可以访问 Microsoft 365 应用程序。  
 
最近发生的多起备受瞩目的勒索软件案件涉及 Contoso 在医疗服务市场的竞争对手。 CEO 和 CISO 担心 Contoso 还没有制定计划来缓解勒索软件攻击的风险。 CISO 亲自要求你起草一份勒索软件响应和缓解计划，并在两周内提交给公司高管。 一些更资深的 IT 员工表示，勒索软件威胁被夸大了，公司真正需要的是良好的备份和可靠的外围安全性。
 
### 要求

* 勒索软件的响应和缓解计划不仅必须包括关键的本地基础结构和云服务，还必须包括存储在公司笔记本电脑和现场员工使用的移动设备上的公司数据
* CEO 和 CISO 采取了强硬的态度，他们绝不会与勒索软件黑客谈判或付费给他们。 他们表示，在遭到勒索软件攻击时，他们的目标是在 12 小时内让关键系统恢复运行，并在 48 小时内恢复全部功能。
* 数据处理策略必须符合隐私要求，包括美国的 HIPAA 和英国的任何相关标准。
* 你的计划还必须详细说明为什么良好的备份不足以缓解来自勒索软件的威胁。

### 初始问题

* 哪些部门和人员需要参与制定勒索软件缓解计划？ 
* 计划需要解决哪些服务和基础结构元素的问题？ 
* 制定可靠的勒索软件响应计划的实际时间线是什么？
* 实施勒索软件防护计划的主要成功措施有哪些？

### 设计任务

* 列出 Azure 和 M365 服务对于备份和保护数据至关重要。 解释为什么每项服务都是勒索软件响应的重要组成部分，以及每项服务的权衡。
* 记下公司现有 Azure 和 M365 解决方案无法满足的任何特殊要求。
* 列出计划需要解决的服务和基础结构元素的问题。
* 估计一些实际的时间线，以便制定可靠的勒索软件响应计划，然后执行该计划。 