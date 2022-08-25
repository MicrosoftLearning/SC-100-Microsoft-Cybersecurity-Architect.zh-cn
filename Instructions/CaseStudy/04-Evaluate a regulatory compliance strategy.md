---
casestudy:
  title: 案例研究：评估监管合规性
  module: 'Module 4: Evaluate a regulatory compliance strategy'
---

本案例研究练习旨在提供执行与本模块中学习的主题相关的一些概念设计任务的经验。

## <a name="case-study-evaluate-regulatory-compliance"></a>案例研究：评估监管合规性

Contoso Pharma is an international pharmaceutical industry with a presence in North America and Europe. Contoso Pharma has workloads on-premises and in Azure. The goal is that in the next two years, all workloads will be fully in Azure and there will be minimum workloads on-premises. Below is a list of their major workloads:

- VM（Windows 和 Linux） 
- 存储帐户
- 密钥保管库
- VM 上的 SQL PaaS 和 SQL 

Contoso Pharma also has a Site-to-Site VPN between the headquarters in Redmond and the main office in London. This VPN is used to allow resources on-premises to communicate.

Contoso Pharma has a legacy environment in Redmond composed by a couple of Windows Server 2012 running a Web Server that is used by the application that queries the database to check for customer's information. Upon investigation it was noted that the communication of the legacy web server with the database is done via HTTP.

### <a name="design-requirements"></a>设计要求

Contoso Pharma 根据其工作负载有不同的合规性需求，如下表所示：

| **“工作负荷”** | **数据类型** | **合规性标准** |
|:---:|:---:|:---:|
| Windows VM | 信用卡持有人信息  | PCI DSS |
| SQL PaaS  | 患者健康状况信息  | HIPAA |
| 存储帐户 | 信用卡持有人信息 | PCI DSS 帐户 |

要符合这些标准，Contoso Pharma 必须能够：

- 监视一段时间内的合规性进展 
- 禁止工作负载所有者预配不符合这些标准的资源 
- 确保环境中部署的新订阅默认使用所需的标准 
- 确保在每个地理位置上预配的资源保留源区域中的数据以实现数据主权目的

### <a name="design-tasks"></a>设计任务

* To ensure that Contoso Pharma can analyze their compliance status over time, which tool should be utilized? Select the most appropriate option.
* 应使用 Azure 中的哪个服务来强制工作负载所有者仅创建符合所需标准的资源？
* 应使用哪个选项来确保当工作负载所有者创建资源时，他们将数据保存在正确的地理位置？
* Contoso Pharma 如何验证预配的 VM 是否符合 PCI DSS，如果不符合，需要做什么来补救？
* Contoso Pharma 是一家国际制药公司，业务遍及北美和欧洲。
* 可以使用哪个 Azure 服务在所有工作负载中强制实施数据加密？
