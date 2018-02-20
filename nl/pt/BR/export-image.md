---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-27"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Exportando uma imagem

Na página Modelos de imagem, é possível exportar um modelo de imagem para uma conta do Object Storage.
{:shortdesc}

O processo de exportação de imagem obtém um modelo de imagem padrão privado pré-existente e converte a imagem em um
arquivo de imagem que é armazenado em um local especificado em uma conta do Object Storage. Use as etapas a seguir para exportar um modelo de imagem.

1. No menu **Dispositivos** no [Portal do Cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/), selecione **Gerenciar > Imagens**.
2. Clique em **Ações** para o modelo de imagem que você deseja exportar e selecione **Exportar imagem**. Se um modelo de imagem com a configuração que você deseja ainda não estiver
disponível, consulte [Criando uma imagem padrão](create-standard-image.html).
3. Na página Exportar imagem, insira o nome do arquivo para a imagem no campo **Nome do arquivo**.
5. Na lista suspensa **Conta**, selecione uma **Conta do Object Storage**.
6. Na lista suspensa **Cluster**, selecione um **Cluster do Object Storage**.
7. Na lista suspensa **Contêiner**, selecione um **Contêiner do Object Storage**.
8. Clique em **Exportar imagem** para exportar a imagem para o local especificado na Conta do Object Storage. Clique em **Fechar** para cancelar
a ação.

## Próximas etapas

Depois de exportar uma imagem, a imagem permanece na lista de modelos de imagem, mas também fica disponível como um arquivo no local do
Object Storage que foi especificado durante o processo de exportação. Para obter mais informações sobre como visualizar um arquivo que foi
exportado para uma Conta do Object Storage, consulte [Visualizando e editando detalhes do arquivo do Object Storage](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html). Como cada imagem tem tamanho e características diferentes, o processo de exportação poderá
levar vários minutos antes que ser concluído. A velocidade média de exportação é 2 GB/minuto. Se decorrerem vários minutos e a imagem ainda não estiver
disponível na Conta do Object Storage, [Entre em contato com o suporte](/docs/get-support/howtogetsupport.html).

