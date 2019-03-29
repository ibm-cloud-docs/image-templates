---

copyright:
  years: 2018
lastupdated: "2018-09-19"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}


# Criando uma imagem criptografada
{: #creating-an-encrypted-image}

Como parte do recurso de Criptografia E2E, é possível criptografar uma imagem para importá-la para os Modelos de imagem e usá-la para implementar uma instância de servidor virtual criptografada.
{:shortdesc}

## Requisitos de imagem criptografada
{: #encrypted-image-reqs}

Uma imagem criptografada que você cria deve atender aos requisitos de imagem a seguir:

* A imagem é compatível com o ambiente de infraestrutura do Console do {{site.data.keyword.cloud}}.
* A imagem inclui um sistema operacional Linux, como o CentOS, o Debian, o Red Hat Enterprise Linux ou o Ubuntu.
* A imagem é ativada por cloud-init.
* A imagem é criptografada com a [criptografia de disco LUKS](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption).

## Usando QEMU e DM-Crypt para criar uma imagem RAW criptografada
{: #luks-disk-encryption}

Para criptografar a sua imagem, deve-se converter um arquivo de imagem VHD fixo no formato RAW. Em seguida, use QEMU e DM-Crypt para criar um novo arquivo de imagem com a criptografia de disco LUKS. Monte o arquivo como um volume criptografado e copie o arquivo de imagem não criptografado para o volume criptografado.

Este procedimento conduz você pelas tarefas a seguir:

* Converta a sua imagem VHD dinâmica em uma imagem VHD fixa.
* Converta a sua imagem VHD fixa no formato de arquivo RAW.
* Crie um arquivo de chave de criptografia de dados que você usará para criptografar a unidade.
* Crie um novo arquivo RAW para conter a imagem, bem como o cabeçalho de criptografia LUKS.
* Formate o arquivo RAW com a criptografia de disco LUKS.
* Anexe o arquivo RAW criptografado a um dispositivo de bloco.
* Copie a imagem não criptografada para o dispositivo de volume criptografado.

Deve-se ter o privilégio para executar comandos usando a autoridade de usuário `root`, por meio de sudo, para montar e desmontar volumes criptografados em seu sistema. É possível emitir o comando a seguir para verificar os seus privilégios de usuário `root`:

```
sudo echo "Hello!"
```
{: pre}

Deve-se receber "Hello!" reproduzido de volta em resposta para concluir essa tarefa de criptografia.

**Dica**: deve-se concluir essa tarefa de criptografia em um sistema que esteja executando um sistema operacional Linux e que tenha os pacotes a seguir disponíveis:
* qemu ou qemu-image (dependendo de seu sistema operacional Linux)
* cryptsetup

Conclua as etapas a seguir para criptografar a sua imagem.

1. Converta o seu arquivo de imagem VHD dinâmico em um arquivo de imagem VHD fixo. Um arquivo VHD fixo evita a distorção que pode ocorrer caso um VHD dinâmico seja convertido diretamente para o formato de arquivo RAW. Execute o comando a seguir para converter o arquivo de imagem VHD dinâmico em um arquivo de imagem VHD fixo:

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <DYNAMIC_VHD_FILE> <FIXED_VHD_FILE>
  ```
  {: pre}

  Por exemplo, se o seu arquivo VHD dinâmico for _Rhel_7.vhd-0.vhd_ e você desejar que o arquivo VHD fixo seja denominado _Rhel_7.fixed.vhd-0.vhd_, execute o comando a seguir:

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  Esse comando pode levar 30 minutos ou mais para ser concluído.
  {: tip}

2. Converta o seu arquivo de imagem VHD fixo para formato de arquivo RAW usando QEMU. A imagem deve estar no formato de arquivo RAW antes que seja possível criptografá-la. Execute o comando QEMU a seguir:

  ```
  qemu-img convert -p -O raw <FIXED_VHD_FILE> <RAW_IMAGE_FILE>
  ```
  {: pre}

  Por exemplo, se o seu arquivo VHD fixo for _Rhel_7.fixed.vhd-0.vhd_ e você desejar o arquivo de saída RAW denominado _Rhel_7.raw-0.raw_, execute o comando a seguir:

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  Esse comando pode levar 30 minutos ou mais para ser concluído.
  {: tip}

3. Identifique a chave de criptografia de dados que você usará para criptografar e decriptografar a unidade. Essa chave de criptografia de dados é a mesma chave que você agrupa e especifica ao importar a imagem criptografada para o {{site.data.keyword.slportal}}. Crie um arquivo que contenha a chave de criptografia de dados que você usará para criptografar e decriptografar a unidade. Nesse arquivo, a chave deve estar desagrupada e em texto codificado em base64. O Base64 ajuda a assegurar que nenhum espaço ou quebra acidental seja incluído. A chave de criptografia de dados codificados em base64 deve ter um mínimo de 32 caracteres ou bytes e um máximo de 512 caracteres ou bytes. O material de chave de criptografia de dados deve estar em uma linha sem quebras de linhas e com nenhuma nova linha. Por exemplo, use o comando a seguir para criar um arquivo chamado `secret.dek` para armazenar o seu `unwrapped_key_material` para a sua chave de criptografia de dados e codifique-o com base64:

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  Mantenha essa chave segura. Se você perder essa chave, não será capaz de decriptografar o seu disco.
  {: tip}

4. Identifique o tamanho correto do novo arquivo RAW a ser criado na etapa a seguir para o disco criptografado, levando em conta a adição de um cabeçalho LUKS. O novo arquivo RAW será usado por _dmcrypt_ e _cryptsetup_ para guardar o conteúdo do disco em um formato LUKS criptografado. O tamanho do novo arquivo RAW deve ser a soma do tamanho do arquivo RAW criado na etapa 2 e um cabeçalho LUKS constante de 4 MB. Para determinar o tamanho de sua imagem existente do RAW, execute o comando a seguir, em que _Rhel_7.raw-0.raw_ é o nome da imagem:

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  Você verá uma saída semelhante à seguinte:

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  Em que _42949017600_ é o número de bytes que a imagem RAW está usando
      1. Inclua 4 MB (convertidos em bytes) no tamanho do arquivo de imagem RAW para levar em conta o cabeçalho LUKS. Por exemplo, 42949017600 + (4 x 1024 x 1024) = 42953211904 bytes.
      2. Converta a soma do tamanho da imagem RAW e o cabeçalho LUKS em megabytes e arredonde para o próximo megabyte. Por exemplo, 42953211904 bytes/1024 bytes/1024 kilobytes = 40963,375 megabytes = **40964 MG**.

5. Crie um novo arquivo RAW com o número correto de bytes que se tornará a imagem RAW criptografada. Use o comando _dd_ a seguir para criar o arquivo RAW:

  ```
  dd if=/dev/zero of=<ENCRYPTED_RAW_FILENAME> bs=1M count=<TARGET_SIZE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  Em que _ENCRYPTED_RAW_FILENAME_ é o arquivo que se tornará a imagem RAW criptografada e _TARGET_SIZE_IN_MEGABYTE_ é o número que você obteve na etapa 4, o tamanho da imagem RAW não criptografada mais o cabeçalho LUKS.

  Por exemplo, se você desejar que o seu novo arquivo RAW que se tornará a imagem criptografada seja _Rhel_7.encrypted.raw_ e o seu tamanho de destino seja de _40964_ MB, execute o comando a seguir:

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. Formate o arquivo RAW com a criptografia de disco LUKS usando _dmcrypt_ e a sua chave de criptografia de dados (da etapa 3). Esta etapa prepara o arquivo para conter a imagem. Para formatar o arquivo com criptografia LUKS, execute o comando _cryptsetup_ a seguir:

  ```
  cryptsetup -v luksFormat <ENCRYPTED_RAW_FILENAME> <DEK_FILENAME>
  ```
  {: pre}

  Em que _ENCRYPTED_RAW_FILENAME_ é o nome do arquivo RAW que você deseja criptografar e _DEK_FILENAME_ é o nome do arquivo no qual a chave de criptografia de dados está armazenada.

  Por exemplo, se o seu arquivo RAW for _Rhel_7.encrypted.raw_ e o arquivo-chave for _secret.dek_, insira:

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  Após executar o comando cryptsetup, você receberá o prompt a seguir. Esse prompt é esperado e deve-se responder com YES para continuar.

  ```
  WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. Anexe o arquivo RAW criptografado como um volume para associar um novo dispositivo de bloco ao sistema operacional. Usando o _cryptsetup_, execute o comando a seguir para abrir o dispositivo RAW criptografado usando a opção luksOpen.

  Deve-se ter o privilégio sudo para executar o comando a seguir usando a autoridade de usuário `root`.
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <ENCRYPTED_RAW_FILENAME> <VOLUME_NAME> --key-file <DEK_FILENAME>
  ```
  {: pre}

  Em que _ENCRYPTED_RAW_FILENAME_ é o nome do arquivo RAW criptografado, _VOLUME_NAME_ é o nome do dispositivo que o _dmcrypt_ tentará criar e _DEK_FILENAME_ é o nome do arquivo de sua chave de criptografia de dados.

  Por exemplo, se o seu arquivo RAW criptografado for _Rhel_7.encrypted.raw_, você desejar nomear o dispositivo de bloco _encryptedVolume_ e o arquivo-chave de criptografia de dados for _secret.dek_, use o comando a seguir:

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  Quando essa etapa for concluída, um novo dispositivo de bloco será criado, no qual você pode ler e gravar em _/dev/mapper/_.

8. Confirme se o mapeador de volume LUKS foi criado com êxito executando o comando a seguir:

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  É necessário ver uma saída semelhante ao exemplo a seguir:

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. Copie a imagem não criptografada (da etapa 2) para o dispositivo de volume criptografado (da etapa 7). Execute o comando _dd_ a seguir para copiar a imagem não criptografada para o volume criptografado:

  ```
  sudo dd if=<RAW_IMAGE_FILE> of=/dev/mapper/<VOLUME_NAME>
  ```
  {: pre}

  Em que _RAW_IMAGE_FILE_ é o nome do arquivo da imagem RAW não criptografada (criada na etapa 2) e _VOLUME_NAME_ é o nome do dispositivo de bloco de volume criptografado (criado na etapa 7).

  Por exemplo, se o seu RAW_IMAGE_FILE for denominado _Rhel_7.raw-0.raw_ e o seu VOLUME_NAME for _encryptedVolume_, execute o comando a seguir:

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  Esse comando pode levar 30 minutos ou mais para ser concluído.
  {: tip}

10. Destrua o mapeador de volume e feche a conexão LUKS com o arquivo de dados criptografado:

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  Em que _encryptedVolume_ é o nome do dispositivo de bloco de volume criptografado.

  O seu _ENCRYPTED_RAW_FILENAME_ está agora inicializado e é possível fazer upload dele para o IBM Cloud Object Storage. Por exemplo, se o seu arquivo RAW criptografado for _Rhel_7.encrypted.raw_, faça upload dessa imagem para o IBM Cloud Object Storage.
