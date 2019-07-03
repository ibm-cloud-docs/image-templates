---

copyright:
  years: 2019
lastupdated: "2019-04-29"

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


# Cifrado de imágenes VHD 
{: #create-encrypted-image}

Para utilizar la función de cifrado E2E, debe cifrar la imagen VHD con la herramienta vhd-util antes de importarla a Plantillas de imagen para suministrar instancias cifradas. Se admiten dos niveles de cifrado AES: AES de 256 bits y AES de 512 bits.
{:shortdesc}

## Requisitos de imagen VHD cifrada
{: #encrypted-image-reqs}

Las imágenes VHD cifradas deben cumplir los requisitos siguientes:

* Estar en formato VHD.
* Ser compatibles con el entorno de infraestructura de la consola de {{site.data.keyword.cloud}}.
* Estar suministradas con un [sistema operativo admitido](/docs/infrastructure/image-templates/?topic=image-templates-preparing-and-importing-images#preparing-and-importing-images).
* Estar habilitadas para cloud-init.
* Estar cifradas con [la herramienta vhd-util](/docs/infrastructure/image-templates?topic=image-templates-create-encrypted-image#vhd-util-tool).

## Cifrado de la imagen VHD
{: #vhd-util-tool}

Siga estos pasos para crear la imagen VHD cifrada:

1. Seleccione un sistema CentOS que ejecute la versión 7 o superior para cifrar la imagen de disco virtual (archivo VHD) para {{site.data.keyword.cloud_notm}}. Si no tiene acceso a hardware físico con CentOS instalado, puede suministrar una instancia de servidor virtual con CentOS 7 dentro de {{site.data.keyword.cloud_notm}} mediante un host público o dedicado. No es necesario que el sistema CentOS utilizado para cifrar archivos VHD esté también cifrado.

2. Inicie sesión en el sistema CentOS y conéctese a la VPN de cliente; a continuación, [vaya al sitio de descarga de SoftLayer ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://downloads.service.softlayer.com/citrix/xen/){: new_window} y seleccione el archivo de paquete RPM de la herramienta vhd-util: vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm   

   Si no puede descargar el archivo de paquete RPM directamente en el sistema CentOS, descargue el archivo a la estación de trabajo en la que esté trabajando en ese momento. A continuación, puede cargarlo al sistema CentOS mediante el mandato de copia segura (scp). Si está utilizando una instancia de servidor virtual en {{site.data.keyword.cloud_notm}}, utilice la dirección IP pública del sistema para esta carga mediante el mandato siguiente.

   ```
   scp vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm root@<vsi_public_ip>:
   ```
   {: pre}

3. Instale el RPM mediante el mandato siguiente:

   ```
   rpm -iv vhd-util-standalone-3.5.0-xs.2+1.0_71.2.2.x86_64.rpm
   ```
   {: pre}

4. Identifique la clave de cifrado AES que necesita para cifrar y descifrar la imagen de disco y grábela en un archivo de claves. Esta clave de cifrado AES es la misma clave de cifrado de datos que ha envuelto con su
clave raíz de cliente proporcionada por el servicio de gestión de claves en [Preparación del entorno](/docs/infrastructure/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#preparing-your-environment). El material de claves que se graba en archivos de claves debe estar desenvuelto y no codificado. 

   Puesto que data_key no está codificado con base64 en los archivos de claves, no puede imprimir ni ver el contenido del archivo de claves desde la línea de mandatos mediante caracteres ASCII estándar. 
   {: tip}

   Utilice el mandato siguiente para crear archivos de claves con una clave de cifrado **AES de 256 bits** o **AES de 512 bits**: 
   
   ```
   echo <data_key> | base64 -d - > <keyfile_name>
   ```
   {: pre} 

   Mandato de ejemplo:

   ```
   echo Nrfen98EpMxF2B+wdgLfagzrqvgUZfMK4vL2T0NsT20ihrsNC9pUUHtizF6218pze8RLCgQ6kwxuE58IWLzgDA== | base64 -d - > aes512.dek
   ```
   {: screen}

5. Utilice el mandato siguiente para verificar los archivos de claves que ha creado en el paso anterior:

   ```
   vhd-util key -C -k <keyfile_name>
   ```
   {: pre}

   Mandato de ejemplo con salida:

   ```
   vhd-util key -C -k aes512.dek
   vhd_util_read_key: using keyfile aes512.dek, Size (bytes) 64
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

   La primera línea de la salida del mandato de ejemplo anterior indica que el archivo de claves denominado `aes512.dek` contiene una clave de 64 bytes, mientras que los números listados en la segunda línea son los hashes SHA256 o los hashes de seguridad de las respectivas claves de cifrado. La salida de los archivos que contengan una clave de cifrado AES de 256 bits indicará una clave de 32 bytes.
   {: tip} 

6. Utilice el mandato siguiente para crear copias cifradas de los archivos VHD. `target_vhd` representa el nombre del archivo que contiene la versión cifrada de `source_vhd`.

   ```
   vhd-util copy -n <source_vhd> -N <target_vhd> -k <keyfile_name>
   ```
   {: pre}    

   Mandato de ejemplo:

   ```
   vhd-util copy -n debian8-ne.vhd -N debian8-aes512.vhd -k aes512.dek
   ```
   {: screen}

7. Verifique que los archivos VHD estén cifrados mediante el mandato siguiente.

   ```
   vhd-util key -p -n <vhd_filename>
   ```
   {: pre}

   Mandato de ejemplo con salida:

   ```
   vhd-util key -p -n debian8-aes512.vhd
   0000000000000000000000000000000000000000000000000000000000000000
   21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda
   ```
   {: screen}

Si el archivo VHD está cifrado, verá dos valores de hash en la salida tal como se muestra en el ejemplo anterior. El primer hash son todo ceros. El segundo hash es el hash SHA256 que utiliza la clave de cifrado AES para cifrar y descifrar el VHD. Asegúrese de que los hashes SHA256 de los archivos VHD son los mismos que los hashes mostrados en el **Paso 5**.

El mandato de ejemplo del **Paso 7** crea un nuevo archivo VHD cifrado que se denomina “debian8-aes512.vhd”. Está cifrado con la clave de cifrado AES de 512 bits del archivo de claves denominado “aes512.dek”. El hash SHA256 para su cifrado es `21681bba94f04b33b112f5f90a0faa885a6d1dbf1bd68ed16c5b995143088eda`.
