---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 关于映像模板
{: #about-image-templates}

标准映像模板为所有{{site.data.keyword.virtualmachinesshort}}（与操作系统无关）提供映像选项。
{:shortdesc}

通过标准映像模板，可以捕获现有虚拟服务器的映像，并基于捕获的映像来创建新映像。标准映像模板与裸机服务器不兼容。

## 映像模板的工作方式
对于任何映像模板，两个主要操作是创建和部署。请求创建映像时，{{site.data.keyword.cloud}} 的自动化系统会使服务器脱机。服务器处于脱机状态期间，将创建数据的压缩备份，记录配置信息，并将映像模板存储在 {{site.data.keyword.cloud_notm}} SAN 上。在映像模板的部署阶段，自动化系统会根据从所选映像收集的数据来构造新服务器。部署过程会对卷进行调整，复原复制的数据，然后对新主机进行最终配置更改（例如，网络配置）。

## 专用映像

专用映像是由帐户上的用户创建的映像，或在其他帐户上创建并共享的映像。缺省情况下，创建的所有映像都是专用的。

## 公共映像

公共映像是由 {{site.data.keyword.cloud_notm}} 发布或由客户公开的预配置虚拟服务器，供所有 {{site.data.keyword.cloud_notm}} 客户使用。通过公共映像模板供应的虚拟服务器通常使用不同配置规范的特定组合进行配置，以实现最佳性能。
