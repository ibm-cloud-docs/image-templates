---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-19"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Exportando uma imagem para o OpenStack Swift

Na página Modelos de imagem, é possível exportar um modelo de imagem para uma conta do [Object Storage OpenStack Swift](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-GettingStarted#getting-started-with-object-storage-openstack-swift).
{:shortdesc}

O processo de exportação de imagem utiliza um modelo de imagem padrão preexistente, privado e converte a imagem em um
arquivo de imagem que é armazenado em um local especificado em uma conta do Object Storage OpenStack Swift. Use as etapas a seguir para exportar um modelo de imagem.

1. No menu **Dispositivos** no [{{site.data.keyword.slportal_full}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/), selecione **Gerenciar > Imagens**.
2. Clique em **Ações** para o modelo de imagem que você deseja exportar e selecione **Exportar imagem**. Se um modelo de imagem com a configuração que você deseja ainda não estiver
disponível, consulte [Criando um modelo de imagem](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).
3. Na página Exportar imagem, insira o nome do arquivo para a imagem no campo **Nome do arquivo**.
5. Na lista suspensa **Conta**, selecione uma **Conta do Object Storage**.
6. Na lista suspensa **Cluster**, selecione um **Cluster do Object Storage**.
7. Na lista suspensa **Contêiner**, selecione um **Contêiner do Object Storage**.
8. Clique em **Exportar imagem** para exportar a imagem para o local especificado na Conta do Object Storage. Clique em **Fechar** para cancelar
a ação.

## Próximas etapas

Após você exportar uma imagem, ela permanecerá na lista de modelos de imagem, mas também estará disponível como um arquivo no local do Object Storage OpenStack Swift que foi especificado durante o processo de exportação. Para obter mais informações sobre a visualização de um arquivo que foi
exportado para uma Conta de Object Storage, consulte [Visualizando e editando detalhes do arquivo](/docs/infrastructure/objectstorage-swift?topic=objectstorage-swift-OSSSLPortal#viewing-and-editing-file-details). Como cada imagem tem tamanho e características diferentes, o processo de exportação poderá
levar vários minutos antes que ser concluído. A velocidade média de exportação é 2 GB/minuto. Se vários minutos expirarem e a imagem ainda não estiver
disponível na Conta do Object Storage OpenStack Swift, [entre em contato com o suporte](/docs/get-support?topic=get-support-getting-customer-support).
