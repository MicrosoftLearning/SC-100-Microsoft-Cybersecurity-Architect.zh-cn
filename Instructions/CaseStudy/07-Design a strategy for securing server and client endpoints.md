---
casestudy:
  title: 案例研究：远程访问和终结点策略
  module: 'Module 7: Design a strategy for securing server and client endpoints'
---

本案例研究练习旨在提供执行与本模块中学习的主题相关的一些概念设计任务的经验。

## <a name="case-study-remote-access-and-endpoint-strategy"></a>案例研究：远程访问和终结点策略

Tailwind Traders is a fictitious home improvement retailer. It operates retail hardware stores across the globe and online. The Tailwind Traders CISO is aware of the opportunities offered by Azure, but also understands the need for strong security and solid cloud architecture. Without strong security and a great point of reference architecture, the company may have difficulty managing the Azure environment and costs, which are hard to track and control. The CISO is interested in understanding how Azure manages and enforces security standards.

### <a name="requirements-remote-access"></a>要求：远程访问

新的 CIO 希望确保远程工作人员可以连接到云资源，而无需在其云工作负载上公开管理端口，并确保远程分支机构能够与公司总部始终保持联系。

The CISO understands that in the current threat landscape, most of the attacks are targeting the endpoints. He needs to establish a new security baseline to harden all endpoints and provide a seamless experience to deploy these baselines across the clients. The CISO also wants to empower the SOC Team to perform investigations on the endpoints to better understand the root cause of an attack.

### <a name="design-tasks"></a>设计任务

* **远程访问**： 
     - 应使用哪种解决方案来实现 CIO 关于远程工作人员的连接的愿景？
     - 应该为远程分支机构使用哪种解决方案？
*               终结点策略：
     - 应使用哪种工具来部署安全基线？
     - 如何使 SOC 团队能够对终结点进行调查？
