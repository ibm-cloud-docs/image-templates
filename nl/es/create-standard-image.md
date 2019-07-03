---

copyright:
  years: 2014, 2018
lastupdated: "2019-06-11"

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

## Antes de empezar
En primer lugar, acceda al menú de dispositivo y asegúrese de que tiene los permisos de cuenta correctos para completar las tareas.

* Acceda al menú del dispositivo de la consola. Para obtener más información, consulte
[Navegación a dispositivos](/docs/infrastructure/image-templates?topic=virtual-servers-navigating-devices).
* Asegúrese de que tiene los permisos de cuenta y el acceso de dispositivo necesarios. Solo el propietario de cuenta o un usuario con el permiso de infraestructura clásico **Gestionar usuarios**
puede ajustar los permisos.

Para obtener más información sobre los permisos, consulte [Permisos clásicos de infraestructura](/docs/iam?topic=iam-infrapermission#infrapermission) y [Gestión de acceso a dispositivos](/docs/vsi?topic=virtual-servers-managing-device-access).

## Creación una plantilla de imagen

Complete los pasos siguientes para crear una plantilla de imagen de un servidor virtual.

1. Desde el menú **Dispositivos**, seleccione **Lista de dispositivos**.
2. Pulse el servidor virtual que desea utilizar para crear una plantilla de imagen.

  Compruebe el separador **Contraseñas** de la página **Detalles de dispositivo**. Asegúrese de que las contraseñas listadas en la página **Detalles de dispositivo** coincidan con las contraseñas del sistema operativo real y con cualquier otra contraseña de complemento de software. Si las contraseñas no coinciden, los servidores virtuales que se crean a partir de esta plantilla de imagen fallan.
  {:tip}

3. Desde el menú **Acciones**, seleccione **Crear plantilla de imagen**.
4. Especifique el nuevo nombre para la imagen en el campo **Nombre de imagen**.
5. Introduzca las notas necesarias para la imagen en el campo **Nota**.
6. Seleccione el recuadro de selección **Acepto** cuando se haya especificado toda la información.
7. Pulse **Crear plantilla** para crear la plantilla de imagen.

## Siguientes pasos

Después de que se cree la plantilla de imagen, pueden crearse más servidores virtuales mediante la plantilla. Los nuevos servidores virtuales tienen la misma configuración que se incluye en la plantilla de imagen.
