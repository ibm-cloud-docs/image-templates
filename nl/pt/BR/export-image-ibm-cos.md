---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-16"

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

Na página Modelos de imagem, é possível exportar um modelo de imagem para uma conta do [IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage).
{:shortdesc}

O processo de exportação de imagem obtém um modelo de imagem padrão privado preexistente ou um modelo de imagem criptografada e converte a imagem em um arquivo de imagem que é armazenado em um local especificado em uma conta do IBM Cloud Object Storage.

*Nota:* se você importou uma imagem VMDK, será possível exportar essa imagem no formato VHD ou VMDK. Por causa das diferenças entre os formatos de imagem, há uma chance de perda de dados. Para proteger seus dados no caso de perda de dados, o arquivo VHD original é retido.

Use as etapas a seguir para exportar um modelo de imagem para o IBM Cloud Object Storage.

1. Autentique para o {{site.data.keyword.slportal}} com um ID de serviço que tenha acesso de gravação para o IBM Cloud Object Storage.
2. No menu **Dispositivos** no [{{site.data.keyword.slportal_full}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/), selecione **Gerenciar > Imagens**.
3. Clique em **Ações** para o modelo de imagem que você deseja exportar e selecione **Exportar imagem para o IBM COS**. Se um modelo de imagem com a configuração que você deseja ainda não estiver
disponível, consulte [Criando um modelo de imagem](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template).
4. Preencha os campos necessários (consulte a tabela 1).
5. Clique em **OK** para exportar a imagem para o local especificado na Conta do IBM Cloud Object Storage.

| Campo | Value |
| ----- | ----- |
| Nome do arquivo | Insira o nome do arquivo para a imagem. |
| {{site.data.keyword.cos_full_notm}} | Selecione uma conta do {{site.data.keyword.cos_full_notm}}. |
| Localização | Selecione a região geográfica específica na qual você deseja que o modelo de imagem seja armazenado. |
| Depósito | Selecione o depósito do {{site.data.keyword.cos_full_notm}} no qual você deseja que o modelo de imagem seja armazenado. Somente depósitos que existem no local regional que você selecionou são válidos. Você receberá uma mensagem de erro se você especificar um depósito que não existe no local selecionado. |
| Chave API | Especifique a chave de API do ID de serviço que tem acesso de gravação ao {{site.data.keyword.cos_full_notm}}. Para obter mais informações, consulte [Gerenciando chaves de API do ID de serviço](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys). |
{: caption="Tabela 1. Valores para exportar uma imagem para o IBM Cloud Object Storage" caption-side="top"}
| Tipo de arquivo de destino | Selecione o tipo de arquivo que você deseja que seja exportado. Se você importou uma imagem VMDK, terá a opção de exportar essa imagem no formato VHD ou VMDK. Se o arquivo estava originalmente no formato VHD, será possível exportar apenas como VHD. |

## Próximas etapas
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
Como cada imagem tem tamanho e características diferentes, o processo de exportação poderá
levar vários minutos antes que ser concluído.

Após você exportar uma imagem, ela permanecerá na lista de modelos de imagem, mas ela também estará disponível como um arquivo no local do IBM Cloud Object Storage que é especificado durante o processo de exportação. É possível fazer download do arquivo de imagem no {{site.data.keyword.cos_full_notm}}. Em seu painel de serviço, selecione a ação **Download** para recuperar o seu objeto por meio do armazenamento. É possível usar o plug-in de transferência em alta velocidade do Aspera para fazer download de imagens maiores que 200 MB.

Quando o seu modelo de imagem é exportado para o IBM Cloud Object Storage, cada dispositivo de bloco (ou disco) tem o seu próprio arquivo associado. Por exemplo, se o seu arquivo de imagem for nomeado image.vhd, o primeiro dispositivo de bloco será exportado como image-0.vhd. O segundo dispositivo de bloco será exportado como image-1.vhd e assim por diante.
{: tip}
