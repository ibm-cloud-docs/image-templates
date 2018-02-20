---

copyright:
  years: 2014, 2018
lastupdated: "2017-10-04"

---


{:new_window: target="_blank"}
{:tip: .tip}

# Preguntas frecuentes: plantillas de imagen

## ¿Qué es una plantilla de imagen estándar?

Una plantilla de imagen estándar es la opción de creación de imágenes de {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.virtualmachineslong}}. Imágenes estándar que permiten a los usuarios capturar una imagen de un servidor virtual existente del servidor virtual independientemente de su sistema operativo y crear un nuevo servidor virtual basado en la imagen.

## ¿Qué es una Plantilla ISO?

La plantilla ISO es un tipo de plantilla que está reservada específicamente para las ISO que pueden utilizarse para arrancar un VSI. Las plantillas ISO están disponibles en dos versiones: públicas y privadas. Las plantillas ISO públicas son plantillas preconfiguradas que se suministran por {{site.data.keyword.BluSoftlayer_notm}} and y pueden utilizar cualquier cliente. Las plantillas ISO privadas se crean importando una imagen de un ISO almacenado en una cuenta de {{site.data.keyword.objectstorageshort}}. Para que una ISO se importe a la pantalla Plantillas de imagen, el ISO debe ser de arranque.

Sólo se pueden utilizar los sistemas operativos admitidos {{site.data.keyword.BluSoftlayer_notm}} para cargar una plantilla ISO en una VSI. Para obtener más información, consulte [Sistemas operativos admitidos
![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.softlayer.com/services/software/).
{:tip}

## ¿Cuál es la diferencia entre imagen pública y privada?

Una imagen pública es una imagen que puede ver y aplicar en un servidor virtual nuevo cualquier usuario {{site.data.keyword.BluSoftlayer_notm}}. {{site.data.keyword.BluSoftlayer_notm}}
crea actualmente imágenes públicas como solución para las opciones de configuración en distintos dispositivos. Los clientes también pueden hacer imágenes públicas y está disponible para cualquier usuario. Una imagen privada es una imagen que pueden ver solo usuarios autorizados. Los usuarios autorizados están predeterminados en su cuenta; sin embargo, las imágenes también pueden compartirse entre diversas cuentas actualizando las opciones que se comparten en {{site.data.keyword.slportal_full}}.

## ¿Puedo capturar y desplegar servidores virtuales con mi hipervisor autogestionado?

Solo los servidores que se suministran por {{site.data.keyword.BluSoftlayer_notm}} pueden capturarse y desplegarse. Los servidores virtuales que ha creado manualmente en dispositivos personales no pueden ser capturados, suministrados, o desplegados.

## ¿Puedo compartir mi Plantilla ISO privada con otros clientes?

Las únicas Plantillas ISO que se publican a todos los clientes son plantillas que se generan por {{site.data.keyword.BluSoftlayer_notm}}. Plantillas ISO privadas son específicas de cuenta y no pueden compartirse entre los clientes a través del {{site.data.keyword.slportal}}.

## ¿Mi plantilla ISO debe estar en el mismo centro de datos que mi servidor virtual para completar un arranque de imagen?

Sí, Las plantillas ISO deben estar en el mismo centro de datos que el servidor virtual para arrancar desde la imagen. Si no está en el mismo centro de datos, la plantilla ISO debe copiarse en el centro de datos adecuado para completar la acción. Si se produce esta situación, aparece un aviso sobre la transacción. Cuando se copia una plantilla ISO un centro de datos diferente, se factura una cuota pequeña a la cuenta de la misma forma en la que se aplican las tarifas para copiar otros tipos de plantillas de imagen.

## ¿Cuál es la diferencia entre los datos físicos y tamaño de volumen?

El volumen es el espacio de disco disponible para almacenar archivos, mientras que los datos físicos son los archivos reales que están almacenados en el disco.

## ¿Qué es la característica de importación/exportación de imágenes?

La característica de importación/exportación de imágenes que se encuentra en la página Plantillas de imagen en {{site.data.keyword.slportal}} permite la conversión de VHD e ISO almacenados en una cuenta de {{site.data.keyword.objectstorageshort}} para convertirse en plantillas de imagen, y viceversa. Cuando importa una imagen, un archivo específico (ya sea VHD o ISO) proviene de un Contenedor de cuenta de [{{site.data.keyword.objectstorageshort}}] esepcificado y se convierte en plantilla de imagen. La plantilla de imagen puede utilizarse para arrancar o cargar un dispositivo. Cuando exporta una imagen, la plantilla de la imagen se convierte en archivo (o distintos archivos si la plantilla tiene diversos discos) que se almacena en una ubicación específica en el Contenedor de cuenta de {{site.data.keyword.objectstorageshort}}. 


