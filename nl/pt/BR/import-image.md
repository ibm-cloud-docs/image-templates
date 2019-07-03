---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:pre: .pre}
{:screen: .screen}


# Preparando e importando imagens
{: #preparing-and-importing-images}

A tela Modelos de imagem no {{site.data.keyword.slportal_full}} permite importar uma imagem de uma instância de serviço do [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about). É possível importar imagens que estão no formato Virtual Hard Disk (VHD) ou Virtual Machine Disk (VMDK). Após a importação, as imagens VMDK são convertidas em VHD. Após uma imagem ser transferida por upload para um depósito em uma instância de serviço do {{site.data.keyword.cos_full_notm}}, será possível importá-la para o repositório de modelos de imagem no {{site.data.keyword.slportal}}.
{:shortdesc}

Deve-se ter uma conta atualizada para importar imagens do {{site.data.keyword.cos_full_notm}}. Para obter mais informações, veja [Alternando para IBMid e vinculando contas](/docs/account/softlayerlink.html).
{: tip}

Deve-se ter uma [instância do IBM Cloud Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision#provision-account) solicitada por meio do console do {{site.data.keyword.cloud_notm}} (cloud.ibm.com) para usar esse recurso de importação.  O IBM Cloud Object Storage do control.softlayer.com não é suportado.
{: important}

Após as imagens serem importadas como um modelo de imagem, elas poderão ser usadas para provisionar ou iniciar um servidor virtual existente. As imagens importadas de uma instância de serviço do {{site.data.keyword.cos_full_notm}} podem ser VHD, VMDK ou ISOs customizados. As importações de VHD e VMDK estão restritas aos sistemas operacionais de 64 bits a seguir:  

* CentOS 6 e 7
* Microsoft Server Standard 2012, R2 2012 e 2016
* RedHat Enterprise Linux 6 e 7
* Ubuntu 14.04 e 16.04

As importações estão limitadas a discos de 100 GB. As imagens devem ser nomeadas de acordo com o exemplo a seguir: filename.vhd-0.vhd ou filename.vmdk-0.vmdk

## Convertendo imagens em VHD
{: #convert-to-vhd}

Os formatos VHD e VMDK são os únicos formatos de imagem suportados para servidores virtuais. Para converter imagens em VHD de qualquer formato diferente de VMDK, use as informações a seguir:

* O Qemu-img 2.7.0 ou mais recente é necessário
* Converta a imagem com o comando a seguir:

  ```
  qemu-img convert -f <image format> <image name> -O vpc -o force_size <image name>
  ```
  {: pre}

* Exemplo de comando:

  ```
  qemu-img convert -f qcow2 test -O vpc -o force_size test
  ```
  {: pre}

Para obter mais informações, consulte [Convertendo formatos de imagem ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) na
documentação do QEMU.

## Modelos ISO
{: #iso-templates}

Apenas Sistemas Operacionais Suportados pelo {{site.data.keyword.BluSoftlayer_notm}} podem ser usados para carregar um Modelo ISO em uma VSI. Uma lista de
Sistemas operacionais suportados pode ser localizada aqui: [https://www.ibm.com/cloud/server-software ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/server-software)

ISOs que são importados usando essa ferramenta devem ser inicializáveis para que a imagem seja elegível para importação.

## Configurando uma imagem para o {{site.data.keyword.virtualmachinesshort}}
{: #config-image-vsi}

Para assegurar que uma imagem possa ser implementada com êxito no ambiente de infraestrutura do {{site.data.keyword.BluSoftlayer_notm}}, imagens do servidor virtual devem ser configuradas com as especificações a seguir:

* ***/boot*** deve ser a primeira partição
* ***/boot*** e ***/*** devem ser o sistema de arquivos ext3 ou ext4
* ***/etc*** e /root devem estar na mesma partição que o ***/***
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** para montar o disco de troca que está conectado ao sistema
* Wget deve ser instalado
* Ferramentas Xen xe-guest-utilities mais recentes devem ser instaladas. Conclua as etapas a seguir:

    1. Faça download do ISO XenServer por meio do Citrix: [https://www.citrix.com/downloads/citrix-hypervisor/![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.citrix.com/downloads/citrix-hypervisor/)

    2. Monte o ISO executando o comando a seguir:

        ```
        mount -o loop xenserver.iso /mnt
        ```
        {: pre}

    3. Localize o RPM para o ISO executando o comando a seguir:

        ```
        ls -l /mnt/Packages/xenserver-pv*
        ```
        {: pre}

    4. A saída lista um RPM semelhante a:

        ```
        xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm
        ```
        {: screen}

     5. É possível copiar o RPM e, em seguida, extrair o ISO para xentools. Execute o comando a seguir para criar uma estrutura de diretório que hospeda o ISO:

        ```
        rpm2cpio <xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm> | cpio -idv
        ```
        {: pre}

     6. Na estrutura de diretório resultante, é possível montar o ISO *guest-tools* e localizar *rpm/debs* para instalar xentools. Consulte a estrutura de diretório de exemplo a seguir:

        ```
        [root@mysystem user1]# rpm2cpio ../xenserver-pv-tools-7.1.0-137222c.2185.noarch.rpm | cpio -idv
        ./opt/xensource/bin/sr_rescan
        ./opt/xensource/libexec/unmount_halted_xstools.sh
        ./opt/xensource/packages/iso/guest-tools-7.1.0-137222c.iso
        139264 blocks
        [root@mysystem user1]#
        ```
        {: pre}

Para obter mais informações sobre as imagens ativadas para cloud-init, consulte [Fornecimento com uma imagem ativada para cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image#provisioning-with-a-cloud-init-enabled-image).

## Preparando para importar uma imagem criptografada
{: #preparing-to-import-an-encrypted-image}

Se você planeja importar uma imagem de VHD que está criptografada com sua própria chave de criptografia de dados, certifique-se de concluir os pré-requisitos e instruções para criptografia descritos em [Usando a Criptografia End to End (E2E) para provisionar uma instância criptografada](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

Deve-se usar a ferramenta vhd-util para criptografar sua imagem, que deve estar no formato VHD. Para obter mais informações, consulte [Criptografando imagens VHD](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image). 
{: important}

## Fazendo upload de uma imagem para o {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

Quando a sua imagem estiver pronta, será possível fazer upload dela para o {{site.data.keyword.cos_full_notm}}. Certifique-se de usar um depósito em um [local regional](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints).

1. No {{site.data.keyword.cos_full_notm}}, navegue para o seu depósito e clique em **Incluir objetos** para [fazer upload](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) da imagem.
2. Use o plug-in de transferência em alta velocidade do [Aspera](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera) para as velocidades de upload mais rápidas de sua imagem.

É possível usar o SDK do COS com o Aspera para iniciar a transferência em alta velocidade dentro de seus aplicativos customizados ao usar Java, Python ou NodeJS. Para obter mais informações, consulte [Usando bibliotecas e SDKs](/docs/services/cloud-object-storage?topic=cloud-object-storage-aspera#aspera-sdk).
{: tip}


## Importando uma imagem do IBM Cloud Object Storage
{: #import-icos}

Conclua as etapas a seguir para importar uma imagem do {{site.data.keyword.cos_full_notm}}.

1. Navegue até o menu do dispositivo e assegure-se de ter as permissões de conta corretas para concluir as tarefas.

   * Navegue até o menu do dispositivo do console. Para obter mais informações, veja [Navegando até dispositivos](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
   * Assegure-se de ter quaisquer permissões de conta necessárias e de ter acesso ao dispositivo. Somente o proprietário da conta ou um usuário com a permissão de infraestrutura clássica **Gerenciar usuários** poderá ajustar as permissões.

   Para obter mais informações sobre permissões, veja [Permissões de infraestrutura clássica](/docs/iam?topic=iam-    infrapermission#infrapermission) e [Gerenciando o acesso ao dispositivo](/docs/vsi?topic=virtual-servers-managing-device-access).

   Se você estiver importando uma imagem criptografada, deverá usar o console do {{site.data.keyword.cloud_notm}}.
   {: important}
2. Acesse a página **Modelos de imagens** selecionando **Dispositivos > Gerenciar > Imagens**.
3. Clique na guia **Importar imagem do IBM COS** para abrir a ferramenta Importar.
4. Preencha os campos necessários (consulte a Tabela 1).
5. Quando a importação estiver concluída por meio do {{site.data.keyword.cos_full_notm}}, a imagem aparecerá na página Modelos de imagem.

| Campo | Value |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Selecione a instância de serviço do {{site.data.keyword.cos_full_notm}} na qual a imagem que você deseja importar está armazenada. |
| Localização | Selecione a região geográfica específica na qual a sua imagem está armazenada. É possível importar imagens para as regiões e data centers associados a seguir: Sul dos EUA (DAL13), Leste dos EUA (WDC07), Grã-Bretanha - UE (LON02), Alemanha - UE (FRA02), Japão - AP. Após a imagem ser importada para um dos data centers que estiverem listados, será possível movê-la para outro data center. |
| Depósito | Selecione o depósito do {{site.data.keyword.cos_full_notm}} no qual a sua imagem está armazenada. Somente depósitos que existem no local regional que você selecionou são válidos. Você receberá uma mensagem de erro se selecionar um depósito que não existe no local selecionado.|
| Arquivo de imagem | Selecione o arquivo de imagem na instância de serviço do {{site.data.keyword.cos_full_notm}} que você deseja importar. Os tipos de arquivos suportados são VHD (Virtual Hard Disk), VMDK (Virtual Machine Disk) e ISO. Se você estiver importando uma imagem criptografada, a imagem deverá estar no formato de arquivo VHD e criptografada com a ferramenta vhd-util. |
| Nome da imagem | Especifique um nome descritivo para a sua imagem. Essa é a imagem que você usará para provisionar instâncias de servidor virtual. |
| Chave API | Especifique a chave de API que dá acesso à sua instância de serviço do {{site.data.keyword.cos_full_notm}}. Ao importar uma imagem criptografada, a chave de API
também deve ter acesso à sua instância de serviço de gerenciamento de chaves. A chave de API está disponível apenas para ser copiada ou transferida por download no momento da criação. Se a chave API for perdida, uma nova chave API deverá ser criada. Para obter mais informações, veja [Trabalhando com chaves API](/docs/iam?topic=iam-manapikey#manapikey). |
| Sistema Operacional | Selecione o sistema operacional que está incluído em sua imagem. |
| Cloud-init | Se a imagem que você estiver importando for ativada por cloud-init, marque essa caixa de seleção. Se você estiver importando uma imagem que tenha um sistema operacional Windows ativado para cloud-init e selecionar essa opção, não será possível selecionar simultaneamente **Sua licença**. Se você estiver importando uma imagem criptografada, essa opção será selecionada por padrão e não será editável porque a sua imagem criptografada deve ser ativada por cloud-init. |
| Sua licença | Se você planejar fornecer a sua própria licença do sistema operacional, marque essa caixa de seleção. Se você estiver importando uma imagem com um sistema operacional Windows, será possível selecionar essa opção se planejar usar a imagem para provisionar [instâncias de host dedicadas](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances). Se a sua versão do sistema operacional Windows não suportar o uso de sua própria licença, essa opção será desativada. Para imagens do Windows, não será possível selecionar Cloud Init se você especificar que usará a sua própria licença. Se você estiver importando uma imagem criptografada com o Red Hat Enterprise Linux como o seu sistema operacional, essa opção será selecionada por padrão e não será editável porque a sua imagem criptografada deverá incluir a sua própria licença do sistema operacional. |
| Modo de inicialização | Selecione o modo de inicialização para a sua imagem. Se um modo de inicialização padrão for configurado para o sistema operacional que você especificar, esse modo de inicialização será selecionado aqui automaticamente. |
| Notas | Inclua quaisquer notas relacionadas à imagem que possam ser úteis para os usuários. |
| Criptografia | Se você estiver importando uma imagem que foi criptografada com sua própria chave de criptografia de dados usando a ferramenta vhd-util, marque esta caixa de seleção. |
{: caption="Tabela 1. Valores para importar uma imagem do IBM Cloud Object Storage" caption-side="top"}

A tabela a seguir mostra os campos adicionais que são aplicáveis à importação de imagens criptografadas apenas. Para obter mais informações sobre imagens criptografadas, consulte [Usando criptografia End to End (E2E) para provisionar uma instância criptografada](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

<!--
To import an encrypted image, your account must have access to the End to End (E2E) Encryption feature. To enable your account for E2E Encryption, please contact [Support](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
{: tip}
-->

| Campo | Value |
| ----- | ----- |
| Chave de criptografia de dados agrupados | Ao importar uma imagem criptografada, especifique o texto cifrado que está associado à chave de criptografia de dados que você usou para criptografar a sua imagem. Para obter mais informações, consulte [Agrupando chaves usando a API](/docs/services/key-protect?topic=key-protect-wrap-keys#api). |
| Instância de serviço de gerenciamento de chaves | Selecione uma instância de serviço de gerenciamento de chaves em sua conta na lista suspensa. A instância de serviço de gerenciamento de chaves deve
incluir a chave raiz do cliente usada para agrupar sua chave de criptografia de dados. |
| Nome da chave | Selecione o nome da chave raiz dentro da instância de serviço de gerenciamento de chaves usada para agrupar sua chave de criptografia de dados. Para obter mais informações, consulte [Visualizando chaves](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Tabela 2. Valores para importar uma imagem criptografada do IBM Cloud Object Storage" caption-side="top"}

## Próximas etapas

Após a importação iniciar, o sistema localizará o arquivo de imagem na instância de serviço do {{site.data.keyword.cos_full_notm}} no depósito especificado. O arquivo de imagem é importado como um modelo de imagem que fica, então, acessível
na página Modelos de imagem. Após a importação ser concluída, a imagem poderá ser usada para solicitar um novo dispositivo ou para iniciar um dispositivo existente.
Além disso, o modelo de imagem pode ser excluído a qualquer momento. Os tempos de importação de imagem variam com base no tamanho do arquivo, mas geralmente levam de vários minutos a uma hora.

Após uma imagem ser importada para o repositório de modelo de imagem, será possível excluí-la do {{site.data.keyword.cos_full_notm}}. É possível continuar a acessar o modelo de imagem em
sua página **Modelos de imagens** e usá-lo para provisionar instâncias do servidor virtual.
