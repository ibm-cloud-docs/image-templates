---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-13"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Usando a Criptografia End to End (E2E) para provisionar uma instância criptografada
{: #using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance}

O recurso Criptografia End to End (E2E) permite que você traga a sua própria imagem de sistema operacional criptografada, ativada por cloud-init que você criptografou usando uma chave de criptografia de dados que você possui e controla. Após concluir alguma configuração de ambiente, é possível importar a sua imagem criptografada para o repositório de modelo de imagem e usá-la para provisionar instâncias de servidor virtual criptografadas. A criptografia E2E fornece criptografia de dados em repouso para o armazenamento que está associado a instâncias de servidor virtual provisionadas.
{:shortdesc}

A Criptografia E2E reúne vários componentes do {{site.data.keyword.cloud}} para fornecer uma solução segura para as suas informações críticas.

* Um serviço de gerenciamento de chaves da IBM, tal como o {{site.data.keyword.keymanagementservicelong_notm}} ou o {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}, para proteger
suas chaves de criptografia (veja a Tabela 1).
* O IBM {{site.data.keyword.iamshort}} (IAM) permite que o serviço Cloud Block Storage acesse seu sistema de gerenciamento de chaves e sua chave raiz que é usada para agrupar a chave de criptografia de dados.
* O {{site.data.keyword.cos_full_notm}} armazena com segurança a sua imagem criptografada quando você faz upload dela.
* No console do {{site.data.keyword.cloud_notm}}, é possível importar a imagem criptografada e criar um modelo de imagem.
* Com um modelo de imagem criptografada disponível no ambiente de infraestrutura do console do {{site.data.keyword.cloud_notm}}, é possível provisionar instâncias de servidor virtual criptografadas.
* Por fim, é possível auditar eventos associados a seus servidores virtuais criptografados por meio do [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).

## Serviços de gerenciamento de chaves de criptografia

O {{site.data.keyword.keymanagementserviceshort}} e o {{site.data.keyword.hscrypto}} (agora disponíveis em determinadas [regiões](/docs/services/hs-crypto?topic=hs-crypto-regions#regions)) usam uma API de provedor de chave comum para fornecer uma abordagem consistente para o gerenciamento de chaves de criptografia. Nos bastidores, os data centers {{site.data.keyword.cloud_notm}} fornecem um módulo de segurança de hardware (HSM) dedicado para proteger suas chaves. Você pode escolher a partir das seguintes opções: 

| Key Management Service | Certificação de criptografia HSM |
| ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/concepts?topic=key-protect-getting-started-tutorial#getting-started-tutorial) | Conformidade com o FIPS 140-2 *Nível 2* |
| [{{site.data.keyword.hscrypto}}](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) | Conformidade com o FIPS 140-2 *Nível 4* |
{: caption="Tabela 1. Opções de serviço de gerenciamento de chaves disponíveis" caption-side="top"}

## Preparando o seu ambiente

1. Deve-se ter uma conta atualizada para usar a criptografia E2E para servidores virtuais. Para obter mais informações, veja [Alternando para IBMid e vinculando contas](/docs/account/softlayerlink.html).
2. Use seu serviço de serviço de gerenciamento de chaves para criar e gerenciar chaves. As etapas de exemplo a seguir são específicas para o {{site.data.keyword.keymanagementserviceshort}}, mas o
fluxo geral também é aplicável ao {{site.data.keyword.hscrypto}}. Se você estiver usando o {{site.data.keyword.hscrypto}}, veja a [documentação](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started) desse serviço para obter as instruções correspondentes.
      1. Forneça o serviço [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision).
      2. [Criar](/docs/services/key-protect?topic=key-protect-create-root-keys) ou [importar](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) uma chave raiz (CRK) no {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Opcional**: se você escolher, será possível [criar](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) ou [importar](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) uma chave padrão para decriptografia.      
      4. [Configure o plug-in da CLI do {{site.data.keyword.cloud_notm}} Key Protect](/docs/services/key-protect?topic=key-protect-set-up-cli) para poder [agrupar a chave de criptografia de dados](/docs/services/key-protect?topic=key-protect-cli-reference#kp-wrap) que deseja usar para criptografar sua imagem VHD com a chave raiz. O texto cifrado associado à chave de criptografia de dados agrupada (WDEK) é necessário ao
importar a imagem criptografada para o console do {{site.data.keyword.cloud_notm}}.  
         
       O Key Protect não salva os dados de autenticação adicionais (AAD), mas ainda é
possível usar os AAD para proteger ainda mais uma chave com até 255 sequências, cada uma delimitada
por uma vírgula e contendo até 255 caracteres. Se você fornecer os AAD para o agrupamento de chaves,
salve os dados em um local seguro para assegurar que seja possível acessar e fornecer os mesmos AAD
em futuras solicitações de desagrupamento de chaves.
       {: tip}
      
3. No IBM {{site.data.keyword.iamshort}} (IAM), [autorize o acesso](/docs/iam?topic=iam-serviceauth#create-auth) entre o seu **Cloud Block Storage** (serviço de origem) e seu **Serviço de gerenciamento de chaves** (serviço de destino). Se você importar imagens criptografadas do {{site.data.keyword.cos_full_notm}}, deverá ter uma [política de acesso definida](/docs/iam?topic=iam-userroles#userroles) para o seu serviço de gerenciamento de chaves no IAM.
4. No IBM Cloud Console, crie uma instância do {{site.data.keyword.cos_full_notm}} e crie um depósito para armazenar dados. Para obter mais informações, veja o [Tutorial de introdução do {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)
      1. Crie a instância do {{site.data.keyword.cos_full_notm}} na região em que
o seu serviço de gerenciamento de chaves é provisionado.
      2. Quando você criar o depósito, a configuração de **Resiliência** deverá ser _Regional_.
      3. Opcionalmente, quando você criar o depósito, será possível [criptografá-lo com a sua chave](/docs/services/cloud-object-storage?topic=cloud-object-storage-encryption#encryption-kp).   

## Preparando as suas imagens criptografadas

1. Selecione uma imagem não criptografada que funcione no ambiente de infraestrutura do {{site.data.keyword.cloud_notm}} que você deseja criptografar. Uma opção é usar um servidor virtual existente para [criar um modelo de imagem](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template). Para obter mais informações, consulte [Trabalhe com um modelo de imagem criado por meio de um servidor virtual provisionado por cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server). Também é possível usar uma imagem VHD existente. Assegure-se de que a imagem atenda aos [requisitos de imagem criptografada](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs).
2. Se você estiver usando um modelo de imagem do {{site.data.keyword.slportal}}, [exporte a imagem não criptografada](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage) para o {{site.data.keyword.cos_full_notm}}.
3. Faça download do arquivo de imagem por meio do {{site.data.keyword.cos_full_notm}} para uma máquina local segura para criptografar a imagem. Em seu painel de serviço, selecione a ação **Download** para recuperar o seu objeto por meio do armazenamento. É possível usar o plug-in de transferência em alta velocidade do Aspera para fazer download de imagens maiores que 200 MB.
4. Use a ferramenta vhd-util para [criptografar sua imagem VHD](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image).
5. No {{site.data.keyword.cos_full_notm}}, navegue para o seu depósito e clique em **Incluir objetos** para [fazer upload](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) da imagem criptografada. É possível usar o plug-in de transferência em alta velocidade do Aspera para fazer upload de imagens maiores que 200 MB.

## Importando uma imagem criptografada e pedindo uma instância

1. Usando o IBM {{site.data.keyword.iamshort}} (IAM), crie um ID de serviço com o qual autenticar ao importar a imagem criptografada para o console do {{site.data.keyword.cloud_notm}}.
      1. Crie um [ID de serviço](/docs/iam?topic=iam-serviceids#serviceids).
      2. Designe uma [política de acesso](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Designe o acesso para estes serviços: {{site.data.keyword.cos_full_notm}} e de gerenciamento de chaves.
      3. Crie uma [chave de API para um ID de serviço](/docs/iam?topic=iam-serviceidapikeys#create_service_key).
      4. Para obter mais informações, consulte [Apresentando IDs de serviço e Chaves de API do {{site.data.keyword.cloud_notm}} IAM ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-iam-service-ids-api-keys){: new_window}.
2. No console do {{site.data.keyword.cloud_notm}}, [importe a imagem criptografada](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos) para a página Modelos de imagem.
3. Na página Modelos de imagem, é possível usar a sua imagem criptografada para [pedir](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template) uma instância de servidor virtual.
4. Com um servidor virtual criptografado provisionado, você tem a opção de auditar os [eventos do servidor virtual](/docs/vsi?topic=virtual-servers-at_events#at_events) por meio do [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).
