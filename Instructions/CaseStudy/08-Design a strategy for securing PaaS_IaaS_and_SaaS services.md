本案例研究练习旨在提供执行与本模块中学习的主题相关的一些概念设计任务的经验。

## <a name="case-study-securing-paas-iaas-and-saas-services"></a>案例研究：保护 PaaS、IaaS 和 SaaS 服务

Tailwind Traders 是一家虚构的家装零售商。 它在全球范围内线上运营五金零售店。 Tailwind Traders CISO 了解 Azure 提供的机会，但也了解对强大的安全性和可靠的云体系结构的需求。 如果没有强大的安全性和出色的参考体系结构，公司可能难以管理 Azure 环境和成本，难以跟踪和控制。 CISO 有兴趣了解 Azure 如何管理和强制实施安全性标准。

## <a name="requirements"></a>要求

为了实现这一愿景，CIO 聘请了一位新的首席信息安全官 (CISO)。 新的 CISO 开始规划其保护 PaaS、IaaS 和 SaaS 工作负载的策略，作为此策略的一部分，他确定公司需要：

-   实现一个云安全状况管理平台，可为 VM 和容器提供本机漏洞评估，并支持针对 Cosmos DB 的威胁检测
-   为其 Azure 工作负载实现一个数据分类系统，该系统能够对 SQL 数据库和存储帐户中的数据进行分类和标记
-   为 Microsoft 365 中的 SaaS 工作负载实现一个安全基线
-   支持 IoT 工作负载的安全态势管理和威胁检测

## <a name="design-tasks"></a>设计任务

* 应使用哪种解决方案来实现以下目的：
   - 在 Azure 中提供数据分类和标记？
   - 为 VM、容器和 Cosmos DB 提供云安全态势管理和威胁检测？
* 应使用哪种解决方案为 IoT 提供云安全态势管理和威胁检测？

