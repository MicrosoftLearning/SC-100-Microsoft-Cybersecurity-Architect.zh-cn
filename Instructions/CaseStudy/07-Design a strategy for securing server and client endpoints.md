---
casestudy:
  title: 案例研究：远程访问和终结点策略
  module: 'Module 7: Design a strategy for securing server and client endpoints'
---

本案例研究练习旨在提供执行与本模块中学习的主题相关的一些概念设计任务的经验。

## <a name="case-study-remote-access-and-endpoint-strategy"></a>案例研究：远程访问和终结点策略

Tailwind Traders 是一家虚构的家装零售商。 它在全球范围内线上运营五金零售店。 Tailwind Traders CISO 了解 Azure 提供的机会，但也了解对强大的安全性和可靠的云体系结构的需求。 如果没有强大的安全性和出色的参考体系结构，公司可能难以管理 Azure 环境和成本，难以跟踪和控制。 CISO 有兴趣了解 Azure 如何管理和强制实施安全性标准。

### <a name="requirements-remote-access"></a>要求：远程访问

新的 CIO 希望确保远程工作人员可以连接到云资源，而无需在其云工作负载上公开管理端口，并确保远程分支机构能够与公司总部始终保持联系。

CISO 了解到，在当前的威胁形势下，大多数攻击都是针对终结点的。 他需要建立一个新的安全基线来强化所有终结点，并提供在所有客户端部署这些基线的无缝体验。  CISO 还希望使 SOC 团队能够对终结点执行调查，以便更好地了解攻击的根本原因。

### <a name="design-tasks"></a>设计任务

* **远程访问**： 
     - 应使用哪种解决方案来实现 CIO 关于远程工作人员的连接的愿景？
     - 应该为远程分支机构使用哪种解决方案？
*               终结点策略：
     - 应使用哪种工具来部署安全基线？
     - 如何使 SOC 团队能够对终结点进行调查？
