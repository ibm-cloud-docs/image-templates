---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:deprecated: .deprecated}
{:new_window: target="_blank"}

# Exportando uma imagem para o OpenStack Swift
{: #exporting-an-image-to-openstack-swift}

Na página Modelos de imagem, é possível exportar um modelo de imagem para uma conta do [Object Storage OpenStack Swift](/docs/infrastructure/objectstorage-swift?topic=infrastructure/objectstorage-swift-getting-started-with-object-storage-openstack-swift#getting-started-with-object-storage-openstack-swift).
{:shortdesc}

Todas as instâncias desse serviço foram descontinuadas. As contas existentes podem ser usadas, mas nenhuma conta nova do {{site.data.keyword.objectstorageshort}} OpenStack Swift pode ser provisionada após 10 de dezembro de 2018. Efetivo em 31 de março de 2019, o IBM Cloud não suporta mais o recurso de importação/exportação de Modelos de imagem com o Cloud Object Storage OpenStack Swift.
{: deprecated}

O processo de exportação de imagem utiliza um modelo de imagem padrão preexistente, privado e converte a imagem em um
arquivo de imagem que é armazenado em um local especificado em uma conta do Object Storage OpenStack Swift. Com o OpenStack Swift, é possível importar imagens somente no formato VHD. Para importar no formato VMDK,
consulte [Exportando uma imagem para o IBM Cloud Object Storage](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage#exporting-an-image-to-ibm-cloud-object-storage)


Use as etapas a seguir para exportar um modelo de imagem.

1. No menu **Dispositivos** no [{{site.data.keyword.slportal_full}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/), selecione **Gerenciar > Imagens**.
2. Clique em **Ações** para o modelo de imagem que você deseja exportar e selecione **Exportar imagem**. Se um modelo de imagem com a configuração que você deseja ainda não estiver
disponível, consulte [Criando um modelo de imagem](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template).
3. Na página Exportar imagem, insira o nome do arquivo para a imagem no campo **Nome do arquivo**.
5. Na lista suspensa **Conta**, selecione uma **Conta do Object Storage**.
6. Na lista suspensa **Cluster**, selecione um **Cluster do Object Storage**.
7. Na lista suspensa **Contêiner**, selecione um **Contêiner do Object Storage**.
8. Clique em **Exportar imagem** para exportar a imagem para o local especificado na Conta do Object Storage. Clique em **Fechar** para cancelar
a ação.

## Próximas etapas

Após você exportar uma imagem, ela permanecerá na lista de modelos de imagem, mas também estará disponível como um arquivo no local do Object Storage OpenStack Swift que foi especificado durante o processo de exportação. Para obter mais informações sobre a visualização de um arquivo que foi
exportado para uma Conta de Object Storage, consulte [Visualizando e editando detalhes do arquivo](/docs/infrastructure/objectstorage-swift?topic=infrastructure/objectstorage-swift-viewing-and-editing-file-details#viewing-and-editing-file-details). Como cada imagem tem tamanho e características diferentes, o processo de exportação poderá
levar vários minutos antes que ser concluído. A velocidade média de exportação é 2 GB/minuto. Se vários minutos expirarem e a imagem ainda não estiver
disponível na Conta do Object Storage OpenStack Swift, [entre em contato com o suporte](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
