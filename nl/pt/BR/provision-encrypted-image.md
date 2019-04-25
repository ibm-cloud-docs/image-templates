---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-27"

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

* O {{site.data.keyword.keymanagementservicefull_notm}} protege as suas chaves com módulos de segurança de hardware (HSMs) baseados em nuvem certificados FIPS 140-2 Nível 2 que protegem contra o roubo de informações.
* O IBM {{site.data.keyword.iamshort}} (IAM) permite que o serviço Cloud Block Storage acesse o {{site.data.keyword.keymanagementserviceshort}} e a sua chave raiz que é usada para agrupar a sua chave de criptografia de dados.
* O {{site.data.keyword.cos_full_notm}} armazena com segurança a sua imagem criptografada quando você faz upload dela.
* No console do {{site.data.keyword.cloud_notm}}, é possível importar a imagem criptografada e criar um modelo de imagem.
* Com um modelo de imagem criptografada disponível no ambiente de infraestrutura do console do {{site.data.keyword.cloud_notm}}, é possível provisionar instâncias de servidor virtual criptografadas.
* Finalmente, você tem a opção de auditar eventos associados a seus servidores virtuais criptografados por meio do [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).

## Preparando o seu ambiente

1. Deve-se ter uma conta atualizada para usar a criptografia E2E para servidores virtuais. Para obter mais informações, consulte [Alternando para IBMid e vinculando contas](/docs/account/softlayerlink.html).
2. Use o {{site.data.keyword.keymanagementservicefull_notm}} para criar e gerenciar chaves.
      1. Forneça o serviço [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-provision#provision).
      2. [Criar](/docs/services/key-protect?topic=key-protect-create-root-keys#create-root-keys) ou [importar](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-keys) uma chave raiz (CRK) no {{site.data.keyword.keymanagementservicelong_notm}}.
      3. **Opcional**: se você escolher, será possível [criar](/docs/services/key-protect?topic=key-protect-create-standard-keys#create-standard-keys) ou [importar](/docs/services/key-protect?topic=key-protect-import-standard-keys#import-standard-keys) uma chave padrão para decriptografia.
      4. [Configure a API do Key Protect](/docs/services/key-protect?topic=key-protect-set-up-api#set-up-api) para que seja possível agrupar a chave de criptografia de dados que você pretende usar para criptografar sua imagem VHD.
      5. [Agrupe a chave de criptografia de dados](/docs/services/key-protect/wrap-keys.html#wrap-keys) com a chave raiz. Será necessário o texto cifrado associado à chave de criptografia de dados agrupados (WDEK) ao importar a imagem criptografada para o console do {{site.data.keyword.cloud_notm}}.
3. No IBM {{site.data.keyword.iamshort}} (IAM), [autorize o acesso](/docs/iam?topic=iam-serviceauth#create-auth) entre o **Cloud Block Storage** (serviço de origem) e o **{{site.data.keyword.keymanagementservicelong_notm}}** (serviço de destino). Os usuários que importam imagens criptografadas do {{site.data.keyword.cos_full_notm}} devem ter uma [política de acesso definida](/docs/iam?topic=iam-userroles#userroles) para o {{site.data.keyword.keymanagementservicelong_notm}} no IAM.
4. No IBM Cloud Console, crie uma instância do {{site.data.keyword.cos_full_notm}} e crie um depósito para armazenar dados. Para obter mais informações, consulte [Tutorial de introdução para o {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial).
      1. Crie a instância do {{site.data.keyword.cos_full_notm}} no mesmo local regional em que o seu serviço do {{site.data.keyword.keymanagementserviceshort}} é provisionado.
      2. Quando você criar o depósito, a configuração de **Resiliência** deverá ser _Regional_.
      3. Opcionalmente, quando você criar o depósito, será possível [criptografá-lo com a sua chave](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-sse-kp#sse-kp).   

## Preparando as suas imagens criptografadas

1. Selecione uma imagem não criptografada que funcione no ambiente de infraestrutura do {{site.data.keyword.cloud_notm}} que você deseja criptografar. Uma opção é usar um servidor virtual existente para [criar um modelo de imagem](/docs/infrastructure/image-templates/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template#creating-an-image-template). Para obter mais informações, consulte [Trabalhar com um modelo de imagem criado de um servidor virtual cloud-init provisionado](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#work-with-an-image-template-created-from-a-cloud-init-provisioned-virtual-server). Também é possível usar uma imagem VHD existente. Assegure-se de que a imagem atenda aos [requisitos de imagem criptografada](/docs/infrastructure/image-templates?topic=image-templates-encrypted-image-reqs#encrypted-image-reqs).
2. Se você estiver usando um modelo de imagem do {{site.data.keyword.slportal}}, [exporte a imagem não criptografada](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage) para o {{site.data.keyword.cos_full_notm}}.
3. Faça download do arquivo de imagem por meio do {{site.data.keyword.cos_full_notm}} para uma máquina local segura para criptografar a imagem. Em seu painel de serviço, selecione a ação **Download** para recuperar o seu objeto por meio do armazenamento. É possível usar o plug-in de transferência em alta velocidade do Aspera para fazer download de imagens maiores que 200 MB.
4. Use a ferramenta vhd-util para [criptografar sua imagem VHD](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image).
5. No {{site.data.keyword.cos_full_notm}}, navegue para o seu depósito e clique em **Incluir objetos** para [fazer upload](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload-data#upload-data) da imagem criptografada. É possível usar o plug-in de transferência em alta velocidade do Aspera para fazer upload de imagens maiores que 200 MB.

## Importando uma imagem criptografada e pedindo uma instância

1. Usando o IBM {{site.data.keyword.iamshort}} (IAM), crie um ID de serviço com o qual autenticar ao importar a imagem criptografada para o console do {{site.data.keyword.cloud_notm}}.
      1. Crie um [ID de serviço](/docs/iam?topic=iam-serviceids#serviceids).
      2. Designe uma [política de acesso](/docs/iam?topic=iam-serviceidpolicy#serviceidpolicy). Designe acesso para esses serviços: {{site.data.keyword.cos_full_notm}} e {{site.data.keyword.keymanagementservicelong_notm}}.
      3. Crie uma [chave de API para um ID de serviço](/docs/iam?topic=iam-serviceidapikeys#create_service_key).
      4. Para obter mais informações, consulte [Apresentando IDs de serviço e Chaves de API do {{site.data.keyword.cloud_notm}} IAM ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){: new_window}.
2. No console do {{site.data.keyword.cloud_notm}}, [importe a imagem criptografada](/docs/infrastructure/image-templates?topic=image-templates-import-icos#import-icos) para a página Modelos de imagem.
3. Na página Modelos de imagem, é possível usar a imagem criptografada para [solicitar](/docs/infrastructure/image-templates?topic=image-templates-ordering-an-instance-from-an-image-template#ordering-an-instance-from-an-image-template) uma instância de servidor virtual.
4. Com um servidor virtual criptografado provisionado, você tem a opção de auditar os [eventos do servidor virtual](/docs/vsi?topic=virtual-servers-at_events#at_events) por meio do [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov).
