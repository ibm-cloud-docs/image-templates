---

copyright:
  years: 2018
lastupdated: "2018-09-19"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}


# Creación de una imagen cifrada

Como parte de la característica de cifrado E2E, puede cifrar una imagen para importarla en Plantillas de imagen y utilizarla para desplegar una instancia de servidor virtual cifrada.
{:shortdesc}

## Requisitos de imagen cifrada
{: #encrypted-image-reqs}

Para crear una imagen cifrada, esta debe cumplir los requisitos de imagen siguientes:

* Que la imagen sea compatible con el entorno de infraestructura de la consola de {{site.data.keyword.cloud}}.
* Que la imagen incluya un sistema operativo Linux, como CentOS, Debian, Red Hat Enterprise Linux o Ubuntu.
* Que la imagen está habilitada para cloud-init.
* Que la imagen esté cifrada con el [cifrado de disco LUKS](/docs/infrastructure/image-templates?topic=image-templates-creating-an-encrypted-image#luks-disk-encryption).

## Utilización de QEMU y DM-Crypt para crear una imagen RAW cifrada
{: #luks-disk-encryption}

Para encriptar la imagen, debe convertir un archivo de imagen VHD fija en formato RAW. A continuación, utilice QEMU y DM-Crypt para crear un nuevo archivo de imagen con cifrado de disco LUKS. Monte el archivo como un volumen cifrado y copie el archivo de imagen sin cifrar en el volumen cifrado.

Este procedimiento le guía a través de las siguientes tareas:

* Convertir la imagen VHD dinámica en una imagen VHD fija.
* Convertir la imagen VHD fija en formato de archivo RAW.
* Crear un archivo de claves de cifrado de datos que vaya a utilizar para cifrar la unidad.
* Crear un nuevo archivo RAW para contener la imagen y la cabecera de cifrado LUKS.
* Dar formato al archivo RAW con cifrado de disco LUKS.
* Adjuntar el archivo RAW cifrado a un dispositivo de bloque.
* Copiar la imagen no cifrada en el dispositivo de volumen cifrado.

Debe tener el privilegio de ejecutar mandatos utilizando la autorización de usuario `root`, a través de sudo, para montar y desmontar los volúmenes cifrados en el sistema. Puede emitir el mandato siguiente para verificar los privilegios de usuario `root`:

```
sudo echo "Hello!"
```
{: pre}

Debe recibir "Hello!" como respuesta para completar esta tarea de cifrado.

**Sugerencia**: debe completar esta tarea de cifrado en un sistema que esté ejecutando un sistema operativo Linux y tenga los paquetes siguientes disponibles:
* qemu o qemu-image (en función del sistema operativo Linux)
* cryptsetup

Complete los pasos siguientes para cifrar la imagen.

1. Convierta el archivo de imagen VHD dinámica en un archivo de imagen VHD fija. Un archivo VHD fijo evita la corrupción que podría producirse si un VHD dinámico se convierte directamente a formato de archivo RAW. Ejecute el mandato siguiente para convertir el archivo de imagen VHD dinámica en un archivo de imagen VHD fija:

  ```
  qemu-img convert -p -O vpc -o subformat=fixed <DYNAMIC_VHD_FILE> <FIXED_VHD_FILE>
  ```
  {: pre}

  Por ejemplo, si el archivo VHD dinámico es _Rhel_7.vhd-0.vhd_ y desea que el archivo VHD fijo se denomine _Rhel_7.fixed.vhd-0.vhd_, ejecute el mandato siguiente:

  ```
  $ qemu-img convert -p -O vpc -o subformat=fixed Rhel_7.vhd-0.vhd Rhel_7.fixed.vhd-0.vhd
  ```
  {: screen}

  Este mandato puede tardar 30 minutos o más en completarse.
  {: tip}

2. Convierta la imagen VHD fija en formato de archivo RAW mediante QEMU. La imagen debe estar en formato de archivo RAW antes de que pueda cifrarla. Ejecute el mandato de QEMU siguiente:

  ```
  qemu-img convert -p -O raw <FIXED_VHD_FILE> <RAW_IMAGE_FILE>
  ```
  {: pre}

  Por ejemplo, si el archivo VHD fijo es _Rhel_7.fixed.vhd-0.vhd_ y desea denominar _Rhel_7.raw-0.raw_ al archivo de salida RAW, ejecute el mandato siguiente:

  ```
  qemu-img convert -p -O raw Rhel_7.fixed.vhd-0.vhd Rhel_7.raw-0.raw
  ```
  {: screen}

  Este mandato puede tardar 30 minutos o más en completarse.
  {: tip}

3. Identifique la clave de cifrado de datos que va a utilizar para cifrar y descifrar la unidad. Esta clave de cifrado de datos es la misma clave que se envuelve y especifica cuando se importa la imagen cifrada a {{site.data.keyword.slportal}}. Cree un archivo que contenga la clave de cifrado de datos que utilizará para cifrar y descifrar la unidad. En este archivo, la clave debe estar desenvuelta y en texto codificado en base64. Base64 permite asegurarse de que no se incluyen espacios o saltos accidentales. La clave de cifrado de datos codificada en base64 debe tener un mínimo de 32 caracteres o bytes y un máximo de 512 caracteres o bytes. El material de la clave de cifrado de datos debe estar en una línea sin saltos de línea ni líneas nuevas. Por ejemplo, utilice el mandato siguiente para crear un archivo llamado `secret.dek` para almacenar el `unwrapped_key_material` para la clave de cifrado de datos y codificarlo con base64:

  ```
  $ echo -n $(echo 'unwrapped_key_material' | base64) > secret.dek
  ```
  {: screen}

  Mantenga esta clave a buen recaudo. Si pierde esta clave, no podrá descifrar el disco.
  {: tip}

4. Identifique el tamaño correcto del nuevo archivo RAW que se va a crear en el paso siguiente para el disco cifrado, que representa la adición de una cabecera LUKS. El nuevo archivo RAW será utilizado por _dmcrypt_ y _cryptsetup_ para mantener el contenido del disco en un formato LUKS cifrado. El tamaño del nuevo archivo RAW debe ser la suma del tamaño del archivo RAW creado en el paso 2 y una cabecera LUKS de 4 MB constante. Para determinar el tamaño de la imagen RAW existente, ejecute el mandato siguiente, donde _Rhel_7.raw-0.raw_ es el nombre de la imagen:

  ```
  ls -l Rhel_7.raw-0.raw
  ```
  {: pre}

  Se muestra una salida similar a la siguiente:

  ```
  -rw-r--r--. 1 user user 42949017600 Mar 5 10:02 Rhel_7.raw-0.raw
  ```
  {: screen}

  Donde _42949017600_ es el número de bytes que está utilizando la imagen RAW
      1. Añada 4 MB (convertidos a bytes) al tamaño de archivo de imagen RAW para tener en cuenta la cabecera LUKS. Por ejemplo, 42949017600 + (4 x 1024 x 1024) = 42953211904 bytes.
      2. Convierta la suma del tamaño de la imagen RAW y la cabecera LUKS a megabytes y redondee hasta el siguiente megabyte. Por ejemplo, 42953211904 bytes/1024 bytes/1024 kilobytes = 40963,375 megabytes = **40964 MG**.

5. Cree un nuevo archivo RAW con el número correcto de bytes que se convertirán en la imagen RAW cifrada. Utilice el mandato _dd_ siguiente para crear el archivo RAW:

  ```
  dd if=/dev/zero of=<ENCRYPTED_RAW_FILENAME> bs=1M count=<TARGET_SIZE_IN_MEGABYTE> status=progress
  ```
  {: pre}

  Donde _ENCRYPTED_RAW_FILENAME_ es el archivo que se convertirá en la imagen RAW cifrada y _TARGET_SIZE_IN_MEGABYTE_ es el número obtenido en el paso 4, el tamaño de la imagen RAW no cifrada más la cabecera LUKS.

  Por ejemplo, si desea que el nuevo archivo RAW que se convertirá en la imagen cifrada sea _Rhel_7.encrypted.raw_, y su tamaño de destino es _40964_ MB, ejecute el mandato siguiente:

  ```
  $ dd if=/dev/zero of=Rhel_7.encrypted.raw bs=1M count=40964 status=progress
  ```
  {: screen}

6. Formatee el archivo RAW con el cifrado de disco de LUKS utilizando _dmcrypt_ y la clave de cifrado de datos (del paso 3). En este paso se prepara el archivo para que contenga la imagen. Para formatear el archivo con cifrado LUKS, ejecute el siguiente mandato _cryptsetup_:

  ```
  cryptsetup -v luksFormat <ENCRYPTED_RAW_FILENAME> <DEK_FILENAME>
  ```
  {: pre}

  Donde _ENCRYPTED_RAW_FILENAME_ es el nombre de archivo del archivo RAW que desea cifrar y _DEK_FILENAME_ es el nombre del archivo en el que se almacena la clave de cifrado de datos.

  Por ejemplo, si el archivo RAW es _Rhel_7.encrypted.raw_ y el archivo de claves es _secret.dek_, especifique:

  ```
  $ cryptsetup -v luksFormat Rhel_7.encrypted.raw secret.dek
  ```
  {: screen}

  Tras ejecutar el mandato cryptsetup, recibirá la solicitud siguiente. Se espera esta solicitud y debe responder con SÍ para continuar.

  ```
  WARNING!
  ========
  This will overwrite data on Rhel_7.encrypted.raw irrevocably.
  Are you sure (Type uppercase yes): YES
  ```
  {: screen}

7. Adjunte el archivo RAW cifrado como un volumen para asociar un nuevo dispositivo de bloque al sistema operativo. Mediante el uso de _cryptsetup_, ejecute el mandato siguiente para abrir el dispositivo RAW cifrado utilizando la opción luksOpen.

  Debe tener el privilegio de sudo para ejecutar el mandato siguiente utilizando la autorización de usuario `root`.
  {: tip}

  ```
  sudo cryptsetup -v luksOpen <ENCRYPTED_RAW_FILENAME> <VOLUME_NAME> --key-file <DEK_FILENAME>
  ```
  {: pre}

  Donde _ENCRYPTED_RAW_FILENAME_ es el nombre de archivo del archivo RAW cifrado, _VOLUME_NAME_ es el nombre del dispositivo que _dmcrypt_ intentará crear y _DEK_FILENAME_ es el nombre de archivo de la clave de cifrado de datos.

  Por ejemplo, si el archivo RAW cifrado es _Rhel_7.encrypted.raw_, desea nombrar el dispositivo de bloque _encryptedVolume_ y el archivo de claves de cifrado de datos es _secret.dek_, utilice el mandato siguiente:

  ```
  $ sudo cryptsetup -v luksOpen Rhel_7.encrypted.raw encryptedVolume --key-file secret.dek
  ```
  {: screen}

  Cuando este paso está completado, se crea un nuevo dispositivo de bloque con el que puede leer y escribir en _/dev/mapper/_.

8. Confirme que el correlacionador de volúmenes LUKS se ha creado correctamente ejecutando el mandato siguiente:

  ```
  ls -l /dev/mapper/
  ```
  {: pre}

  Se debería mostrar una salida similar al ejemplo siguiente:

  ```
  total 0
  lrw-rw---- 1 root root        7 Mar 5 22:22 encryptedVolume -> ../dm-0
  ```
  {: screen}

9. Copie la imagen no cifrada (del paso 2) en el dispositivo de volumen cifrado (del paso 7). Ejecute el mandato _dd_ siguiente para copiar la imagen no cifrada en el volumen cifrado:

  ```
  sudo dd if=<RAW_IMAGE_FILE> of=/dev/mapper/<VOLUME_NAME>
  ```
  {: pre}

  Donde _RAW_IMAGE_FILE_ es el nombre de archivo de la imagen RAW no cifrada (creada en el paso 2) y _VOLUME_NAME_ es el nombre del nombre de dispositivo de bloque de volumen cifrado (creado en el paso 7).

  Por ejemplo, si RAW_IMAGE_FILE se denomina _Rhel_7.raw-0.raw_ y VOLUME_NAME es _encryptedVolume_, ejecute el mandato siguiente:

  ```
  $ sudo dd if=Rhel_7.raw-0.raw of=/dev/mapper/encryptedVolume status=progress
  ```
  {: screen}

  Este mandato puede tardar 30 minutos o más en completarse.
  {: tip}

10. Destruya el correlacionador de volúmenes y cierre la conexión LUKS con el archivo de datos cifrado:

  ```
  sudo cryptsetup luksClose encryptedVolume
  ```
  {: pre}

  Donde _encryptedVolume_ es el nombre del dispositivo de bloque de volumen cifrado.

  El _ENCRYPTED_RAW_FILENAME_ se ha inicializado y puede subirlo a IBM Cloud Object Storage. Por ejemplo, si el archivo RAW cifrado es _Rhel_7.encrypted.raw_, cargue dicha imagen en IBM Cloud Object Storage.
