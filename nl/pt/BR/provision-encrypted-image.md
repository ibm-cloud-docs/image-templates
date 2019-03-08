---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-21"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Usando a Criptografia End to End (E2E) para provisionar uma instância criptografada

O recurso Criptografia End to End (E2E) permite que você traga a sua própria imagem de sistema operacional criptografada, ativada por cloud-init que você criptografou usando uma chave de criptografia de dados que você possui e controla. Após concluir alguma configuração de ambiente, é possível importar a sua imagem criptografada para o repositório de modelo de imagem e usá-la para provisionar instâncias de servidor virtual criptografadas. A criptografia E2E fornece criptografia de dados em repouso para o armazenamento que está associado a instâncias de servidor virtual provisionadas. Para ganhar acesso a esse recurso, entre em contato com o Suporte.
{:shortdesc}

A Criptografia E2E reúne vários componentes do {{site.data.keyword.cloud}} para fornecer uma solução segura para as suas informações críticas.

* O {{site.data.keyword.keymanagementservicefull_notm}} protege as suas chaves com módulos de segurança de hardware (HSMs) baseados em nuvem certificados FIPS 140-2 Nível 2 que protegem contra o roubo de informações.
* O IBM {{site.data.keyword.iamshort}} (IAM) permite que o serviço Cloud Block Storage acesse o {{site.data.keyword.keymanagementserviceshort}} e a sua chave raiz que é usada para agrupar a sua chave de criptografia de dados.
* O {{site.data.keyword.cos_full_notm}} armazena com segurança a sua imagem criptografada quando você faz upload dela.
* No {{site.data.keyword.slportal}}, é possível importar a sua imagem criptografada e criar um modelo de imagem.
* Com um modelo de imagem criptografada disponível no ambiente de infraestrutura do {{site.data.keyword.cloud_notm}} Console, é possível provisionar instâncias de servidor virtual criptografadas.
* Finalmente, você tem a opção de auditar eventos que estão associados a seus servidores virtuais criptografados por meio do Activity Tracker.

## Preparando o seu ambiente

1. Deve-se ter uma conta atualizada para usar a criptografia E2E para servidores virtuais. Para obter mais informações, consulte [Alternando para IBMid e vinculando contas](/docs/account?topic=account-unifyingaccounts).
2. Use o {{site.data.keyword.keymanagementservicefull_notm}} para criar e gerenciar chaves.
      1. Forneça o serviço [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision).
      2. [Criar](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys) ou [importar](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) uma chave raiz (CRK) no {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Opcional**: se você escolher, será possível [criar](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) ou [importar](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) uma chave padrão para decriptografia.
      4. Obtenha [o acesso à API do {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api) para que seja possível agrupar a chave de criptografia de dados.
      5. [Agrupe a chave de criptografia de dados](/docs/services/key-protect?topic=key-protect-wrap-keys#wrap-keys) com a chave raiz. Você precisará do texto cifrado que está associado à chave de criptografia de dados agrupada (WDEK) ao importar a sua imagem criptografada para o {{site.data.keyword.slportal}}.
3. No IBM {{site.data.keyword.iamshort}} (IAM), [autorize o acesso](/docs/iam?topic=iam-serviceauth#create-an-authorization) entre o **Cloud Block Storage** (serviço de origem) e o {{site.data.keyword.keymanagementservicelong_notm}} (serviço de destino). Os usuários que importam imagens criptografadas do {{site.data.keyword.cos_full_notm}} devem ter uma [política de acesso definida](/docs/iam?topic=iam-userroles) para o {{site.data.keyword.keymanagementservicelong_notm}} no IAM.
4. No IBM Cloud Console, crie uma instância do {{site.data.keyword.cos_full_notm}} e crie um depósito para armazenar dados. Para obter mais informações, consulte [Introdução ao {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-console-#getting-started-console-).
      1. Crie a instância do {{site.data.keyword.cos_full_notm}} no mesmo local regional em que o seu serviço do {{site.data.keyword.keymanagementserviceshort}} é provisionado.
      2. Quando você criar o depósito, a configuração de **Resiliência** deverá ser _Regional_.
      3. Opcionalmente, quando você criar o depósito, será possível [criptografá-lo com a sua chave](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-manage-encryption#sse-kp).   

## Preparando as suas imagens criptografadas

1. Selecione uma imagem não criptografada que funcione no ambiente de infraestrutura do {{site.data.keyword.cloud_notm}} que você deseja criptografar. Uma opção é usar um servidor virtual existente para [criar uma imagem customizada](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template). Para obter mais informações, consulte [Trabalhe com um modelo de imagem criado por meio de um servidor virtual provisionado por cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-a-standard-image-created-from-a-cloud-init-provisioned-virtual-server). Também é possível usar uma imagem VHD existente. Assegure-se de que a imagem atenda aos [requisitos de imagem criptografada](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#encrypted-image-reqs).
2. Se você estiver usando um modelo de imagem do {{site.data.keyword.slportal}}, [exporte a imagem não criptografada](/docs/infrastructure/image-templates?topic=image-templates-exporting-to-ibm-cos) para o {{site.data.keyword.cos_full_notm}}.
3. Faça download do arquivo de imagem por meio do {{site.data.keyword.cos_full_notm}} para uma máquina local segura para criptografar a imagem. Em seu painel de serviço, selecione a ação **Download** para recuperar o seu objeto por meio do armazenamento. É possível usar o plug-in de transferência em alta velocidade do Aspera para fazer download de imagens maiores que 200 MB.
4. Use o formato de criptografia de disco LUKS para [criptografar a sua imagem](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption) usando QEMU e DMCrypt.
5. No {{site.data.keyword.cos_full_notm}}, navegue para o seu depósito e clique em **Incluir objetos** para [fazer upload](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data) da imagem criptografada. É possível usar o plug-in de transferência em alta velocidade do Aspera para fazer upload de imagens maiores que 200 MB.

## Importando uma imagem criptografada e pedindo uma instância

1. Usando o IBM {{site.data.keyword.iamshort}} (IAM), crie um ID de serviço com o qual autenticar ao importar a imagem criptografada para o {{site.data.keyword.slportal}}.
      1. Crie um [ID de serviço](/docs/iam?topic=iam-serviceids#serviceids).
      2. Designe uma [política de acesso](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Designe acesso para esses serviços: {{site.data.keyword.cos_full_notm}} e {{site.data.keyword.keymanagementservicelong_notm}}.
      3. Crie uma [chave de API para um ID de serviço](/docs/iam?topic=iam-serviceidapikeys#creating-an-api-key-for-a-service-id).
      4. Para obter mais informações, consulte [Apresentando IDs de serviço e Chaves de API do {{site.data.keyword.cloud_notm}} IAM ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}.
2. No {{site.data.keyword.slportal}}, [importe a imagem criptografada](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images#import-icos) para a página Modelos de imagem. (Também é possível importar a imagem criptografada usando o [{{site.data.keyword.slapi_short}}](/docs/infrastructure/image-templates?topic=image-templates-importing-an-encrypted-image-by-using-the-softlayer-api).)
3. Na página Modelos de imagem, é possível usar a sua imagem criptografada para [pedir](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template) uma instância de servidor virtual.
4. Com um servidor virtual criptografado provisionado, você tem a opção de auditar os [eventos do servidor virtual](/docs/vsi?topic=virtual-servers-at_events#at_events) por meio do [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov).
