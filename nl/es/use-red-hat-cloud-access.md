---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-04"

subcollection: image-templates

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}


# Mediante su propia licencia de sistema operativo o suscripción

Cuando se crea una plantilla de imagen con una imagen VHD, puede seleccionar proporcionar su propia licencia de sistema operativo RHEL a través de la suscripción a [Red Hat Cloud Access ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) o una licencia de Windows a través del Contrato empresarial de Microsoft.
{:shortdesc}

Si despliega una imagen en {{site.data.keyword.BluSoftlayer_full}} que indica que está utilizando su propia licencia, aparecen los siguientes términos de soporte:
* {{site.data.keyword.IBM_notm}} proporciona soporte para hipervisores, suministro de instancias, importación de imágenes, reinicio de imágenes, captura de imágenes.
* La empresa de la que adquiere la licencia del sistema operativo proporciona un soporte para la imagen en sí misma. {{site.data.keyword.IBM_notm}} no proporciona soporte para la imagen.

Cuando proporciona su propia licencia para una imagen, se aplican las siguientes restricciones a la imagen:
* La imagen es privada. No puede compartirse públicamente.
* La imagen no puede incluir complementos de software. El software adicional debe añadirse después suministrarse la imagen.

## Uso de acceso de Red Hat Cloud
Para obtener más información acerca de nuestra certificación como un proveedor de nube Red Hat Enterprise Linux, consulte [Infraestructura como un servicio (Iaas) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://access.redhat.com/ecosystem/cloud-provider/2262101).

## Cómo utilizar su propia licencia de Windows
Se admiten los siguientes sistemas operativos:
* Windows Server 2016
* Windows Server 2012
* Windows Server 2012 R2

Si tiene preguntas acerca de la elegibilidad de su licencia de Windows existente o entender requisitos de informes, póngase en contacto con el representante de Microsoft. Cuando cree una plantilla de imagen que especifique que está utilizando su propia licencia de Windows, debe suministrar esa imagen en un host dedicado. No puede suministrar una instancia pública o una instancia dedicada que se auto-asigna a un host cuando utiliza una imagen que indica que utiliza su propia licencia de Windows. Adicionalmente, cuando crea o actualiza una plantilla de imagen de Windows que especifica que está utilizando su propia licencia, la imagen de Windows puede no ser una imagen de cloud-init.

## Importación de una imagen que designa su propia licencia

Puede importar una imagen de VHD y especificar que proporciona su propia licencia y suscripción para el sistema operativo.

Para acceder a la página Importar Imagen de las Plantillas de imagen y marcar una imagen de VHD para utilizar su propia licencia o suscripción,
complete los siguientes pasos:
1. Desde el menú **Dispositivos** seleccione **Gestionar > Imágenes**.
2. Pulse el separador **Importar imagen**.
3. Complete la información necesaria para importar su imagen de VHD, y seleccione el recuadro de selección **Su licencia** que se muestra junto al recuadro desplegable **Sistema operativo**. Para obtener más información acerca de la importación de imágenes, consulte [Preparación e importación de imágenes](/docs/infrastructure/image-templates?topic=image-templates-preparing-and-importing-images).

## Actualización de una plantilla de imagen para especificar que un usuario ha proporcionado una licencia de SO

Si tiene una plantilla de VHD existente, puede especificar que desea proporcionar su propia licencia o subscripción para el sistema operativo.

Para acceder a una plantilla de imagen y designar que utilice su licencia o suscripción existente, siga estos pasos:
1. Desde el menú **Dispositivos** seleccione **Gestionar > Imágenes**.
2. En la lista de plantillas, pulse el nombre de la plantilla de imagen que desea actualizar.
3. En la página Detalles de la plantilla de imagen, seleccione el recuadro de selección **Proporcionado por el usuario** en la cabecera
**Licencia de SO**, y pulse **Actualizar**.
