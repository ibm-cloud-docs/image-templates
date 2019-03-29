---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-17"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Preparando e importando imagens
{: #preparing-and-importing-images}

A tela Modelos de imagem no {{site.data.keyword.slportal_full}} permite que os usuários importem uma imagem de uma instância de serviço do [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage). Após uma imagem ser transferida por upload para um depósito em uma instância de serviço do {{site.data.keyword.cos_full_notm}}, será possível importá-la para o repositório de modelos de imagem no {{site.data.keyword.slportal}}.
{:shortdesc}

Deve-se ter uma conta atualizada para importar imagens do {{site.data.keyword.cos_full_notm}}. Para obter mais informações, veja [Alternando para IBMid e vinculando contas](/docs/account?topic=account-unifyingaccounts).
{: tip}

Após as imagens serem importadas como um modelo de imagem, elas poderão ser usadas para provisionar ou iniciar um servidor virtual existente. As imagens que são importadas de uma instância de serviço do {{site.data.keyword.cos_full_notm}} podem ser VHDs ou ISOs customizados. As importações VHD estão restritas aos sistemas operacionais de 64 bits a seguir:  

* CentOS 6 e 7
* RedHat Enterprise Linux 6 e 7
* Ubuntu 14.04 e 16.04
* Microsoft Server Standard 2012, R2 2012 e 2016

As importações VHD estão limitadas a discos de 100 GB. Os VHDs devem ser nomeados de acordo com o exemplo a seguir: filename.vhd-0.vhd.

## Convertendo imagens em VHD
{: #convert-to-vhd}

O formato VHD é o único formato de imagem suportado para servidores virtuais. Para converter imagens em VHD, use as informações a seguir:

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

    1. Faça download do XenServer ISO por meio do Citrix: [https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.citrix.com/downloads/citrix-hypervisor/product-software/xenserver-76-free-edition.html)

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

Para obter mais informações sobre as imagens ativadas para cloud-init, consulte [Fornecimento com uma imagem ativada para cloud-init](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image).

## Fazendo upload de uma imagem para o {{site.data.keyword.cos_full_notm}}
{: #upload-to-ibm-cos}

Quando a sua imagem estiver pronta, será possível fazer upload dela para o {{site.data.keyword.cos_full_notm}}. Certifique-se de usar um depósito em um [local regional](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#select-regions-and-endpoints).

1. No {{site.data.keyword.cos_full_notm}}, navegue para o seu depósito e clique em **Incluir objetos** para [fazer upload](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-upload-data#uploading-data) da imagem.
2. Use o plug-in de transferência em alta velocidade do [Aspera](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#Aspera-high-speed-transfer) para as velocidades de upload mais rápidas de sua imagem.

É possível usar o SDK do COS com o Aspera para iniciar a transferência em alta velocidade dentro de seus aplicativos customizados ao usar Java, Python ou NodeJS. Para obter mais informações, consulte [Usando bibliotecas e SDKs](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-use-aspera-high-speed-transfer#sdk).
{: tip}


## Importando uma imagem do IBM Cloud Object Storage
{: #import-icos}

Conclua as etapas a seguir para importar uma imagem do {{site.data.keyword.cos_full_notm}}.

1. No [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/), acesse a página **Modelos de imagem** selecionando **Dispositivos > Gerenciar > Imagens**.
2. Clique na guia **Importar imagem do IBM COS** para abrir a ferramenta Importar.
3. Preencha os campos necessários (consulte a Tabela 1).
4. Quando a importação estiver concluída por meio do {{site.data.keyword.cos_full_notm}}, a imagem aparecerá na página Modelos de imagem.

| Campo | Value |
| ----- | ----- |
| {{site.data.keyword.cos_full_notm}} | Selecione a instância de serviço do {{site.data.keyword.cos_full_notm}} na qual a imagem que você deseja importar está armazenada. |
| Localização | Selecione a região geográfica específica na qual a sua imagem está armazenada. É possível importar imagens para as regiões e os data centers associados a seguir: Sul dos EUA (DAL13), Leste dos EUA (WDC07), UE-Grã Bretanha (LON02), UE-Alemanha (FRA02), AP-Japão (TOK02). Após a imagem ser importada para um dos data centers que estiverem listados, será possível movê-la para outro data center. |
| Depósito | Selecione o depósito do {{site.data.keyword.cos_full_notm}} no qual a sua imagem está armazenada. Somente depósitos que existem no local regional que você selecionou são válidos. Você receberá uma mensagem de erro se selecionar um depósito que não existe no local selecionado.|
| Arquivo de imagem | Selecione o arquivo de imagem na instância de serviço do {{site.data.keyword.cos_full_notm}} que você deseja importar. Os tipos de arquivos suportados são VHD, ISO e RAW. Se você estiver importando uma imagem criptografada, a imagem deverá estar no formato de arquivo RAW e criptografada com a criptografia de disco LUKS. |
| Nome da imagem | Especifique um nome descritivo para a sua imagem. Essa é a imagem que você usará para provisionar instâncias de servidor virtual. |
| Chave API | Especifique a chave de API que dá acesso à sua instância de serviço do {{site.data.keyword.cos_full_notm}}. Ao importar uma imagem criptografada, a Chave de API também deverá ter acesso ao Key Protect. A chave de API está disponível apenas para ser copiada ou transferida por download no momento da criação. Se a chave API for perdida, uma nova chave API deverá ser criada. Para obter mais informações, veja [Trabalhando com chaves API](/docs/iam?topic=iam-manapikey). |
| Sistema Operacional | Selecione o sistema operacional que está incluído em sua imagem. Para imagens criptografadas, apenas os sistemas operacionais Linux são seleções válidas. |
| Cloud-init | Se a imagem que você estiver importando for ativada por cloud-init, marque essa caixa de seleção. Se você estiver importando uma imagem que tiver um sistema operacional Windows ativado por cloud-init e selecionar essa opção, também não será possível especificar **Sua licença**. Se você estiver importando uma imagem criptografada, essa opção será selecionada por padrão e não será editável porque a sua imagem criptografada deve ser ativada por cloud-init. |
| Sua licença | Se você planejar fornecer a sua própria licença do sistema operacional, marque essa caixa de seleção. Se você estiver importando uma imagem com um sistema operacional Windows, será possível selecionar essa opção se planejar usar a imagem para provisionar [instâncias de host dedicadas](/docs/vsi?topic=virtual-servers-dedicated-hosts-and-dedicated-instances#dedicated-hosts-and-dedicated-instances). Se a sua versão do sistema operacional Windows não suportar o uso de sua própria licença, essa opção será desativada. Para imagens do Windows, não será possível selecionar Cloud Init se você especificar que usará a sua própria licença. Se você estiver importando uma imagem criptografada com o Red Hat Enterprise Linux como o seu sistema operacional, essa opção será selecionada por padrão e não será editável porque a sua imagem criptografada deverá incluir a sua própria licença do sistema operacional. |
| Modo de inicialização | Selecione o modo de inicialização para a sua imagem. Se um modo de inicialização padrão for configurado para o sistema operacional que você especificar, esse modo de inicialização será selecionado aqui automaticamente. |
| Notas | Inclua quaisquer notas relacionadas à imagem que possam ser úteis para os usuários. |
| Criptografia | A seleção para essa caixa de seleção é determinada pelo tipo de arquivo da imagem que você seleciona para importar. Imagens VHD e ISO indicam que o arquivo de imagem não é criptografado. Portanto, a caixa de seleção não está selecionada para imagens VHD e ISO. Um arquivo de imagem RAW indica que a imagem é uma imagem criptografada. Se um arquivo de imagem RAW for especificado, essa caixa de seleção será selecionada por padrão e não será editável. |
{: caption="Tabela 1. Valores para importar uma imagem do IBM Cloud Object Storage" caption-side="top"}

A tabela a seguir mostra os campos adicionais que são aplicáveis à importação de imagens criptografadas apenas. Para obter mais informações sobre imagens criptografadas, consulte [Usando criptografia End to End (E2E) para provisionar uma instância criptografada](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance).

Para importar uma imagem criptografada, a sua conta deve ter acesso ao recurso de criptografia End to End (E2E). Para ativar a sua conta para Criptografia E2E, entre em contato com o Suporte.
{: tip}

| Campo | Value |
| ----- | ----- |
| ID da instância de serviço do {{site.data.keyword.keymanagementserviceshort}} | Ao importar uma imagem criptografada, a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}} deve estar na mesma região que o seu depósito do {{site.data.keyword.cos_full_notm}}. É possível usar a CLI do {{site.data.keyword.cloud_notm}} para localizar o seu ID da instância do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte [Recuperando o seu ID da instância](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID#retrieve-instance-ID). |
| Chave de criptografia de dados agrupados | Ao importar uma imagem criptografada, especifique o texto cifrado que está associado à chave de criptografia de dados que você usou para criptografar a sua imagem. Para obter mais informações, consulte [Agrupando chaves usando a API](/docs/services/key-protect?topic=key-protect-wrap-keys#api). |
| ID da chave raiz | Ao importar uma imagem criptografada, especifique o ID da chave raiz que foi usada para agrupar a chave de criptografia de dados. Para obter mais informações, consulte [Visualizando chaves](/docs/services/key-protect?topic=key-protect-view-keys#view-keys). |
{: caption="Tabela 2. Valores para importar uma imagem criptografada do IBM Cloud Object Storage" caption-side="top"}

## Próximas etapas

Após a importação iniciar, o sistema localizará o arquivo de imagem na instância de serviço do {{site.data.keyword.cos_full_notm}} no depósito especificado. O arquivo de imagem é importado como um modelo de imagem que fica, então, acessível
na página Modelos de imagem. Após a importação ser concluída, a imagem poderá ser usada para solicitar um novo dispositivo ou para iniciar um dispositivo existente.
Além disso, o modelo de imagem pode ser excluído a qualquer momento. Os tempos de importação de imagem variam com base no tamanho do arquivo, mas geralmente levam de vários minutos a uma hora.

Após uma imagem ser importada para o repositório de modelo de imagem, será possível excluí-la do {{site.data.keyword.cos_full_notm}}. É possível continuar acessando o modelo de imagem por meio da página **Modelos de imagem** e usando-a para provisionar instâncias de servidor virtual.
