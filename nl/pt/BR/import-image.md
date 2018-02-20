---
copyright:
  years: 2014, 2018
lastupdated: "2017-10-31"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:screen: .screen}


# Preparando e importando imagens

A tela Modelos de imagem no [Portal do Cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/) permite que os usuários façam upload de uma imagem existente de uma conta do [Object Storage](/docs/infrastructure/objectstorage-swift/index.html) baseada em Swift.
{:shortdesc}

Após as imagens serem importadas como um modelo de imagem, elas poderão ser usadas para provisionar ou iniciar um servidor virtual existente. As imagens que são importadas de uma conta do Object Storage podem ser VHDs ou ISOs customizados. As importações VHD estão restritas aos sistemas operacionais de 64 bits a seguir:

* CentOS 6 e 7
* RedHat Enterprise Linux 6 e 7
* Ubuntu 14.04 e 16.04
* Microsoft Server Standard 2012, R2 2012 e 2016

As importações VHD estão limitadas a discos de 100 GB. Os VHDs devem ser nomeados de acordo com o exemplo a seguir: filename.vhd-0.vhd.

## Convertendo imagens em VHD

O formato VHD é o único formato de imagem de suporte para o {{site.data.keyword.BluVirtServers_full}}. Para converter imagens em VHD, use as informações a seguir:

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

Apenas Sistemas Operacionais Suportados pelo {{site.data.keyword.BluSoftlayer_notm}} podem ser usados para carregar um Modelo ISO em uma VSI. Uma lista de
Sistemas Operacionais Suportados pode ser localizada aqui: [http://www.softlayer.com/services/software/ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.softlayer.com/services/software/)

ISOs que são importados usando essa ferramenta devem ser inicializáveis para que a imagem seja elegível para importação.

## Configurando uma imagem para o {{site.data.keyword.virtualmachinesshort}}

Para assegurar que uma imagem possa ser implementada com êxito no ambiente de infraestrutura do {{site.data.keyword.BluSoftlayer_notm}}, imagens do servidor virtual devem ser configuradas com as especificações a seguir:

* ***/boot*** deve ser a primeira partição
* ***/boot*** e ***/*** devem ser o sistema de arquivos ext3 ou ext4
* ***/etc*** e /root devem estar na mesma partição que o ***/***
* ***/etc/fstab -> LABEL=SWAP-xvdb1 swap swap :*** para montar o disco de troca que está conectado ao sistema
* Wget deve ser instalado
* Ferramentas Xen xe-guest-utilities mais recentes devem ser instaladas. Conclua as etapas a seguir:
    
    1. Faça download do ISO XenServer do Citrix: [https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.citrix.com/downloads/xenserver/product-software/xenserver-72-standard-edition.html)
    
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
    
Para obter mais informações sobre as imagens ativadas para cloud-init, consulte [Fornecimento com uma imagem ativada para cloud-init](image_cloud-init.html).

## Importando uma imagem

Conclua as etapas a seguir para importar uma imagem no Portal do Cliente.

1. Localize e registre os detalhes a seguir para a imagem por meio da conta do {{site.data.keyword.objectstorageshort}}. Para obter mais informações, consulte [Visualizando e editando detalhes do arquivo do Object Storage](/docs/infrastructure/objectstorage-swift/view-and-edit-object-storage-file-details.html).
  * Nome da conta
  * Cluster
  * Contêiner
  * Nome do arquivo da imagem
2. No [Portal do Cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/), acesse a página **Modelos de imagem** selecionando **Dispositivos > Gerenciar > Imagens**.
3. Clique na guia **Importar imagem** para abrir a ferramenta de Importação.
4. Selecione a **Conta do {{site.data.keyword.objectstorageshort}}** para a imagem que você deseja importar da lista suspensa **Conta**.
5. Selecione o **Cluster do {{site.data.keyword.objectstorageshort}}** para a imagem que você deseja importar da lista suspensa **Cluster**.
6. Selecione o **Contêiner do {{site.data.keyword.objectstorageshort}}** para a imagem que você deseja importar da lista suspensa **Contêiner**.
7. Selecione o **Nome do arquivo da imagem** como ele está listado no {{site.data.keyword.objectstorageshort}} na lista suspensa **Arquivo de imagem**.
8. Insira o nome da imagem para o novo modelo de imagem no campo **Nome da imagem**.
9. Insira quaisquer notas aplicáveis na caixa de texto **Notas**.
10. Selecione o sistema operacional da imagem na lista suspensa **Sistema operacional**.

  A lista suspensa Sistema operacional é desativada se a imagem para importação é um ISO customizado. Essa etapa é necessária apenas quando a importação envolve um VHD.
  {:tip}

11. Se a imagem que você está importando estiver ativada para cloud-init, selecione a caixa de seleção **Cloud Init**. Para obter mais informações, consulte [Fornecimento com uma imagem ativada para cloud-init](image_cloud-init.html).        
12. Se você planeja fornecer sua própria licença de sistema operacional, selecione a caixa de seleção **Sua licença**. Para obter mais informações, consulte [Usando Red Hat Cloud Access](use-red-hat-cloud-access.html).
13. Clique em **Importar** para importar a imagem para a tela Modelos de imagem. Clique em **Cancelar** para cancelar a ação.

## Próximas etapas

Após a importação ser iniciada, o sistema localizará o arquivo de imagem na conta do {{site.data.keyword.objectstorageshort}} usando o
caminho especificado (Conta > Cluster > Contêiner > Arquivo de Imagem). O arquivo de imagem é importado como um modelo de imagem que fica, então, acessível
na página Modelos de imagem. Após a importação ser concluída, a imagem poderá ser usada para solicitar um novo dispositivo ou para iniciar um dispositivo existente.
Além disso, a imagem poderá ser excluída a qualquer momento. Os tempos de importação de imagem variam com base no tamanho do arquivo, mas geralmente levam de vários minutos a uma hora.

