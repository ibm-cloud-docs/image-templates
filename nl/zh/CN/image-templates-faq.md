---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-30"

subcollection: image-templates

---


{:new_window: target="_blank"}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}

# 常见问题：映像模板

## 什么是标准映像模板？
{: faq}

标准映像模板是 {{site.data.keyword.BluSoftlayer_notm}} 的 {{site.data.keyword.virtualmachineslong}} 映像选项。标准映像模板支持用户捕获现有虚拟服务器（与其操作系统无关）的映像，并基于该映像创建新的虚拟服务器。

## 什么是 ISO 模板？
{: faq}

ISO 模板是一种专为可用于引导 VSI 的 ISO 保留的模板类型。ISO 模板有两个版本：公共和专用。公共 ISO 模板是 {{site.data.keyword.BluSoftlayer_notm}} 提供的预配置模板，可供任何客户使用。专用 ISO 模板通过导入{{site.data.keyword.objectstorageshort}}帐户上存储的 ISO 的映像进行创建。ISO 必须可引导，才能导入到“映像模板”屏幕。

仅 {{site.data.keyword.BluSoftlayer_notm}} 支持的操作系统可用于将 ISO 模板装入到 VSI 上。有关更多信息，请参阅 [Supported Operating Systems ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.softlayer.com/services/software/)。
{:tip}

## 公共映像与专用映像有何差别？
{: faq}

公共映像是可由任何 {{site.data.keyword.BluSoftlayer_notm}} 用户查看并应用于新虚拟服务器的映像。目前，{{site.data.keyword.BluSoftlayer_notm}} 将公共映像创建为解决方案，以用于不同设备上的配置选项。客户还可以使映像公开，供任何用户使用。专用映像是只能由授权用户查看的映像。授权用户缺省为您帐户中的任何用户；不过，通过更新 {{site.data.keyword.slportal}} 中的共享选项，映像还可以在多个帐户之间共享。

## 可以使用自我管理的系统管理程序来捕获和部署虚拟服务器吗？
{: faq}

只能捕获和部署 {{site.data.keyword.BluSoftlayer_notm}} 供应的服务器。无法捕获、供应或部署您在个人设备上手动创建的单个虚拟服务器。

## 可以与其他客户共享我的专用 ISO 模板吗？
{: faq}

唯一可向所有客户公开的 ISO 模板是 {{site.data.keyword.BluSoftlayer_notm}} 生成的模板。专用 ISO 模板是特定于帐户的，不能通过 {{site.data.keyword.slportal}}在客户之间共享。

## ISO 模板是否需要与虚拟服务器位于同一数据中心才能完成从映像引导？
{: faq}

是的，ISO 模板必须与虚拟服务器位于同一数据中心才能从映像引导。如果不位于同一数据中心，那么必须将 ISO 模板复制到相应的数据中心才能完成该操作。如果发生这种情况，会出现有关该事务的警告。将 ISO 模板复制到其他数据中心时，会向帐户收取一小笔费用，方式与复制其他类型映像模板时收取的费用一样。

## 物理数据与卷大小有何差别？
{: faq}

卷是可用于存储文件的磁盘空间，而物理数据由存储在磁盘上的实际文件组成。

## 什么是映像导入/导出功能？
{: faq}

位于 {{site.data.keyword.slportal}}的“映像模板”页面上的映像导入/导出功能支持将存储在{{site.data.keyword.objectstorageshort}}帐户上的 VHD 和 ISO 转换为映像模板，或者将映像模板转换为 VHD 和 ISO。导入映像时，特定文件（VHD 或 ISO）将从指定 [{{site.data.keyword.objectstorageshort}}] 帐户的容器中提取，并转换为映像模板。然后可以使用该映像模板来引导或装入设备。导出映像时，映像模板会转换为存储在{{site.data.keyword.objectstorageshort}}帐户的容器上指定位置中的一个文件（如果模板有多个磁盘，那么转换为多个文件）。

## 如何为整个服务器而不仅仅是主驱动器创建映像模板？
{: faq}

要为整个服务器创建映像模板，请参阅[创建映像模板](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template)中的指示信息。

如果选择将映像模板导出到 IBM Cloud Object Storage，那么每个块设备（或磁盘）都会有其自己的关联文件。例如，如果映像文件命名为 image.vhd，那么第一个块设备将导出为 image-0.vhd。第二个块设备导出为 image-1.vhd，依此类推。
{: tip}
