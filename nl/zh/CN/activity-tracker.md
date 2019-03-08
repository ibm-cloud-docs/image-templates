---

copyright:
  years: 2018
lastupdated: "2018-08-09"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 使用 Activity Tracker 审计虚拟服务器事件

您可以使用 [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov) 在虚拟服务器实例的整个生命周期内对其进行审计。您必须具有使用在美国南部供应的高端服务套餐的 Activity Tracker 实例。高端套餐授予您对 Kibana 仪表板的访问权，该仪表板具有更多用于过滤和搜索审计日志的选项。

日志在 {{site.data.keyword.cloud}} 帐户所有者的“帐户日志”部分中显示。帐户所有者可以查看以下日志：
* 帐户所有者触发的事件。
* 与帐户链接的用户触发的事件。

可以通过 Activity Tracker 来审计以下虚拟服务器实例事件：
* 供应虚拟服务器实例
* 禁用用于专用或公共接口的端口
* 启用用于专用或公共接口的端口
* 设置端口速度
* 创建映像模板
* 关闭虚拟服务器实例的电源
* 打开虚拟服务器实例的电源
* 重新引导虚拟服务器实例
* 重命名虚拟服务器实例
* 使虚拟服务器实例进入急救方式
* 将安全组添加到虚拟服务器实例的公共或专用接口
* 从虚拟服务器实例的公共或专用接口中除去安全组
* 通过映像装入虚拟服务器实例
* 通过映像引导虚拟服务器实例
* 取消虚拟服务器实例设备

对于与网络接口关联的事件，审计日志不会区分事件是发生在公共接口还是发生在专用接口上。
{: tip}
