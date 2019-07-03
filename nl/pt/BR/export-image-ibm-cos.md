---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

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

Na página Modelos de imagens, é possível exportar um modelo de imagem para uma [conta do {{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about).
{:shortdesc}

O processo de exportação de imagens usa um modelo de imagem padrão privado e preexistente
ou um modelo de imagem criptografado e converte a imagem em um arquivo de imagem que é armazenado
em um local especificado em uma conta do {{site.data.keyword.cos_full_notm}}.

*Nota:* se você importou uma imagem VMDK, será possível exportar essa imagem no formato VHD ou VMDK. Por causa das diferenças entre os formatos de imagem, há uma chance de perda de dados. Para proteger seus dados no caso de perda de dados, o arquivo VHD original é retido.

## Antes
de Começar
Primeiro, navegue até o menu do dispositivo e assegure-se de ter as permissões de conta corretas para concluir as tarefas.

* Navegue até o menu do dispositivo do console. Para obter mais informações, veja [Navegando até dispositivos](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Assegure-se de ter acesso de gravação ao {{site.data.keyword.cos_full_notm}}. Somente o proprietário da conta ou um usuário com a permissão de infraestrutura clássica **Gerenciar usuários** poderá ajustar as permissões.

Para obter mais informações sobre permissões, veja [Permissões de infraestrutura clássica](/docs/iam?topic=iam-infrapermission#infrapermission) e [Gerenciando o acesso ao dispositivo](/docs/vsi?topic=virtual-servers-managing-device-access).

Se você planeja exportar esse modelo de imagem para o {{site.data.keyword.cos_full_notm}}, certifique-se de que seu nome não contenha caracteres que possam ser problemáticos em um
endereço da web. Por exemplo, ?, =, < e outros caracteres especiais que possam causar comportamento indesejado se não codificados por URL.
{: tip}

## Exportando uma imagem para o IBM Cloud Object Storage

Use as etapas a seguir para exportar um modelo de imagem para o IBM Cloud Object Storage.

1. No menu **Dispositivos**, selecione **Gerenciar > Imagens**.
2. Clique em **Ações** para o modelo de imagem que você deseja exportar e selecione **Exportar imagem para o IBM COS**. Se um modelo de imagem com a configuração que você deseja ainda não estiver
disponível, consulte [Criando um modelo de imagem](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template).
3. Preencha os campos necessários (consulte a tabela 1).
4. Clique em **OK** para exportar a imagem para o local especificado
na Conta do {{site.data.keyword.cos_full_notm}}.

| Campo | Value |
| ----- | ----- |
| Nome do arquivo | Insira o nome do arquivo para a imagem. |
| {{site.data.keyword.cos_full_notm}} | Selecione uma conta do {{site.data.keyword.cos_full_notm}}. |
| Localização | Selecione a região geográfica específica na qual você deseja que o modelo de imagem seja armazenado. |
| Depósito | Selecione o depósito do {{site.data.keyword.cos_full_notm}} no qual você deseja que o modelo de imagem seja armazenado. Somente depósitos que existem no local regional que você selecionou são válidos. Você receberá uma mensagem de erro se você especificar um depósito que não existe no local selecionado. |
| Chave API | Especifique a chave de API do ID de serviço que tem acesso de gravação ao {{site.data.keyword.cos_full_notm}}. Para obter mais informações, consulte [Gerenciando chaves de API do ID de serviço](/docs/iam?topic=iam-serviceidapikeys#serviceidapikeys). |
{: caption="Tabela 1. Valores para exportar uma imagem para o {{site.data.keyword.cos_full_notm}}" caption-side="top"}
| Tipo de arquivo de destino | Selecione o tipo de arquivo que você deseja que seja exportado. Se você importou uma imagem VMDK, terá a opção de exportar essa imagem no formato VHD ou VMDK. Se o arquivo estava originalmente no formato VHD, será possível exportar apenas como VHD. |

## Próximas etapas
{: #next-steps-exporting-an-image-to-ibm-cloud-object-storage}
Como cada imagem tem tamanho e características diferentes, o processo de exportação poderá
levar vários minutos antes que ser concluído.

Depois de exportar uma imagem, ela permanecerá na lista de modelos de imagem, mas também estará disponível como um arquivo no local do {{site.data.keyword.cos_full_notm}} que é especificado
durante o processo de exportação. É possível fazer download do arquivo de imagem no {{site.data.keyword.cos_full_notm}}. Na Lista de recursos no console do {{site.data.keyword.cloud_notm}}, acesse sua instância de
serviço do Cloud Object Storage. No depósito em que sua imagem está armazenada, selecione os arquivos
de imagem cujo download você deseja fazer e selecione **Fazer download de objetos**.

Quando o seu modelo de imagem é exportado para o IBM Cloud Object Storage, cada dispositivo de bloco (ou disco) tem o seu próprio arquivo associado. Por exemplo, se o seu arquivo de imagem for nomeado image.vhd, o primeiro dispositivo de bloco será exportado como image-0.vhd. O segundo dispositivo de bloco será exportado como image-1.vhd e assim por diante.
{: tip}
