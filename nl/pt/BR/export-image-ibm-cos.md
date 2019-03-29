---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-02"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# Exportando uma imagem para o IBM Cloud Object Storage
{: #exporting-an-image-to-ibm-cloud-object-storage}

Por meio da página Modelos de imagem é possível exportar um modelo de imagem para uma conta do [IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage).
{:shortdesc}

O processo de exportação de imagem utiliza um modelo de imagem padrão preexistente, privado ou um modelo de imagem criptografado e converte a imagem em um
arquivo de imagem que é armazenado em um local especificado em uma conta do IBM Cloud Object Storage.

Use as etapas a seguir para exportar um modelo de imagem para o IBM Cloud Object Storage.

1. Autentique para o {{site.data.keyword.slportal}} com um ID de serviço que tenha acesso de gravação para o IBM Cloud Object Storage.
2. No menu **Dispositivos** no [{{site.data.keyword.slportal_full}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/), selecione **Gerenciar > Imagens**.
3. Clique em **Ações** para o modelo de imagem que você deseja exportar e selecione **Exportar imagem para o IBM COS**. Se um modelo de imagem com a configuração que você deseja ainda não estiver
disponível, consulte [Criando um modelo de imagem](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).
4. Preencha os campos necessários (consulte a tabela 1).
5. Clique em **OK** para exportar a imagem para o local especificado na Conta do IBM Cloud Object Storage.

| Campo | Value |
| ----- | ----- |
| Nome do arquivo | Insira o nome do arquivo para a imagem. |
| {{site.data.keyword.cos_full_notm}} | Selecione uma conta do {{site.data.keyword.cos_full_notm}}. |
| Localização | Selecione a região geográfica específica na qual você deseja que o modelo de imagem seja armazenado. |
| Depósito | Selecione o depósito do {{site.data.keyword.cos_full_notm}} no qual você deseja que o modelo de imagem seja armazenado. Somente depósitos que existem no local regional que você selecionou são válidos. Você receberá uma mensagem de erro se você especificar um depósito que não existe no local selecionado. |
| Chave API | Especifique a chave de API do ID de serviço que tem acesso de gravação ao {{site.data.keyword.cos_full_notm}}. Para obter mais informações, consulte [Gerenciando chaves de API do ID de serviço](/docs/iam?topic=iam-serviceidapikeys). |
{: caption="Tabela 1. Valores para exportar uma imagem para o IBM Cloud Object Storage" caption-side="top"}

## Próximas etapas
Como cada imagem tem tamanho e características diferentes, o processo de exportação poderá
levar vários minutos antes que ser concluído.

Após você exportar uma imagem, ela permanecerá na lista de modelos de imagem, mas ela também estará disponível como um arquivo no local do IBM Cloud Object Storage que é especificado durante o processo de exportação.

Quando o seu modelo de imagem é exportado para o IBM Cloud Object Storage, cada dispositivo de bloco (ou disco) tem o seu próprio arquivo associado. Por exemplo, se o seu arquivo de imagem for nomeado image.vhd, o primeiro dispositivo de bloco será exportado como image-0.vhd. O segundo dispositivo de bloco será exportado como image-1.vhd e assim por diante.
{: tip}
