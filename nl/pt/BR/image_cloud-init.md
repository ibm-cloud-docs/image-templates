---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Fornecimento com uma imagem ativada para cloud-init

Quando você solicita um servidor virtual, muitos sistemas operacionais agora usam uma imagem ativada para cloud-init para otimizar o tempo de fornecimento. Também é possível importar
uma imagem customizada que você tem ativada para cloud-init.
{:shortdesc}

Os sistemas operacionais a seguir agora assumirão o padrão para uma imagem ativada por cloud-init quando você pedir um servidor virtual sem complementos. (Os complementos incluem software adicional, scripts de pós-fornecimento e monitoramento avançado.)
* CentOS 7
* Debian 8, 9
* Ubuntu 16.04, 18.04
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

Quando você solicitar um servidor virtual com um sistema operacional ativado para cloud-init, será possível incluir dados do usuário ou metadados com scripts de fornecimento customizados. No campo Dados do usuário no formulário de pedido, insira dados do usuário cloud-init ou metadados opcionais para o servidor.

## Importar uma imagem ativada para cloud-init customizada

Se você criou uma imagem customizada que está ativada por cloud-init, será possível designá-la como uma imagem de cloud-init na página Importar imagem do
{{site.data.keyword.slportal_full}}.

Para acessar a página Importar imagem de Modelos de imagem e marcar uma imagem como ativada para cloud-init, conclua as etapas a seguir:
1. No menu **Dispositivos**, selecione **Gerenciar** > **Imagens**.
2. Clique na guia **Importar imagem**.
3. Preencha as informações necessárias para importar sua imagem ativada para cloud-init e selecione a caixa de seleção **Cloud init** que é mostrada próxima
à caixa suspensa **Sistema operacional**. Para obter mais informações sobre como importar imagens, consulte Importar uma imagem.

## Marcar um modelo de imagem como ativado para cloud-init

Se você tiver um modelo de imagem VHD existente que seja ativado para cloud-init, será possível designá-lo como ativado para cloud-init na página de detalhes
do modelo de imagem.

Para acessar um modelo de imagem e marcá-lo como ativado para cloud-init, conclua as etapas a seguir:
1. No menu **Dispositivos**, selecione **Gerenciar** > **Imagens**.
2. Na lista de modelos, clique no nome do modelo de imagem que você deseja atualizar.
3. Na página Detalhes do modelo de imagem, selecione a caixa de seleção **Ativado** sob o título **Cloud Init** e clique em **Atualizar**.

## Trabalhe com um modelo de imagem criado por meio de um servidor virtual provisionado por cloud-init

Cloud-init normalmente é executado apenas uma vez. No entanto, se você provisionar um servidor virtual por meio de uma imagem ativada por cloud-init e subsequentemente criar
um modelo de imagem por meio desse servidor virtual, o UUID será registrado. Se esse modelo de imagem for usado para criar outro
servidor virtual, o cloud-init será executado novamente.

## Criar modelos de imagem que são ativados para cloud-init

Para obter informações sobre como configurar imagens, consulte
[Documentação de cloud-init![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloudinit.readthedocs.io/en/latest/).

Para obter informações sobre origens de dados, consulte [Origens de dados ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html). As imagens cloud-init do {{site.data.keyword.cloud_notm}} são criadas para o
ambiente usando a origem de dados [Config Drive ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://cloudinit.readthedocs.io/en/latest/topics/datasources/configdrive.html) - Versão 2 para fornecer os metadados.

### Requisitos do Linux
* Cloud-init versão 0.7.7 ou mais recente

### Requisitos do Windows
* Cloudbase-init Metadata Service para suporte de rede pública e privada na infraestrutura do {{site.data.keyword.cloud_notm}}. O serviço também atualiza o Portal do Cliente com as credenciais do servidor virtual do Windows. É possível acessar o serviço em
[https://github.com/softlayer/bluemix-cloudbase-init ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/softlayer/bluemix-cloudbase-init).
* Se você estiver usando um Vyatta em seu ambiente, configure o Vyatta para permitir chamadas de API para balanceadores de carga de API. Para obter mais informações, consulte [Guia de configuração do Brocade vRouter (Vyatta) para Ambientes do VMware com o File Storage ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/infrastructure/FileStorage?topic=FileStorage-configureVyatta#setting-up-brocade-vrouter-vyatta-for-vmware-environments-with-file-storage).
