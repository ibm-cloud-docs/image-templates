---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-15"

keywords:

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}


# Creación una plantilla de imagen
{: #creating-an-image-template}

Con las plantillas de imagen, puede replicar varias opciones de configuración para {{site.data.keyword.virtualmachinesshort}}.
{:shortdesc}

En cualquier momento durante la vida de un servidor virtual, puede crear una plantilla de imagen. A continuación, puede utilizarla para duplicar rápidamente partes de su configuración en otro servidor virtual. Puede crear plantillas de imagen desde cualquier servidor virtual, independientemente de su sistema operativo. Cuando se haya completado la plantilla de imagen, puede utilizarla para crear otro servidor virtual.

Complete los pasos siguientes para crear una plantilla de imagen de un servidor virtual.

1. Acceda al [{{site.data.keyword.slportal_full}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/){: new_window} utilizando sus credenciales exclusivas.
2. Desde el menú **Dispositivos**, seleccione **Lista de dispositivos**.
3. Pulse el servidor virtual que desea utilizar para crear una plantilla de imagen.

  Compruebe el separador **Contraseñas** de la página **Detalles de dispositivo**. Asegúrese de que las contraseñas listadas en la página **Detalles de dispositivo** coincidan con las contraseñas del sistema operativo real y con cualquier otra contraseña de complemento de software. Si las contraseñas no coinciden, los servidores virtuales que se crean a partir de esta plantilla de imagen fallan.
  {:tip}

4. Desde el menú **Acciones**, seleccione **Crear plantilla de imagen**.
5. Especifique el nuevo nombre para la imagen en el campo **Nombre de imagen**.
6. Introduzca las notas necesarias para la imagen en el campo **Nota**.
7. Seleccione el recuadro de selección **Acepto** cuando se haya especificado toda la información.
8. Pulse **Crear plantilla** para crear la plantilla de imagen.

## Siguientes pasos

Después de que se cree la plantilla de imagen, pueden crearse más servidores virtuales mediante la plantilla. Los nuevos servidores virtuales tienen la misma configuración que se incluye en la plantilla de imagen.
