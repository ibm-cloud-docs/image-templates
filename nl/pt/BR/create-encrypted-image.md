---

copyright:
  years: 2019
lastupdated: "2019-03-27"

keywords: VHD image file, encryption, encrypted image, image

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}


# Criptografando imagens VHD 
{: #create-encrypted-image}

Para usar o recurso Criptografia E2E, deve-se criptografar sua imagem VHD com a ferramenta vhd-util antes de importá-la para os Modelos de imagem para provisionar instâncias criptografadas. Dois níveis de criptografia AES são suportados: AES de 256 bits e AES de 512 bits.
{:shortdesc}

## Requisitos de imagem VHD criptografada
{: #encrypted-image-reqs}

As imagens VHD criptografadas devem atender aos requisitos a seguir:

* VHD formatado.
* Compatibilidade com o ambiente de infraestrutura do console do {{site.data.keyword.cloud}}.
* Provisionado com um [S.O. suportado](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images).
* Cloud-init ativado.
* Criptografado com [a ferramenta vhd-util](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool).

## Criptografando sua imagem VHD
{: #vhd-util-tool}

Siga estas etapas para criar sua imagem VHD criptografada:

1. Selecione um sistema CentOS executando a versão 7 ou superior para criptografar sua imagem do disco virtual (arquivo VHD) para o {{site.data.keyword.cloud_notm}}. Se você não tiver acesso ao hardware físico com o CentOS instalado, será possível provisionar uma instância de servidor virtual com o CentOS 7 dentro do {{site.data.keyword.cloud_notm}} usando um host público ou dedicado. O sistema CentOS usado para criptografar arquivos VHD não precisa ser criptografado.

2. Efetue login no sistema CentOS e conecte-se à VPN do cliente e, em seguida, [acesse o site de download do SoftLayer ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://downloads.service.softlayer.com/citrix/xen/){: new_window} e selecione o arquivo de pacote RPM da ferramenta vhd-util: vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm   

   Se não for possível fazer download do arquivo de pacote RPM diretamente no sistema CentOS, faça download do arquivo para a estação de trabalho na qual está trabalhando atualmente. Será possível, então, fazer upload dele para o sistema CentOS usando o comando de cópia segura (scp). Se você estiver usando uma instância de servidor virtual no {{site.data.keyword.cloud_notm}}, use o endereço IP público do sistema para esse upload usando o comando a seguir.

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. Instale o RPM usando o comando a seguir:

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. Identifique a chave de criptografia AES necessária para criptografar e decriptografar sua imagem de disco e grave-a em um arquivo-chave. Essa chave de criptografia AES é a mesma chave de criptografia de dados que você agrupou com a chave raiz do cliente Key Protect em [Preparando seu ambiente](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment). O material de chave que é gravado em arquivos-chave deve ser desagrupado e não ser codificado. 

   Como o data_key não é codificado em base64 nos arquivos-chave, não é possível imprimir ou visualizar o conteúdo do arquivo-chave por meio da linha de comando usando caracteres ASCII padrão.
   {: tip}

   Use o comando a seguir para criar arquivos-chave com uma chave de criptografia **AES de 256 bits** ou **AES de 512 bits**: 
   
   ```
   echo <data_key> | base64 -d - > <keyfile_name>
   ```
   {: pre} 

   Exemplo de comando:

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. Use o comando a seguir para verificar os arquivos-chave criados na etapa anterior:

   ```
   vhd-util key -C -k <keyfile_name>
   ```
   {: pre}

   Exemplo de comando com saída:

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   A primeira linha da saída do comando de exemplo anterior indica que o arquivo-chave denominado `aes512.dek` contém uma chave de 64 bytes, enquanto os números listados na segunda linha são os hashes SHA256 ou hashes de segurança para as respectivas chaves de criptografia. A saída para arquivos que contêm uma chave de criptografia AES de 256 bits indicará uma chave de 32 bytes.
   {: tip} 

6. Use o comando a seguir para criar cópias criptografadas de seus arquivos VHD. `target_vhd` representa o nome do arquivo que contém a versão criptografada de `source_vhd`.

   ```
   vhd-util copy -n <source_vhd> -N <target_vhd> -k <keyfile_name>
   ```
   {: pre}    

   Exemplo de comando:

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. Verifique se os arquivos VHD estão criptografados usando o comando a seguir.

   ```
   vhd-util key -p -n <vhd_filename>
   ```
   {: pre}

   Exemplo de comando com saída:

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

Se o arquivo VHD estiver criptografado, você verá dois valores de hash na saída, conforme mostrado no exemplo anterior. O primeiro hash contém apenas zeros. O segundo hash é o hash SHA256 que a chave de criptografia AES usa para criptografar e decriptografar o VHD. Certifique-se de que os hashes SHA256 para os arquivos VHD sejam os mesmos que os hashes mostrados na **Etapa 5**.

O comando de exemplo na **Etapa 7** cria um novo arquivo VHD criptografado, que é denominado, “debian8-aes512.vhd”. Ele é criptografado com a chave de criptografia AES de 512 bits do arquivo-chave denominado “aes512.dek”. O hash SHA256 para sua criptografia é `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`.
