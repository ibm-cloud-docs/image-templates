---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-04"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}


# Usando sua própria licença ou assinatura do S.O.
{: #using-your-own-os-license-or-subscription}

Quando você cria um modelo de imagem com uma imagem do VHD, é possível selecionar para fornecer sua própria licença do sistema operacional RHEL por meio da assinatura do [Red Hat Cloud Access ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) ou uma licença do Windows por meio do Microsoft Enterprise Agreement.
{:shortdesc}

Se você implementar uma imagem no {{site.data.keyword.BluSoftlayer_full}} que indique que você está usando sua própria licença, os termos de suporte a seguir existirão:
* O {{site.data.keyword.IBM_notm}} fornece suporte para hypervisores, fornecendo instâncias, importando imagens, reinicializando uma imagem, recarregando um S.O. e capturando uma imagem.
* A empresa da qual você compra a licença do sistema operacional fornece suporte para a própria imagem. O {{site.data.keyword.IBM_notm}} não fornece suporte para a imagem.

Quando você fornece sua própria licença para uma imagem, as restrições a seguir se aplicam à imagem:
* A imagem é uma imagem privada. Ela não pode ser compartilhada publicamente.
* A imagem não pode ter quaisquer complementos de software incluídos. Software adicional deve ser incluído após a imagem ser provisionada.

## Usando o Red Hat Cloud Access
Para obter mais informações sobre nossa certificação como um provedor em nuvem Red Hat Enterprise Linux, consulte [Infraestrutura como serviço (IaaS) ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://access.redhat.com/ecosystem/cloud-provider/2262101).

## Usando sua própria licença do Windows
Os sistemas operacionais a seguir são suportados:
* Windows Server 2016
* Windows Server 2012
* Windows Server 2012 R2

Se você tiver perguntas sobre a elegibilidade de sua licença do Windows existente ou desejar entender os requisitos de relatório, entre em contato com seu representante Microsoft. Ao criar um modelo de imagem que especifica que você está usando sua própria licença do Windows, deve-se provisionar essa imagem em um host dedicado. Não será possível provisionar uma instância pública ou uma instância dedicada que seja designada automaticamente para um host ao usar uma imagem que indique que você está usando sua própria licença do Windows. Além disso, ao criar ou atualizar um modelo de imagem do Windows que especifica que você está usando sua própria licença, a imagem do Windows não pode ser uma imagem cloud-init.

## Importando uma imagem que designa sua própria licença

É possível importar uma imagem do VHD e especificar que você está fornecendo sua própria licença ou assinatura para o sistema operacional.

Para acessar a página Importar imagem de modelos de imagem e marcar uma imagem do VHD para usar sua própria licença ou assinatura, conclua as etapas a seguir:
1. No menu **Dispositivos**, selecione **Gerenciar > Imagens**.
2. Clique na guia **Importar imagem**.
3. Conclua as informações necessárias para importar sua imagem do VHD e selecione a caixa de seleção **Sua Licença** que é mostrada próxima à
caixa suspensa **Sistema Operacional**. Para obter mais informações sobre como importar imagens, consulte [Preparando e importando imagens](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images).

## Atualizando um modelo de imagem para especificar uma licença de S.O. fornecida pelo usuário

Se você tiver um modelo de imagem do VHD existente, será possível especificar que você deseja fornecer sua própria licença ou assinatura para o sistema operacional.

Para acessar um modelo de imagem e designar que ele use sua própria licença ou assinatura existente, conclua as etapas a seguir:
1. No menu **Dispositivos**, selecione **Gerenciar > Imagens**.
2. Na lista de modelos, clique no nome do modelo de imagem que você deseja atualizar.
3. Na página Detalhes do modelo de imagem, selecione a caixa de seleção **Fornecido pelo usuário** sob o título **Licença do S.O.** e clique em **Atualizar**.
